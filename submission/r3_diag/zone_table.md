# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 6 | 1 | 1 | 1 | 3 | SPURIOUS (1) |
| mid | 6 | 0 | 1 | 4 | 8 | SPURIOUS (1) |
| edge | 6 | 2 | 1 | 4 | 1 | MISSING (1) |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: Đối với người gán nhãn (L), vùng **edge** gãy nhiều nhất với 2 box missing và 1 box spurious (chủ yếu do nhầm nhãn L4 và bỏ sót R5), tiếp đến là vùng **center** với 1 missing và 1 spurious. Đối với model (M), vùng **mid** gãy nặng nề nhất khi sinh ra tới 8 box thừa (M_only/LM_noR) và bỏ sót 4 box (LR_noM), đồng thời vùng **edge** cũng bỏ sót 4 box của reference.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: Model YOLO26m được huấn luyện chủ yếu trên ảnh viễn thám/ảnh phẳng thông thường, do đó khi gặp hiệu ứng cong méo fisheye mạnh ở vùng mid và edge, model dễ nhận diện sai các mảng texture mặt đường/cột đèn thành phương tiện (gây ra 8 FP ở mid) và không nhận diện được xe bị kéo dãn hình học (gây ra 4 FN ở mid và 4 FN ở edge). Về phía người học (L), vùng edge bị nén quang học và cắt biên (truncated) khiến việc phân định giữa Truck và ThreeWheeler bị nhầm lẫn. Giới hạn: tập mẫu 3 frame chỉ đại diện cho một lát cắt nhỏ của điều kiện giao thông ban ngày, không thể khái quát cho toàn bộ phân bố lỗi của hệ thống SVM 4 camera.

