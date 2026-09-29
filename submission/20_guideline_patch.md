# Guideline patch

- **Rule mới đề xuất:** R08 — Quy tắc phân định đối tượng ở xa và xử lý nén độ phân giải trong ảnh fisheye
- **Áp dụng cho:** Tất cả các class phương tiện (Car, Truck, ThreeWheeler, Bike, Pedestrian) ở vùng center và mid có chiều cao xấp xỉ ngưỡng 40 px.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật hiện tại chỉ quy định cứng ngưỡng chiều cao H >= 40 px nhưng chưa tính đến hiện tượng nén độ phân giải và quang sai sắc ở khoảng cách xa trong camera mắt cá, khiến annotator dễ nhầm lẫn loại phương tiện hoặc sinh ra box giả.
- **`rules_version` mới:** 1.1.0
- **Hiệu lực từ:** rework

