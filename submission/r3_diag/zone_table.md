# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 9 | 2 | 2 | 2 | 3 | WRONG_CLASS (2) |
| mid | 7 | 4 | 1 | 3 | 3 | MISSING (3) |
| edge | 4 | 3 | 2 | 4 | 4 | WRONG_CLASS (1) |

## Nhận xét

- **Zone gãy nhất cho L (Hằng)**: **mid** với 4/7 = 57% missing và **edge** với 3/4 = 75% missing (chỉ 1 matched). Lỗi WRONG_CLASS xuất hiện ở cả 3 zone (2 center + 1 edge), chủ yếu là gán nhầm auto-rickshaw thành `Truck` thay vì `ThreeWheeler` (R04). Ở zone center 2 spurious tương ứng với box vẽ nhầm vào vùng ego_body (L5 frame 167700 và L4 frame 199770 - reference ignore các box này bằng polygon ego_body). Frame 199770 gãy nặng nhất (3 TP / 6 FN theo local quality) do bỏ sót R3 Pedestrian mid, R6 Bike edge nhỏ, R7 Truck mid nhỏ.
- **Zone gãy nhất cho model M**: **edge** với 4/4 = 100% missing (LRM=0 ở edge). Model gán rất nhiều box lệch class hoặc thừa ở zone center của frame 199770 (7 M_only). Tổng hợp: model có xu hướng dựng box vùng biên/méo nhưng không khớp reference; giả thuyết **E4_model_domain** cho vùng méo fisheye cần dữ liệu nhiều slice hơn để chứng minh.
- **Giả thuyết**:
  - L nhầm Car/ThreeWheeler thành Truck (confusion matrix: ThreeWheeler→Truck 2, Car→Truck 1) - gap trong hiểu R04 về mapping van/pickup/rickshaw.
  - L vẽ box class object trên vùng thân người ngồi cùng camera - thiếu ego_body polygon bên phải ở frame 167700 (đã có ở 199770).
  - Model M gãy vùng edge fisheye do lệch miền dữ liệu (đào tạo trên ảnh phẳng, không biết vòng kính) - phù hợp E4 nhưng chỉ 1 slice fisheye không đủ chứng minh.
  - Bỏ sót Bike/Pedestrian nhỏ (~40-50px) ở zone mid/edge - tập trung vào vật lớn dễ thấy hơn vật nhỏ vùng méo rìa.
- **Giới hạn**: 3 frame ADASIND của 1 camera - không đại diện cho 4 camera SVM. Zone edge chỉ có 4 ref boxes → mẫu quá nhỏ để kết luận E4 chắc chắn. Reference cũng có nghi ngờ (R4 frame 199770 ghi ThreeWheeler nằm hoàn toàn trong polygon ego_body của chính reference - đã escalate).
