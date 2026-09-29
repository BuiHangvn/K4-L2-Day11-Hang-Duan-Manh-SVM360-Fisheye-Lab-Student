# Quan sát vạch ô đỗ

- Hai vạch `parking_line` chính đã vẽ: (1) vạch chéo phía tiền cảnh giữa ảnh chạy từ
  khoảng `(x=405, y=650)` lên `(x=528, y=720)` — chia hai ô đỗ liền nhau ở hàng đầu;
  (2) vạch chéo bên phải tiền cảnh chạy từ `(x=696, y=622)` sang `(x=960, y=685)` —
  ranh giới ô đỗ ngoài cùng bên phải. Đây là hai vạch tôi tự tin nhất về vai trò
  "chia ô đỗ" trên ảnh core.
- Ngoài hai vạch chính, XML export còn chứa nhiều polyline ngắn (2 điểm) ở hàng ô đỗ
  phía xa (y ≈ 470–570) do thao tác trong CVAT tạo thêm; chúng bám theo các đoạn sơn
  mờ hơn ở hàng thứ hai. Tôi giữ nguyên chứ không đi xoá vì các đoạn này vẫn nằm trên
  vệt sơn thấy được, dù độ tin cậy thấp hơn hai vạch tiền cảnh. Người soát nên đọc
  hai vạch tiền cảnh (dòng XML 52 và 54) làm bằng chứng chính cho vai trò `parking_line`.
- Một vạch/dấu **không** nên tính là `parking_line`: polyline ở dòng 94 chạy suốt chiều
  ngang ảnh từ `(0, 541)` sang `(960, 506)` — đây là **mép/biên** phân chia hai hàng ô đỗ
  gần và xa (một dải sơn liên tục dọc lối xe chạy), không phải vạch chia hai ô đơn lẻ.
  Đúng luật ở [docs/11-parking-lines-vi.md], vạch dạng mép/lối xe chạy không nên gán
  `parking_line`; đây là lỗi kiểu tôi ghi nhận để không lặp lại.
- Polygon `free_space` bao vùng mặt đường trống giữa hàng ô đỗ tiền cảnh và hàng ô đỗ
  phía trong. Biên trên đi theo mép dưới của hàng ô xa (đường vạch dài dòng 94); biên
  dưới đi theo mép trên của các ô tiền cảnh. Polygon **không** đi xuyên chiếc xe đỏ
  (bằng cách dừng lại trước và bỏ qua vùng nó chiếm ở giữa-trái); không bao gồm hàng
  cây, curb hay lề đường.
- Ca chưa chắc cần hỏi người soát: các đoạn vạch mờ ở hàng ô đỗ phía xa quanh xe đỏ
  và mép trái ảnh — không xác định chắc đây là ranh giới ô riêng lẻ hay chỉ là vệt
  sơn cũ. Tôi đã vẽ theo phần thấy được nhưng độ tin cậy thấp, cần người soát xác nhận
  hoặc bỏ khi có bối cảnh chuẩn (rig, calibration bãi đỗ).
