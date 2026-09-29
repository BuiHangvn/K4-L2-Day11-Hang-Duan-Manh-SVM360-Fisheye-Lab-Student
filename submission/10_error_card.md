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

- **Lỗi nổi bật**: `MISSING` chiếm cao nhất (15 ca), phần lớn ở zone **mid B3** (7 ca) và **edge B3** (5 ca) trên slice B3-dense — cụ thể frame `adasind_199770.jpg` thiếu R3 Pedestrian mid, R6 Bike edge nhỏ, R7 Truck mid nhỏ, R4 ThreeWheeler (ref defect). Lỗi phổ biến thứ hai: `WRONG_CLASS` (8 ca) chủ yếu là gán `Truck` thay `ThreeWheeler` cho auto-rickshaw (R04 gap).
- **Nguyên nhân khả dĩ (`why`)**: `E1_annotator_error` chi phối — Hằng tập trung vẽ vật lớn dễ thấy ở center nên bỏ sót vật nhỏ ở mid/edge, và nhầm mapping R04 giữa Truck vs ThreeWheeler cho auto-rickshaw Ấn Độ (không phân biệt được rickshaw có mái = ThreeWheeler). Ngoài ra 2 ca `IGNORE_SCOPE` (frame 167700 L5, 199770 L4) đều là ego_body người ngồi bên phải bị gán nhầm thành object class - do thiếu polygon ego_body phải ở 167700 và L4 199770 phủ đè lên polygon ego_body ref.
- **Cách sửa và owner**: rework đã hoàn thành 12/13 dòng action=rework - kết quả delta zone edge từ 1 matched lên 4 matched, zone mid từ 3 lên 4. Còn 1 dòng R3 Pedestrian mid chưa vẽ (`annotator` sẽ handle round sau). Escalate 2 ca cho `data_ops`: 199770 R4 ThreeWheeler nằm trong ego_body ref (ref defect) và R7 Truck class chưa rõ. Giả thuyết `E4_model_domain` cần `ai_team` verify với nhiều slice hơn.
- **Bằng chứng**: `submission/findings.csv` 44 dòng có evidence path, `submission/r3_diag/zone_table.md` bảng số, `submission/rework/delta.md` chứng minh trước/sau, `submission/p1_calib/compare.md` cho C0.
