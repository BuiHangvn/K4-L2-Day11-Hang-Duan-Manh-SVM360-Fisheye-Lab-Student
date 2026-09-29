# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

## 1. Hai lát cắt cần review trước

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| **Lát cắt A · Class Truck vs ThreeWheeler cho auto-rickshaw** (spans C0 + 145860 + 167700 + 199770) | 4 `WRONG_CLASS` với `rule_id=R04`: (1) C0 L5+R3 auto-rickshaw vàng nhỏ → gán Car; (2) 167700 L4+R6 auto-rickshaw vàng lớn → gán Truck; (3) 199770 L6+R8 rickshaw đỏ có mái → gán Truck; (4) r2_qa 145860 L2 nghi vật vàng cam → đang là Truck. Confusion matrix local quality: ThreeWheeler→Truck 2/4 = 50% confusion. Recall ThreeWheeler = 0.25 — thấp nhất 5 class. | Lỗi mapping rule R04 lặp lại **4 lần trên 4 frame khác nhau + 2 slice** (C0 và B3-dense) — pattern quá rõ để đổ hoàn toàn cho annotator error đơn lẻ. Kết hợp E1 + E2 (guideline_gap): rule chỉ mô tả bằng chữ, thiếu ảnh minh hoạ crop rickshaw có mái vs Truck. Nếu không sửa cả 2 (annotator + guideline) thì cohort tiếp theo sẽ tái phát y hệt. | (a) `submission/findings.csv` — 4 dòng có `rule_id=R04`; (b) `submission/r3_diag/local_quality_confusion.csv` — hàng ThreeWheeler; (c) `submission/20_guideline_patch.md` — đề xuất R04.1 với cây quyết định + ảnh crop; (d) ảnh crop rickshaw từ 3 frame để làm training data cho clinic |
| **Lát cắt B · Ego_body scope ở khung fisheye có passenger bên phải** (C0 + 167700 + 199770) | 3 ca `IGNORE_SCOPE` mức P0 hoặc SPURIOUS liên quan ego: (1) C0 L6 SPURIOUS Bike ở vùng ego trái + thiếu polygon ego_body ban đầu (E1 R07); (2) 167700 L5 IGNORE_SCOPE Pedestrian (906,723)-(1080,1420) — ego passenger phải bị gán nhầm class; (3) 199770 L4 IGNORE_SCOPE Truck (928,796)-(1080,1310) — ego passenger phải bị gán Truck. Cả 3 đều mức P0 theo R10 (sai phạm vi dữ liệu). | Sai R07 mức P0 là mức cao nhất trong bài lab — sai phạm vi làm hỏng mọi compare/local-quality/model đo tiếp theo. Frame 167700 và 199770 đều có passenger 2 bên (không chỉ trái), khác C0 chỉ có 1 bên. Annotator lần đầu dễ nghĩ ego chỉ ở 1 vị trí; cần checklist "vẽ ego_body cho MỖI vị trí có thân người trước khi vẽ box". Rework đã sửa 12/13 ca, chứng minh checklist đúng thứ tự loại được 100% ca ego. | (a) `submission/findings.csv` — 3 dòng có `rule_id=R07`; (b) `submission/rework/annotations-v2.xml` — 5 polygon ego_body đúng vị trí (145860 trái; 167700 trái+phải; 199770 trái+phải); (c) `submission/rework/delta.md` — spurious center 2→0 sau rework; (d) `submission/screenshots/199770-R4-ego-body-conflict.jpg` — case escalate cho vùng ego + object ambiguity |

## 2. Giới hạn của kết luận từ ba frame ADASIND

Slice B3-dense chỉ **3 frame** của 1 slice của 1 camera fisheye trên xe auto-rickshaw ở Ấn Độ. Cụ thể các giới hạn:

- **Class không đại diện**: 20 in-scope box gồm Bike (6), ThreeWheeler (4), Pedestrian (4), Truck (4), Car (2). **Không có Bus mẫu nào** — không thể benchmark class Bus. Tỷ lệ Bike/ThreeWheeler cao bất thường so với dataset production có nhiều Car/Truck.
- **Condition không đại diện**: 3 frame đều ban ngày, không mưa, không đêm, không sương. Không kết luận được về đèn xe khác, phản chiếu, ngược sáng cực đoan.
- **Zone edge mẫu quá nhỏ**: chỉ 4 ref boxes zone edge trên 20 tổng. Kết luận E4_model_domain "model gãy vùng edge" chỉ dựa trên matched=0/4 — cần ≥30 mẫu edge trên nhiều slice để chứng minh có ý nghĩa thống kê.
- **Rig chỉ 1 camera**: không test được seam, tracking cross-camera, BEV, hay 4-camera fusion. Sampling plan 200 frame cho 4 camera ở `45_sampling_plan.csv` là **giả lập** — không có ảnh 4 camera thật để verify.
- **Ref có nghi ngờ**: 1 ca escalate (R4 199770) và 1 ca E5 (R7 199770) chưa giải quyết → dù coi ref là ground truth thì vẫn có noise ước tính ~5-10% dòng finding.

## 3. Chuyển sang kế hoạch bốn camera giả lập

**Cách soát độ phủ 200 frame ở `45_sampling_plan.csv`**:

1. **Tránh chọn nhiều frame liên tiếp trong cùng cảnh** — dùng khoảng cách tối thiểu ~30 frame theo timestamp (≈1 giây nếu 30fps) để coi là "ca độc lập". Nếu chỉ lấy 200 frame liên tiếp trong 7 giây thì phương sai cảnh gần như 0 — dữ liệu không có giá trị.
2. **Mỗi camera front/rear/left/right cần ≥1 ca mỗi condition**: ngày, đêm, mưa, ngược sáng, tuyết (nếu có), seam với camera lân cận, blind spot. Với 200 frame / 4 camera = 50 frame/camera, chia 25 normal + 25 hard là hợp lý (front) hoặc 20 normal + 30 hard (side/rear vì hard case ưu tiên hơn).
3. **Hard slice phải có mỗi camera ≥1 ca có ≥2 object nhỏ zone edge** để test khả năng model fisheye handle vùng méo. Không được để hard slice toàn cảnh downtown center với vật lớn — như vậy không hard.
4. **Phân bố class trong sampling** cần match distribution production, không được lệch quá 20% (ví dụ nếu production có 40% Car thì 200 frame cần có ~35-45% frame chứa Car).

**Vì sao kế hoạch 200 frame này CHỈ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi**:

- **(a) 200/50000 = 0.4%** không đủ representative theo phân phối condition thật của 50k frame — sai số ước lượng ≥7% tại confidence 95% (theo Wald interval).
- **(b) Chưa có label ground truth cho 200 frame** — cần cycle qua gold set (`submission/46_gold_set_plan.md`) với 2 annotator + 1 QA trước khi báo bất kỳ số nào. Nếu skip gold set và tính precision/recall trên 200 frame với label 1 annotator, số bị bias nghiêm trọng.
- **(c) Peer agreement 3 người trên slice ADASIND (Hằng, Duẩn, Mạnh)** không thay reference đa người cho 4 camera — chỉ chứng minh nhất quán về guideline, không chứng minh guideline đúng cho các cảnh chưa gặp.
- **(d) Không có calibration cross-camera** trong tình huống lab hiện tại → không thể kết luận về seam/tracking dù mẫu 200 frame có ca seam.

**Chỉ số hợp lệ có thể báo từ 200 frame**:
- Số ca hard mỗi camera cần soát (đếm)
- Tỷ lệ frame có ≥1 object edge zone / tổng (đo coverage phân bố zone)
- Confusion matrix class (nếu có label 2+ người) — nhưng chỉ trên tập nhỏ, không extrapolate cho 50k
- Danh sách frame cần escalate cho Lab Coach

**KHÔNG được báo**: precision/recall/mAP tổng, "model đạt X%", "gold set", tỷ lệ lỗi production.
