# Sensor context

## Rig (quan sát từ ảnh, chưa có tài liệu chính thức)

Camera fisheye có vẻ được lắp trên phương tiện **hai/ba bánh kiểu open-air** (auto-rickshaw / tuk-tuk / xe máy chở khách) — không phải ô tô con. Bằng chứng:

- 3 frame của slice B3-dense (adasind_145860, 167700, 199770) đều cho **góc nhìn thấp gần mặt đường** (điểm nhìn cách đường ~1-1.5m theo ước lượng phối cảnh) — không phải góc nhìn cao 2-3m của ô tô SUV/sedan
- Không có capo ô tô, không có gương chiếu hậu ngoài, không có táp lô hoặc trụ A/B trong khung hình
- Có phần thân người ngồi cạnh camera xuất hiện ở đáy khung — dấu hiệu của rig open-air nơi passenger ngồi lộ ra ngoài camera
- Đường chân trời cong theo vòng kính (frame 145860 highway thấy rõ) → xác nhận là fisheye lens, không phải wide-angle rectilinear
- Camera hướng nhìn **về phía trước** trong 3/3 frame (không thấy đuôi xe hay side view)

Không có tài liệu rig chính thức cho ADASIND (dataset chỉ công khai frame + annotations, không kèm mount description), nên chỉ ghi theo quan sát trực quan trên ảnh.

## `ego_body` — nơi thân người/xe ego xuất hiện

**Frame 145860 (highway)**: 1 vị trí — **đáy trái**. Thấy rõ chân + giày sneaker xanh + tay áo caro xanh của người ngồi cùng camera. Không có ego bên phải (frame highway chỉ có 1 người). Polygon ego_body ~ (0, 1020) đến (350, 1791).

**Frame 167700 (đường phố đông)**: 2 vị trí — **đáy trái** (vai + tay áo caro rõ) và **đáy phải** (mờ hơn — phần đầu/vai người ngồi bên phải, đã bị bôi mờ mặt cho privacy). Polygon ego_body trái ~ (0, 1182) đến (286, 1675); polygon ego_body phải ~ (1080, 791) đến (972, 1403).

**Frame 199770 (phố có cây)**: 2 vị trí — **đáy trái** (tay áo caro xanh) và **đáy phải** (mảng người mặc áo trắng, có thể là váy). Polygon ego_body trái ~ (1.6, 981) đến (147, 1593); polygon ego_body phải ~ (1080, 1279) đến (960, 1554).

Không thấy capo, gương chiếu hậu hay vô lăng ở bất kỳ frame nào. Hai frame 006840 và 271039 (được GUIDE nêu là ngoại lệ không có ego) không thuộc slice B3-dense của Hằng.

## Vòng kính (lens circle)

- **Hình dạng**: gần tròn (hơi ellipse do fisheye lens có FOV rộng theo chiều ngang hơn dọc)
- **Vị trí**: nằm gần giữa khung hình 1080×1920, hơi lệch xuống dưới trung tâm ~50-100px
- **Kích thước**: chiếm khoảng **90-95% chiều rộng** ảnh (~950-1000px trên 1080) và **85-90% chiều cao** (~1650-1750px trên 1920)
- **Vignette**: bốn góc khung hình (đặc biệt 4 góc rectangle 1080×1920) là vùng đen do ống kính không phủ tới — đây là vùng `lens_border` với 2 polygon top + bottom được prefill cho mỗi frame trong `assets/prefill/B3-dense.xml`
- **Méo (barrel distortion)**: rìa vòng kính có méo mạnh; vật ở gần mép bị kéo dài theo phương tiếp tuyến. Ảnh 145860 cho thấy rõ đường chân trời cong theo vòng kính. Vật ở center (r/R < 0.35) ít méo, vật ở edge (r/R ≥ 0.6) méo nặng — box thẳng khó fit chính xác vùng edge.

## Ghi chú cho ai đọc sau

- Frame ADASIND đã bôi mờ khuôn mặt + biển số cho privacy — annotator không thể xác định gender/tuổi/danh tính; nếu rule yêu cầu attribute như vậy thì cần dataset khác
- Không có timestamp giữa 3 frame — không suy được vận tốc xe hoặc đối tượng
- Không có calibration intrinsic (focal length, distortion coefficients) — không thể undistort chính xác về ảnh phẳng; annotator phải vẽ trực tiếp trên fisheye gốc theo R02
