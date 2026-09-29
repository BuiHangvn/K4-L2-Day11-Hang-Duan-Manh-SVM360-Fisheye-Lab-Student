# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 9 | 2 | 2 | 2 | 3 | WRONG_CLASS (2) |
| mid | 7 | 4 | 1 | 3 | 3 | MISSING (3) |
| edge | 4 | 3 | 2 | 4 | 4 | WRONG_CLASS (1) |

## Nhận xét

### Zone gãy nhất — dẫn số ở bảng trên

**L (Hằng) gãy nhất ở**:
- Zone **mid**: missing 4/7 = **57%**, matched 3/7 = 43%. Cụ thể trên frame 199770 bỏ sót R3 Pedestrian (847,829)-(905,947) H=118, R7 Truck (211,816)-(275,861) H=45, và R4 ThreeWheeler (935,990)-(1080,1300) — ca R4 thuộc reference defect đã escalate. Pattern: vật ở zone mid thường có kích thước 45-120px, dễ bị bỏ qua khi annotator tập trung vào vật lớn center.
- Zone **edge**: matched chỉ 1/4 = **25%**, missing 3/4 = 75%. Slice B3-dense có 4 ref box zone edge (mẫu nhỏ) nhưng chỉ khớp được 1 → tỷ lệ rất thấp. Kết hợp WRONG_CLASS (L4+R6 ThreeWheeler bị gán Truck ở 167700) và BOX_GEOMETRY (L1+R5 Bike quá rộng ở 199770).

**M (model YOLO26m đóng băng) gãy nhất ở**:
- Zone **edge**: model **matched=0/4 = 0%** ở zone edge (dựa trên compare `LRM=0` ở edge trong `model_compare.md`). Kết hợp có 4 M_only (model thừa) và 2 LM_noR (model + L cùng thấy nhưng ref không có, có thể L geometry lệch nên compare không ghép).
- Frame 199770 có **7 M_only** (model thấy nhưng L và R đều không) — tổng M thừa cao bất thường ở 1 frame → có thể do model hallucinate ở cảnh phố đông với nhiều điểm noise.

**Điểm chung L và M cùng gãy** ở zone edge — nhưng lý do khác nhau: L bỏ sót do focus attention, M dựng box thừa do không calibrate cho fisheye distortion.

### Giả thuyết vì sao

- **Gap R04 mapping** (E1_annotator_error kết hợp E2_guideline_gap): Hằng gán auto-rickshaw thành `Truck` 3 lần trên 4 ref ThreeWheeler ở B3-dense. Confusion matrix: `ThreeWheeler → Truck` = 2/4 = **50%**. Root cause là rule R04 chỉ mô tả bằng chữ, không có ảnh minh hoạ rickshaw có mái vs Truck. Đã đề xuất R04.1 patch trong `submission/20_guideline_patch.md`.
- **Ego_body scope confusion** (E1_annotator_error, R07): Trước rework có 2 box class object phủ đè polygon ego_body ref (167700 L5 Pedestrian, 199770 L4 Truck). Đây không phải lỗi ngẫu nhiên — cả 2 đều là **ego passenger bên phải** mà Hằng nghĩ là người/xe thật đứng ngoài. Rework đã fix bằng xoá box + vẽ polygon ego_body phải. Bài học: ego_body ADASIND không chỉ ở đáy trái (đúng cho xe con) mà còn ở đáy phải (đúng cho auto-rickshaw có 2 passenger).
- **Box lỏng ở vật nhỏ zone edge** (E1, R02): 2 ca BOX_GEOMETRY sau rework đã fix — L1 Bike (312,1011)-(362,1112) 167700 lệch dọc so với ref (296,942)-(346,1112); L1 Bike (79,828)-(145,909) 199770 rộng gấp đôi ref (91,836)-(119,903). Cả 2 đều là vật nhỏ dưới H=100 ở zone gần edge/mid — annotator vẽ box theo bounding rectangle thô, không precise cho vật nhỏ.
- **Model M gãy edge** (E4_model_domain — giả thuyết): matched=0/4 và có 4 M_only ở zone edge. Có thể do YOLO26m train trên ảnh phẳng, chưa biết `lens_border` và fisheye distortion → dựng box thừa vùng méo và bỏ vật thật ở vùng edge. **Nhưng chỉ 4 ref boxes ở edge là mẫu quá nhỏ** để chứng minh có ý nghĩa thống kê — cần benchmark với ≥3 slice khác (B1, B2) và ≥30 mẫu edge trước khi kết luận. Hiện chỉ ghi là giả thuyết trong 7 dòng r3_diag findings với `why=E4_model_domain, owner=ai_team, action=keep_with_reason`.
- **Reference defect** (E0): 1 ca escalate mức P2 — 199770 R4 ThreeWheeler nằm hoàn toàn trong polygon ego_body của chính ref. Không phải lỗi học viên, cần data_ops sửa reference. Chi tiết trong `submission/30_escalation_ticket.md` Ticket 1.

### Giới hạn của slice ba frame

- **Mẫu quá nhỏ**: 20 in-scope box trên 3 frame. Zone edge chỉ 4 ref → không có ý nghĩa thống kê để so class-level precision/recall.
- **Không có Bus mẫu nào**: 5 class có mẫu (Bike, Car, Pedestrian, ThreeWheeler, Truck) — không thể đánh giá Bus.
- **Chỉ 1 slice, 1 camera, 1 dataset (ADASIND Ấn Độ)**: kết luận E4_model_domain hay pattern chung không extrapolate được cho các camera 4-view khác hay dataset châu Âu/Mỹ.
- **1 ca reference defect** (R4 199770) và **1 ca reference class ambiguous** (R7 199770) → dù coi ref là ground truth, vẫn có noise ~5-10% dòng finding không thể tự giải trong scope học viên.
- **Không có timestamp cross-frame** trong ADASIND slice B3-dense (3 frame không liên tiếp: 145860, 167700, 199770) → không thể đánh giá tracking hay temporal consistency.
