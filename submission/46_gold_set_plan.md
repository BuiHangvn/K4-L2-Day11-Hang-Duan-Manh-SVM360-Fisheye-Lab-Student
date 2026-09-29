# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide, **không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có normal và hard slice. "Gold set" ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference ADASIND hoặc nhãn bạn vừa vẽ.

## 1. Ma trận hard case theo camera

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| **front** | (a) Ngược nắng cuối ngày phía trước; (b) xe cắt ngang giữa làn ở ngã tư; (c) người/xe đạp qua đường; (d) đêm có đèn xe khác chạy ngược chiều; (e) mưa che tầm nhìn | Camera trước là input cho phanh khẩn cấp — recall Pedestrian/Bike phải cao nhất; nắng chói làm mất box; xe cắt ngang qua zone center→edge nhanh nên box lệch geometry; đêm phá recall và confuse class Car↔Truck do chỉ thấy đèn | Intrinsic + extrinsic calibration đầu xe; annotation space trên **fisheye gốc** (không undistort trước gán); ngưỡng H=40 giữ nguyên; ego_body có thể ôm capo (khác auto-rickshaw ADASIND) | 2 annotator độc lập vẽ + 1 QA đối chiếu; ca bất đồng escalate cho Lab Coach mở ảnh phóng to; chỉ gọi gold khi 3 người khớp về class + geometry IoU≥0.7 + attribute (`truncated`, `occluded`, `edge_zone`) |
| **rear** | (a) Người đi ngang gần khi lùi; (b) chướng vật thấp (cọc, thùng, xe đạp dựng); (c) đêm chỉ có đèn phanh trắng/đỏ; (d) trẻ em/thú cưng thấp gần bumper; (e) reverse trong bãi đỗ với nhiều xe khác | Hậu quả lỗi FN khi lùi có thể va chạm — recall Pedestrian ≥95% là bắt buộc; vật thấp <H=40 bị bỏ theo R01 nhưng vẫn có tác động thực (cần bàn với `guideline` về ngưỡng riêng cho rear); đèn phanh trắng gây confusion Car↔Truck theo hình dáng ánh sáng | Calibration đuôi xe (có thể khác front vì góc gắn nghiêng xuống); vùng ego_body bao cản sau/gầm nếu có, khác front (capo); có thể có bảng số xe ego lấy vào frame cần blur | Tương tự front nhưng thêm 1 reviewer chuyên về ca đêm; tiêu chí gold: recall Pedestrian ≥95% trên bộ mẫu này; đo lại nếu có tuyết/mưa lớn |
| **left** | (a) Motor/xe đạp len giữa 2 làn khi rẽ trái; (b) seam với front-left và rear-left ở góc xe (1 vật xuất hiện ở 2-3 camera); (c) blind spot khi chuyển làn; (d) rickshaw len giữa (đông ở Đông Nam Á); (e) vạch chia làn hoặc curb sát | Motor vào blind spot bị model bỏ ở vùng edge camera trái; seam có 2 box cho cùng vật nếu chưa có policy → dễ bị coi là `DUPLICATE` sai; văn hoá giao thông Ấn Độ/VN có nhiều motor chen ngang; camera side thường có gương chiếu hậu ego vào frame | **Timestamp đồng bộ ≤50ms** giữa 3 camera (front-left, left, rear-left) để đối chiếu vùng seam; ego_body có thể có tay lái/gương/thân xe tuỳ rig; ngưỡng `edge_zone=true` ở góc chồng camera cần đánh dấu để reviewer biết đây là ca seam | Reviewer cần đối chiếu **cả 3 camera trong cùng timestamp**; ca seam đánh dấu ưu tiên; policy cross-camera phải quyết trước (giữ 2 box + shared_id vs merge sau BEV) trước khi gọi gold |
| **right** | (a) Người/xe rẽ vào ngã ba từ bên phải; (b) seam với front-right/rear-right; (c) curb gần khi đỗ; (d) hàng người tĩnh ở lề (chợ, trường học); (e) xe máy vượt phải (illegal nhưng phổ biến) | Motor rẽ vào từ bên phải là ca TP muộn (model phát hiện chậm gây phanh gấp); curb sát gây confusion với ego_body; hàng người tĩnh cần `crowd_or_group` ignore hay tách từng box (đang là gap của rule hiện tại); side phải thường ít observation hơn side trái → dữ liệu training có thể lệch | Tương tự left, cần timestamp đa camera; đặc biệt vùng thân xe bên phải có thể có gương hoặc tay lái phụ; policy về `crowd_or_group` cần Lab Coach quyết ngưỡng bao nhiêu người thì gộp | Tương tự left; thêm ca `crowd_or_group` policy để reviewer thống nhất trước khi split thành box lẻ; ưu tiên ca rẽ ngang vào từ ngã ba (TP muộn nguy hiểm) |

## 2. Refresh policy — khi nào cần làm lại gold set

- **Refresh toàn bộ** (chọn lại 200 frame mới, gold từ đầu):
  - (a) Thay camera/thay firmware → zone bán kính và ngưỡng `edge_zone` đổi, ảnh trước không tương thích
  - (b) Đổi vị trí gắn camera (rig change) → ego_body scope đổi hoàn toàn
  - (c) Đổi lens (đổi hệ số méo fisheye) → geometry L trước lệch nghiêm trọng
- **Refresh partial** (chỉ subset 10-15% mẫu):
  - (a) Rule R04 hoặc R07 thay đổi mapping class → review lại các ca liên quan class đó
  - (b) 6 tháng 1 lần refresh để bắt drift dữ liệu (thay đổi giao thông, biển báo mới, xe mẫu mới ra thị trường, thay đổi văn hoá đô thị)
  - (c) Khi có ≥3 ticket escalation cùng loại từ annotator → refresh mẫu class đó
- **Không cần refresh** nếu:
  - (a) Chỉ sửa attribute soát tay (không đổi rule)
  - (b) Bump minor version `rules_version` với thêm ví dụ (không đổi ngữ nghĩa) — nhưng phải thông báo lớp

## 3. Ca seam / cross-camera cần policy và evidence trước khi ghép box

**Kịch bản cụ thể**: motor rẽ trái ở ngã ba xuất hiện ở cả camera left (zone edge phải) và front (zone edge trái) trong cùng timestamp T. Có 2 box khác nhau, khác kích thước, khác zone. Không được ghép thành 1 track hay xoá 1 box chỉ dựa vào IoU trên BEV.

**Cần 3 điều trước khi quyết**:
1. **Timestamp khớp ≤50ms** giữa 2 camera (chứng minh vật quan sát cùng lúc, không phải trước/sau)
2. **Calibration extrinsic verified** — chiếu box về cùng hệ toạ độ (world frame hoặc BEV) với sai số ≤10cm ở khoảng cách 5m
3. **Policy đầu ra của hệ downstream đã quyết**:
   - (a) "keep both với flag `same_object_id`" — annotator vẽ 2 box, đánh cùng track_id, downstream tự xử lý
   - (b) "merge sau BEV homography" — 1 box duy nhất sau khi chiếu về BEV, annotator vẫn vẽ 2 box gốc để phục vụ training
   - (c) "keep box có IoU cao hơn với ground plane" — chỉ giữ 1 box, xoá box còn lại

**Trước khi có 3 điều này**: ghi 2 box riêng biệt, KHÔNG tự nối track, KHÔNG dùng `DUPLICATE` (DUPLICATE chỉ áp dụng trên **cùng 1 ảnh**, không phải cross-camera). Escalate cho Lab Coach + `ai_team` để quyết policy.

## 4. Vì sao peer agreement hoặc quality report trên ảnh MỘT camera chưa chứng minh gold set đúng cho CẢ BỐN camera

1. **Rig differences per camera**: mỗi camera có góc gắn/lens/vị trí ego_body khác nhau → luật vẽ có thể khác về ngưỡng `edge_zone`, phạm vi ego_body, và cách đọc `truncated` (front bị lens_border cắt vs side bị gương cắt là hai lý do khác nhau nhưng cùng attribute).
2. **Class distribution differences**: front nhiều xe/biển báo/giao thông; rear ít vật động (chủ yếu tĩnh khi lùi); side nhiều motor/rickshaw/người đi bộ. Guideline calibrated trên 1 camera không cover distribution class khác.
3. **Fisheye distortion differences**: nếu dùng khác lens (front dùng lens fov 180°, side dùng 195°) thì hệ số méo khác, box "tight" theo ngưỡng IoU khác.
4. **Peer agreement 2-3 người trên ADASIND** chỉ chứng minh **nhất quán về guideline** giữa 3 annotator — không chứng minh guideline **đúng** cho các condition edge (đêm, mưa, tuyết, ngược sáng) hoặc các cảnh chưa gặp (highway tốc độ cao, hầm, đô thị dày đặc).
5. **3 frame ADASIND không đại diện** được các cảnh downtown/highway/parking đa dạng của rig 4 camera thật; tỷ lệ Bike/ThreeWheeler trên ADASIND cao hơn production đa vùng.
6. **Không có ground truth về seam/tracking cross-camera** trong ADASIND — không thể verify chính sách nối/tách box giữa các camera dù đã calibrate được guideline single-camera.

**Kết luận**: Gold set cho 4 camera cần vòng review độc lập cho **từng camera**, chứ không phải extrapolate từ kết quả 1 camera. Chi phí gấp 4 lần nhưng không thể tắt.
