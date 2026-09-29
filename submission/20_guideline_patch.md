# Guideline patch

- **Rule mới đề xuất (R04.1 — auto-rickshaw taxonomy)**: Bổ sung ví dụ minh hoạ cho R04
  "ThreeWheeler = xe ba bánh chở người/hàng". Cụ thể auto-rickshaw Ấn Độ (dù có mái
  che, có buồng hành khách kín, sơn màu nào) vẫn là **ThreeWheeler**, KHÔNG phải Bus
  hay Truck. Rule cần thêm cột đối chiếu ảnh: "rickshaw đỏ có mái = ThreeWheeler;
  rickshaw vàng = ThreeWheeler; tuk-tuk có bảng hiệu bên hông = ThreeWheeler".
- **Áp dụng cho**: class `ThreeWheeler` vs `Truck` vs `Bus` (mapping R04). Không đổi
  attribute hay ignore_region.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ**: R04 chỉ nêu "xe ba bánh
  chở người/hàng: auto-rickshaw, e-rickshaw, xích lô" bằng chữ mà không có ảnh minh
  hoạ. Trên slice B3-dense: 3 ca WRONG_CLASS đã xảy ra vì Hằng gán auto-rickshaw
  thành `Truck` (2 ca) và trước rework 1 ca gán thành `Car` — đều là gap của luật
  hiện tại cho annotator lần đầu làm với ảnh Ấn Độ. Confusion matrix local quality
  cho thấy ThreeWheeler→Truck 2/4 = 50%.
- **`rules_version` mới**: v1.0.0 → **v1.1.0** (minor bump vì thêm ví dụ cho rule
  hiện có, không đổi ngữ nghĩa)
- **Hiệu lực từ**: round `rework` trở đi. Findings trước rework giữ nguyên rules_version=v1.0.0
  để truy được ngữ cảnh gốc.
