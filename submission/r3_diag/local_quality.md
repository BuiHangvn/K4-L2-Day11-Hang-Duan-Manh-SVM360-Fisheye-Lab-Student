# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `e4653f46918342c6a43b2a307ba7557ba13cc5589e8c89bde2a2dbf5d04a33da`; slice `B3-dense`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_145860.jpg, adasind_167700.jpg, adasind_199770.jpg. Frame thiếu trong export: không.
TP=11; FP=5; FN=9; số lần đối chiếu=22; mean IoU của TP=0.768.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.500 | 0.873 | 0.773 |
| precision | 0.688 | 0.820 | 0.500 |
| recall | 0.550 | 0.550 | 0.250 |
| jaccard | 0.440 | 0.461 | 0.250 |
| dice | 0.611 | 0.614 | 0.400 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 2 | 3 | 0.773 | 0.600 | 0.500 | 0.375 | 0.545 |
| Car | 1 | 0 | 1 | 0.955 | 1.000 | 0.500 | 0.500 | 0.667 |
| Pedestrian | 3 | 0 | 1 | 0.955 | 1.000 | 0.750 | 0.750 | 0.857 |
| ThreeWheeler | 1 | 0 | 3 | 0.864 | 1.000 | 0.250 | 0.250 | 0.400 |
| Truck | 3 | 3 | 1 | 0.818 | 0.500 | 0.750 | 0.429 | 0.600 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_145860.jpg | 2 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_167700.jpg | 6 | 3 | 3 | 0.600 | 0.667 | 0.667 |
| adasind_199770.jpg | 3 | 2 | 6 | 0.300 | 0.600 | 0.333 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 0 | 0 | 0 | 3 |
| Car | 0 | 1 | 0 | 0 | 1 | 0 |
| Pedestrian | 0 | 0 | 3 | 0 | 0 | 1 |
| ThreeWheeler | 0 | 0 | 0 | 1 | 2 | 1 |
| Truck | 0 | 0 | 0 | 0 | 3 | 1 |
| <extra> | 2 | 0 | 0 | 0 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
