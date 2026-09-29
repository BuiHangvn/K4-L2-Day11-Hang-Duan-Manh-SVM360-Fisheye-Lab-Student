# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B3 | MISSING | 3 |
| center | B3 | SPURIOUS | 5 |
| center | B3 | WRONG_CLASS | 4 |
| edge | B3 | BOX_GEOMETRY | 1 |
| edge | B3 | MISSING | 5 |
| edge | B3 | SPURIOUS | 4 |
| edge | B3 | WRONG_CLASS | 2 |
| edge | C0 | SPURIOUS | 1 |
| edge | C0 | WRONG_CLASS | 1 |
| mid | B3 | BOX_GEOMETRY | 2 |
| mid | B3 | IGNORE_SCOPE | 4 |
| mid | B3 | MISSING | 7 |
| mid | B3 | SPURIOUS | 3 |
| mid | B3 | WRONG_CLASS | 1 |
| unknown | C0 | STRUCTURE | 1 |

## Top defects
- MISSING: 15 (ví dụ frame adasind_199770.jpg)
- SPURIOUS: 13 (ví dụ frame adasind_019560.jpg)
- WRONG_CLASS: 8 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

### 1. Lỗi nổi bật và pattern quan sát

**MISSING (15 ca)** là top-1 defect, tập trung ở zone **mid B3** (7 ca) và **edge B3** (5 ca). Điểm đáng chú ý: 100% ca MISSING ở B3 đều thuộc frame `adasind_199770.jpg` — frame phố đông nhất trong slice với nhiều vật nhỏ và hàng ô đỗ rickshaw ở xa. Cụ thể các dòng đã ghi:

- `R3 Pedestrian` (847, 829)-(905, 947) H=118px zone mid — người áo trắng đứng bên đường, bị nắng chói che một phần
- `R6 Bike` (109, 839)-(144, 882) H=43px zone edge — Bike nhỏ vừa qua ngưỡng H=40, gần vòng kính
- `R7 Truck` (211, 816)-(275, 861) H=45px zone mid — vật xa mờ, cả model M cũng bỏ sót
- `R4 ThreeWheeler` (935, 990)-(1080, 1300) — thuộc ca escalate (ref defect nội tại)

**WRONG_CLASS (8 ca)** là top-2 defect. Pattern rõ nét: 5/8 ca là gán `Truck` cho auto-rickshaw có mái/không có mái, phải là `ThreeWheeler` theo R04. Confusion matrix local quality xác nhận: `ThreeWheeler → Truck` xảy ra 2 lần (ThreeWheeler recall = 0.25 — thấp nhất trong 5 class).

**SPURIOUS + IGNORE_SCOPE (13 + 4)** phần lớn liên quan **ego_body**: 3/13 SPURIOUS ở C0 do quên polygon ego_body ban đầu; 2/4 IGNORE_SCOPE ở B3 là box class object phủ đè polygon ego_body của reference (167700 L5 Pedestrian, 199770 L4 Truck — cả hai đã confirm là ego passenger bên phải chứ không phải người/xe thật).

### 2. Nguyên nhân khả dĩ (`why`) và chứng cứ

- **`E1_annotator_error` chi phối** (~70% dòng r1_craft có action=rework): (a) tập trung vào vật lớn dễ thấy ở center nên bỏ sót vật nhỏ mid/edge; (b) gap R04 mapping — không phân biệt được auto-rickshaw có mái với Truck/Bus (ảnh Ấn Độ chưa quen); (c) confuse Label `ignore_region` với class object khác khi vẽ ego_body — đã lặp lại ở C0 (vẽ Bike vào vùng ego) và P2 (vẽ Pedestrian vào vùng ego passenger phải 167700).
- **`E0_reference_defect`** (1 ca P0, 1 ca P2): `199770 R4 ThreeWheeler` (935, 990)-(1080, 1300) nằm HOÀN TOÀN trong ref polygon ego_body (924, 813)-(1080, 1552) — vi phạm R09 của chính reference. Đã escalate vì đây là mâu thuẫn nội tại không thể tự giải trong scope học viên.
- **`E4_model_domain`** (giả thuyết): model M matched=0/4 ở zone edge và 7 M_only ở frame 199770. Nhưng chỉ 1 slice ADASIND không đủ chứng minh — cần kiểm với B1/B2 và ≥30 frame trước khi kết luận model gãy vùng méo fisheye.
- **`E5_unresolved`** (2 ca): `145860 L2 IGNORE_SCOPE` (vùng unreadable ref chỉ 43×49px trùng box L2 — chưa rõ vùng thực sự unreadable hay ref sai); `199770 R7 Truck` (class chưa rõ, cần soi phóng to).

### 3. Cách sửa và owner

Rework đã hoàn thành **12/13 dòng action=rework**, kết quả delta chi tiết trong `submission/rework/delta.md`:

- Zone **edge**: matched **1 → 4** (+300%), missing 3 → 0, spurious 2 → 0 — cải thiện lớn nhất
- Zone **center**: matched 7 → 9, spurious 2 → 0 (xoá 2 box vẽ nhầm ego_body)
- Zone **mid**: matched 3 → 4, spurious 1 → 0, còn 3/4 missing chưa fix

**Còn tồn**: 1 dòng `R3 Pedestrian 199770 mid` chưa vẽ — `owner=annotator` sẽ handle round sau nếu có (không nghiêm trọng vì P1).

**Escalate cho `data_ops`**: 2 ca ở `submission/30_escalation_ticket.md` — R4 199770 (ref nội tại mâu thuẫn) và R7 199770 (class chưa rõ).

**Verify cho `ai_team`**: giả thuyết E4_model_domain cần benchmark với ≥3 slice fisheye khác nhau (không chỉ B3-dense) và ≥30 frame để có kết luận đáng tin.

**Guideline patch cho `guideline`**: đề xuất R04.1 thêm ảnh minh hoạ auto-rickshaw = ThreeWheeler — chi tiết trong `submission/20_guideline_patch.md`.

### 4. Bằng chứng

- `submission/findings.csv` — 44 dòng, mỗi dòng có `evidence` trỏ tới XML annotations, reference, hoặc compare report
- `submission/r3_diag/zone_table.md` — bảng zone × cell của L/R/M có nhận xét
- `submission/r3_diag/local_quality.md` — micro/macro precision/recall + confusion matrix
- `submission/rework/delta.md` — số matched/missing/spurious trước+sau rework theo zone
- `submission/p1_calib/compare.md` — 3 ca calibration C0 (WRONG_CLASS + SPURIOUS + STRUCTURE)
- `submission/screenshots/199770-R4-ego-body-conflict.jpg` — hình chụp ca escalate
- `submission/screenshots/167700-rework-classes.jpg` — hình chụp sau rework
