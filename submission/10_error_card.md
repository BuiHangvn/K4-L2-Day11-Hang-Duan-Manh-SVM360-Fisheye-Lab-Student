# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | ATTRIBUTE | 1 |
| center | B2 | MISSING | 3 |
| center | B2 | SPURIOUS | 5 |
| center | B2 | STRUCTURE | 1 |
| center | C0 | SPURIOUS | 2 |
| center | C0 | WRONG_CLASS | 1 |
| edge | B2 | BOX_GEOMETRY | 1 |
| edge | B2 | MISSING | 5 |
| edge | B2 | SPURIOUS | 2 |
| edge | B2 | STRUCTURE | 1 |
| edge | B2 | WRONG_CLASS | 1 |
| mid | B2 | ATTRIBUTE | 1 |
| mid | B2 | MISSING | 5 |
| mid | B2 | SPURIOUS | 10 |
| mid | B2 | STRUCTURE | 1 |
| mid | C0 | ATTRIBUTE | 1 |

## Top defects
- SPURIOUS: 19 (ví dụ frame adasind_019560.jpg)
- MISSING: 13 (ví dụ frame adasind_082170.jpg)
- ATTRIBUTE: 3 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: Lỗi SPURIOUS (19 ca) và MISSING (13 ca) chiếm đa số, tập trung chủ yếu ở vùng mid (10 spurious) và edge (5 missing). Nguyên nhân chính là hiện tượng méo quang học fisheye cực mạnh ở vùng rìa kính, khiến model AI tiền huấn luyện trên ảnh phẳng nhầm lẫn các vệt bóng/vành kính thành phương tiện (`E4_model_domain`), đồng thời annotator bị nhầm lẫn giữa Truck cắt biên và ThreeWheeler ở frame `102750` (`E1_annotator_error`).
- Cách sửa và ai nhận việc (`owner`): Annotator (`annotator`) đã thực hiện rework sửa class L4 thành Truck, loại bỏ box rác L3 và bổ sung Truck bị thiếu tại P5. Đội ngũ AI (`ai_team`) cần huấn luyện lại model với dữ liệu tăng cường fisheye hoặc bổ sung module un-distortion trước khi phát hiện đối tượng.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): Báo cáo `submission/r3_diag/model_compare.html`, dòng finding `adasind_102750.jpg` trong `submission/findings.csv`, và kết quả cải thiện trong `submission/rework/delta.md`.

