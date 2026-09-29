# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling đã nằm trong file tương ứng nên không hỏi lại ở đây.

## 1. Vật ở vùng seam — `DUPLICATE` hay quy tắc riêng?

Cần **quy tắc riêng**, KHÔNG phải `DUPLICATE`.

**Vì sao không phải DUPLICATE**:
- `DUPLICATE` theo taxonomy `docs/05` được định nghĩa cho hai box trên **cùng một ảnh** trỏ vào cùng một vật — thường là annotator lỡ tay vẽ 2 lần, hoặc auto-label output 2 box overlap
- Vật ở vùng seam xuất hiện trên **hai ảnh khác nhau** từ hai camera có góc nhìn khác nhau — đó là quan sát **hợp lệ theo góc riêng** của mỗi camera, không phải lỗi
- Hai box này có thể khác class attribute (`truncated` ở camera này do lens_border, không `truncated` ở camera kia; `edge_zone=true` ở camera này, `false` ở camera kia), khác kích thước box (do vật ở gần camera này hơn), khác zone (edge camera A, mid camera B)
- Coi seam là `DUPLICATE` sẽ ép annotator phải xoá 1 box → mất dữ liệu training cho camera bị xoá

**Quy tắc riêng cần quyết trước khi coi là 1 vật (không phải 2)**:
1. **Timestamp đồng bộ**: 2 box cùng thời điểm ≤50ms — nếu lệch ≥1 frame thì có thể là vật khác nhau di chuyển
2. **Extrinsic calibration**: chiếu 2 box về hệ toạ độ chung (world hoặc BEV) — nếu overlap ≥50% ground plane thì cùng vật
3. **Chính sách output đã quyết** ở tầng downstream:
   - (a) `keep_both_shared_id` — annotator vẽ 2 box, đánh cùng `track_id`, downstream fusion tự xử lý — tốt cho training multi-view
   - (b) `merge_after_bev_homography` — 1 box duy nhất sau chiếu BEV, annotator vẫn vẽ 2 box gốc để phục vụ training model per-camera
   - (c) `keep_higher_iou_ground_plane` — chỉ giữ 1 box có IoU cao hơn với ground plane, xoá box còn lại — tiết kiệm nhưng mất dữ liệu multi-view

Chưa có 3 điều này thì ghi 2 box độc lập với `note: possible_seam_pair` và **KHÔNG** dùng `DUPLICATE`. Escalate cho Lab Coach + `ai_team` để quyết policy nội bộ.

## 2. Track qua nhiều frame — cùng track ID, keyframe, Outside khi nào?

### Trong cùng camera (single-camera tracking)

- **Giữ cùng `track_id`** khi:
  - Vật quan sát liên tục (không bị che hoàn toàn ≥2 frame)
  - Class không đổi (nếu class chuyển từ Bike sang Pedestrian vì rider xuống xe → **tách track** theo R03)
  - Còn nằm trong khung hình (chưa qua `lens_border` hoặc `ego_body`)
- **Thêm keyframe** khi:
  - Hình học thay đổi lớn — box nhích ≥15-20% kích thước hoặc quay ≥30° so với frame trước
  - Attribute thay đổi (`occluded=false → true` khi vật khác đi qua che một phần)
  - Annotator cần đánh dấu để interpolation không lệch — không được để tool tự interpolate qua khoảng có thay đổi lớn
- **Chuyển sang trạng thái Outside** khi:
  - Vật rời khỏi trường nhìn (ra ngoài `lens_border` hoặc bị `ego_body` che hoàn toàn) ≥2-3 frame liền
  - Track vẫn giữ ID nhưng frame không có box — nếu vật quay lại có thể resume cùng ID nếu đủ evidence
  - Không dùng Outside cho vật vẫn thấy được nhưng nhỏ hơn H=40 — cái đó là **kết thúc track** (nếu không quay lại) hoặc **giữ track không box với reason** (nếu chờ vật lớn lên)

### Bằng chứng cần trước khi nối track qua HAI camera

Không đủ 1 trong 5 điều dưới → **không nối**, ghi 2 track riêng và note lý do:

1. **Timestamp đồng bộ ms-level** giữa 2 camera — nếu 2 camera capture khác thời điểm thì có thể là 2 vật khác nhau
2. **Calibration extrinsic đã verify gần đây** — không phải calibration ban đầu chưa refresh (calibration drift theo thời gian do rung động, va chạm)
3. **Vật vào seam camera A và ra seam camera B trong cửa sổ thời gian hợp lý** theo vận tốc ước lượng (ví dụ motor 30km/h qua vùng chồng 1m mất ~120ms — nếu 2 camera thấy vật cách nhau 5s thì không phải cùng vật)
4. **Class + attribute nhất quán** giữa 2 lần thấy — nếu camera A thấy Bike và camera B thấy Pedestrian thì có thể là rider xuống xe, không phải cross-camera match
5. **Policy đầu ra của hệ downstream** đã quyết — nếu output per-camera thì không nối; nếu output BEV/world frame thì mới nối theo shared_id; policy phải nằm trong document official, không quyết ad-hoc

**Chú ý về appearance similarity**: KHÔNG đủ dựa vào giống nhau về màu/hình dáng để nối track — vì (a) 2 xe cùng model có thể giống nhau; (b) hai lần thấy cùng vật ở 2 camera có góc khác nhau nên appearance khác nhau tự nhiên.

## 3. Tự nhìn lại — ca tin nhãn mình nhưng sai vs reference

### Ca cụ thể

`adasind_167700.jpg` object `L5` — box `Pedestrian` (906, 723)-(1080, 1420), truncated=true. Ban đầu mình tin đây là **người thật** đứng bên phải xe vì:
- Nhìn ảnh thấy hình dáng người mờ mờ
- Đã tick `truncated=true` khi self-QC nhắc "box chạm mép ảnh"
- Không có cảnh báo tự động nào ở P2 (self-QC chỉ warn thiếu ego_body, không warn class sai)

Nhưng ở **P4 khi mở reference** (`data/_ref/B3-dense.xml`), compare cho thấy vùng này thuộc polygon `ignore_region` reason=`ego_body` của ref: (922, 747)-(1077, 1443). Nghĩa là đây là **người ngồi cùng camera** (passenger bên phải trên xe auto-rickshaw ADASIND), không phải người đứng ngoài xe.

### Đã xử lý thế nào

- Ghi finding `r1_craft, 167700, L5, IGNORE_SCOPE, E1_annotator_error, P0, annotator, R07, action=rework` vào `submission/findings.csv`
- Ghi decision log `D01, 2026-09-29, "adasind_167700.jpg L5 Pedestrian góc phải cao là ego_body hay người thật?", "Xoá box và thêm polygon ignore_region ego_body", status=resolved`
- Ở P5 rework: xoá box Pedestrian L5 khỏi CVAT + vẽ polygon `ignore_region` reason=`ego_body` mới ở vùng phải đáy (1080, 791)-(1080, 1395) với ~30 điểm bám thân người
- Kết quả: sau rework, zone center spurious giảm 2→0 (`submission/rework/delta.md`)

### Nếu làm lại slice này, mình sẽ đổi gì

1. **Vẽ TẤT CẢ polygon ego_body TRƯỚC TIÊN** khi mở mỗi frame, không phải cuối cùng. Thứ tự: (a) mở frame; (b) nhìn 30 giây tìm mọi vị trí có thân người ngồi cùng camera (không chỉ đáy trái mặc định — check cả đáy phải, capo nếu ô tô); (c) vẽ hết polygon ego_body với reason đúng; (d) MỚI SAU ĐÓ vẽ box class object. Như vậy không bị confuse ego passenger với person thật đứng ngoài.
2. **Đọc lại R07 trước khi bắt đầu mỗi slice** — không nghĩ "ego_body luôn ở đáy trái" (đúng cho ô tô con nhưng SAI cho auto-rickshaw có 2 passenger 2 bên, hoặc xe tuk-tuk chở nhiều người)
3. **Sau khi vẽ polygon `ignore_region`, tĩnh tâm 30 giây** rồi mới vẽ box class object — tránh confuse label giữa `ignore_region` và class object khi tool CVAT có dropdown label. Ở C0 mình đã vẽ Bike vào vùng ego (nhầm label), lỗi này lặp lại ở P2 khi vẽ Pedestrian vào vùng ego passenger phải 167700 — cả 2 đều do rush chọn label.
4. **Self-QC checklist manual cần đứng TRƯỚC** khi bắt đầu vẽ, không phải sau. Cụ thể checklist "vẽ ego_body cho MỖI vị trí có thân người trước khi vẽ box" phải là **bước 0**, không phải bước cuối. Lỗi C0 (quên ego_body) lặp lại ở P2 (thiếu ego_body phải 167700) cho thấy checklist reactive không loại được lỗi này.
5. **Với ca ambiguous** (không rõ ego hay object): không tự quyết trong P2, mà **note lại** và escalate ở P4 khi có reference/model để đối chiếu — tránh commit sai class vào bản khoá.
