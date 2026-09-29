# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `133efe7fe171d46653efa8aa473f571eab72f83405e426adebe60605cfaf4c9f`; slice `B2-edge`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_069450.jpg, adasind_082170.jpg, adasind_102750.jpg. Frame thiếu trong export: không.
TP=15; FP=3; FN=3; số lần đối chiếu=20; mean IoU của TP=0.844.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.750 | 0.925 | 0.850 |
| precision | 0.833 | 0.917 | 0.667 |
| recall | 0.833 | 0.792 | 0.333 |
| jaccard | 0.714 | 0.708 | 0.333 |
| dice | 0.833 | 0.802 | 0.500 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 5 | 0 | 1 | 0.950 | 1.000 | 0.833 | 0.833 | 0.909 |
| Pedestrian | 3 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 6 | 3 | 0 | 0.850 | 0.667 | 1.000 | 0.667 | 0.800 |
| Truck | 1 | 0 | 2 | 0.900 | 1.000 | 0.333 | 0.333 | 0.500 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_069450.jpg | 5 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_082170.jpg | 7 | 0 | 1 | 0.875 | 1.000 | 0.875 |
| adasind_102750.jpg | 3 | 3 | 2 | 0.429 | 0.500 | 0.600 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|
| Bike | 5 | 0 | 0 | 0 | 1 |
| Pedestrian | 0 | 3 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 6 | 0 | 0 |
| Truck | 0 | 0 | 1 | 1 | 1 |
| <extra> | 0 | 0 | 2 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
