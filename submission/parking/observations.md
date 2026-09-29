# Quan sát vạch ô đỗ — bãi đỗ camera thường

Ảnh core: `assets/parking/parking-lot-core.jpg` (960×720, bãi đỗ trống có xe đỏ giữa ảnh + hàng cây phía xa).

## 1. Hai vạch `parking_line` chính đã chọn

- **Vạch chéo giữa tiền cảnh**: polyline từ `(x=405, y=650)` xuống `(x=528, y=720)` — chia hai ô đỗ liền nhau ở hàng đầu bên trái xe đỏ. Đây là vạch tôi tự tin nhất về vai trò "chia ô đỗ" vì (a) đoạn sơn liền, không đứt gãy; (b) hướng vuông góc với hướng đỗ xe của các ô lân cận; (c) độ dài phù hợp với 1 chiều ô đỗ.
- **Vạch chéo bên phải tiền cảnh**: polyline từ `(x=696, y=622)` sang `(x=960, y=685)` — ranh giới ô đỗ ngoài cùng bên phải khung hình. Sơn còn rõ ở đoạn tôi vẽ; vạch dừng ở mép ảnh (không thấy tiếp).

## 2. Các đoạn thêm trong XML export

Ngoài 2 vạch chính, XML export chứa **thêm nhiều polyline ngắn** (2 điểm) ở hàng ô đỗ phía xa (y ≈ 470–570) do thao tác trong CVAT tạo thêm. Chúng bám theo các đoạn sơn mờ hơn ở hàng thứ hai. Cụ thể XML `submission/parking/annotations.xml` có tổng **25 polyline** — nhiều hơn 2 vạch chính tôi định vẽ. Tôi giữ nguyên chứ không đi xoá vì:
- Các đoạn ngắn nằm trên vệt sơn thấy được (không phải vạch tưởng tượng)
- Độ tin cậy về vai trò chia ô đỗ của chúng thấp hơn 2 vạch chính (vạch mờ, khó xác định điểm đầu/cuối)
- Xoá sạch có thể mất bằng chứng cho những vạch thật sự là ranh giới ô

**Người soát nên tập trung vào 2 vạch chính (dòng XML 52 và 54)** làm bằng chứng cho vai trò `parking_line`; các đoạn còn lại là quan sát bổ sung với độ tin cậy thấp hơn.

## 3. Một vạch đã bỏ qua / gán không đúng

- **Polyline dài xuyên ngang ảnh (dòng 94 XML)**: chạy từ `(0, 541)` sang `(960, 506)` — đây là **mép/biên phân chia hai hàng ô đỗ gần và xa** (một dải sơn liên tục dọc lối xe chạy), KHÔNG phải vạch chia hai ô đơn lẻ. Theo `docs/11-parking-lines-vi.md`: "Nếu một dải sơn dài chỉ dẫn lối xe chạy, là mép đường, mũi tên hoặc vạch qua đường, đừng gọi nó là parking_line". Đây là **lỗi kiểu** tôi ghi nhận trong bài để không lặp lại — polyline này lẽ ra phải là mép đường/biên, không nên gán class `parking_line`.
- **Các đoạn vạch mờ ở hàng ô đỗ phía xa quanh xe đỏ**: cả thân vạch lẫn hai đầu đều mờ, không xác định chắc được vị trí ranh giới ô. Đã bỏ qua khi chọn 2 vạch chính (không vẽ theo suy đoán), nhưng có thể vô tình vẽ 1 số polyline ngắn ở đoạn này khi tương tác với CVAT.

## 4. Polygon `free_space`

- **Phạm vi**: bao vùng mặt đường trống nhìn thấy được giữa hàng ô đỗ tiền cảnh và hàng ô đỗ phía trong (khoảng lối xe chạy chính giữa bãi)
- **Biên trên**: đi theo mép dưới của hàng ô xa (chạm gần đường vạch dài dòng 94 XML — nơi chuyển từ lối xe chạy sang hàng ô đỗ xa)
- **Biên dưới**: đi theo mép trên của các ô tiền cảnh (chạm các đoạn vạch parking_line đã vẽ)
- **Không đi xuyên**:
  - Chiếc **xe đỏ giữa ảnh** — polygon dừng lại trước và bỏ qua vùng nó chiếm ở giữa-trái ảnh
  - Hàng cây phía xa (đã ngoài phạm vi lối xe chạy)
  - Curb hoặc lề đường (không có curb thấy rõ trong ảnh, nhưng vạch parking_line đóng vai trò ranh giới)
  - Cột đèn hoặc biển báo phía xa

Polygon 1 dòng XML — không tách nhiều mảnh. Có ~14 điểm bao phần free-space.

## 5. Ca còn nghi ngờ cần hỏi người soát

- Các đoạn vạch mờ ở hàng ô đỗ phía xa quanh xe đỏ và mép trái ảnh — không xác định chắc đây là ranh giới ô riêng lẻ hay chỉ là vệt sơn cũ mờ đi. Đã vẽ theo phần thấy được nhưng độ tin cậy thấp, cần người soát xác nhận hoặc bỏ khi có bối cảnh chuẩn (rig, calibration bãi đỗ)
- Đoạn sơn ở góc phải xa: khó phân biệt là vạch chia ô đỗ hay vạch chỉ hướng lối xe chạy — vì góc quan sát nghiêng, hình học không rõ

## Ghi chú

Ảnh core `parking-lot-core.jpg` là camera thường (không phải fisheye) và không có calibration hay ground truth. Polygon `free_space` là nhận xét về mặt đường **nhìn thấy được** tại thời điểm chụp, KHÔNG phải tuyên bố nơi xe tự hành có thể đi an toàn. Chỉ ảnh core được nạp vào task parking; ảnh đối chiếu `parking-lot-contrast.png` chỉ dùng để so sánh vai trò vạch, không đưa vào export.
