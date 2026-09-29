# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): Các vạch sơn trắng nằm ở tiền cảnh (khu vực phía dưới bên trái và trung tâm của ảnh), phân định rõ ràng ranh giới phân chia giữa các ô đỗ xe đơn lẻ cạnh nhau trên mặt sân bãi đỗ.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: Vạch kẻ dài định hướng lối xe chạy ở trung tâm bãi và các mép vỉa hè/ranh giới lòng đường không được vẽ làm `parking_line`, vì chúng chỉ có chức năng dẫn hướng lưu thông nội bộ hoặc phân cách lối đi, không phải là vạch tạo ranh giới ô đỗ xe.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: Polygon `free_space` bao quanh bề mặt đường nhựa trống nhìn thấy rõ của làn xe chạy chính giữa hai hàng ô đỗ; đa giác dừng lại trước hàng xe đỗ ở hậu cảnh và mép vỉa hè (curb), không đi xuyên qua các vật cản hay vùng bị che khuất.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): không có
