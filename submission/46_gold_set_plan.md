# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Ngược sáng đèn pha ban đêm, xe cắt ngang đột ngột ở ngã tư | Chói sáng làm mất viền xe, bóng đổ dài gây nhầm box | Giữ không gian ảnh gốc fisheye, lưu ma trận nội suy intrinsic và extrinsic | Review chéo 2 annotator độc lập (dual-annotation) + trọng tài senior duyệt |
| rear | Lùi xe vào bãi hẹp, người đi bộ/trẻ em cúi thấp sát cản sau | Điểm mù camera lùi, che khuất một phần bởi cản xe (ego_body) | Đánh dấu vùng ego_body chính xác, giữ hệ tọa độ camera sau | Soát kỹ checklist occluded/truncated, đối chiếu cảm biến siêu âm nếu có |
| left | Xe máy/xe đạp vượt sát sườn xe trong điều kiện trời mưa | Bị biến dạng quang học cực đại ở rìa mép ống kính, nước đọng | Lưu thông số distortion coefficient (k1-k4), giữ mask lens_border | Đo độ khớp IoU giữa 2 lượt thẩm định mù, phân xử nếu IoU < 0.85 |
| right | Đỗ xe sát vỉa hè cao, cây cối che khuất một phần phương tiện | Rìa kính cong làm vỉa hè và bánh xe bị biến dạng hình học | Không gian pixel mắt cá gốc, không unwarp trước khi gán nhãn | Soát đối chiếu với camera trước/sau tại vùng chồng lấn (overlap) |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule): Cần làm mới (refresh) gold set khi có thay đổi phần cứng camera (góc nhìn, cảm biến, tiêu cự), khi xe được hiệu chuẩn lại (re-calibration làm đổi ma trận ngoại suy extrinsic), hoặc khi có bản cập nhật định nghĩa taxonomy và guideline nhãn (ví dụ đổi ngưỡng chiều cao H hoặc cách gộp rider).
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box: Cần quy định rõ chính sách: cùng một đối tượng xuất hiện ở vùng chồng lấn (seamline) giữa camera trước và hông sẽ được gán 2 box riêng biệt trên từng ảnh mắt cá gốc nhưng chung một Global Track ID, hoặc được ghép thành một bounding box 3D trong không gian BEV (Bird's Eye View). Bằng chứng cần giữ gồm ma trận đồng bộ thời gian (hardware sync timestamp) và ma trận ghép nối camera.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera: Bốn camera mắt cá lắp quanh xe có góc đặt, độ cao, phân bố ánh sáng và mức độ biến dạng quang học khác nhau hoàn toàn. Độ khớp cao trên camera trước không đảm bảo camera hông nhận diện chính xác các góc chết hay vùng seamline phức tạp.
