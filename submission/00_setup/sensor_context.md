# Sensor context

- Rig: Camera fisheye đơn gắn ở phía trước phương tiện (góc nhìn hướng ra đường phía trước xe), độ cao tầm thấp ngang tầm nắp capo/gương chiếu hậu (khoảng 1.0m - 1.2m so với mặt đất). Bộ dữ liệu ADASIND không cung cấp thông số calibration ma trận nội/ngoại hay thông số rig chi tiết, nên chỉ ghi nhận qua quan sát thực nghiệm từ ảnh.
- `ego_body`: Nhìn thấy một phần nắp capo/thân xe của chính xe mang camera (ego vehicle) xuất hiện rõ ở vùng đáy góc dưới của khung hình.
- Vòng kính (lens circle): Ống kính mắt cá tròn (circular fisheye), vùng nhìn thấy hình tròn nằm ở vị trí trung tâm khung hình và chiếm phần lớn diện tích (khoảng 75% - 80% khung hình độ phân giải 1920x1080), bốn góc và viền xung quanh là vành đen ngoài ống kính (lens_border).
