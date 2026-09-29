# Guideline patch — R04.1 ThreeWheeler taxonomy

## 1. Rule mới đề xuất — R04.1

Bổ sung ví dụ minh hoạ và cây quyết định cho R04 hiện tại. Rule R04 chỉ ghi bằng chữ "xe ba bánh chở người/hàng: auto-rickshaw, e-rickshaw, xích lô" mà không có ảnh phóng to hay tiêu chí phân biệt cụ thể — thiếu này gây confusion cho annotator lần đầu làm với ảnh Ấn Độ.

**R04.1 (đề xuất mới)**: **Auto-rickshaw / tuk-tuk / xích lô máy luôn là `ThreeWheeler`, KHÔNG phụ thuộc vào**:
- Màu sơn (vàng, đỏ, xanh, đen)
- Có mái che hay không (mái vải, mái cứng, không mái)
- Có buồng hành khách kín (như tuk-tuk có cửa sổ) hay hở
- Kích thước box trên ảnh (rickshaw xa <100px vẫn là ThreeWheeler nếu nhận biết được kiểu dáng 3 bánh)

**Cây quyết định để phân biệt**:
1. Có 3 bánh (2 sau + 1 trước) VÀ có buồng cho hành khách/hàng? → **ThreeWheeler**
2. Có 4 bánh, khung xe nhỏ, chở người thường/gia đình? → **Car** (bao gồm van chở người theo R04 hiện tại)
3. Có 4 bánh, khung xe cao/dài, thùng phía sau hoặc chở hàng nặng? → **Truck**
4. Có 4 bánh, khung xe dài, chở nhiều hàng ghế cho hành khách? → **Bus** (bao gồm minibus)

## 2. Áp dụng cho

- Class `ThreeWheeler` vs `Truck` vs `Bus` — mapping R04 khi gặp ảnh giao thông đô thị Ấn Độ / Đông Nam Á
- Không đổi attribute (`occluded`, `truncated`, `edge_zone`) hay `ignore_region.reason`
- Áp dụng cho cả B3-dense, B1-dense, B2-edge và các slice ADASIND khác

## 3. Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ

Bằng chứng từ slice B3-dense đã làm:

- **Confusion matrix local quality** (`submission/r3_diag/local_quality.md`): `ThreeWheeler → Truck` = 2 ca trên 4 ThreeWheeler ref = **50%** confusion rate. Recall class ThreeWheeler chỉ **0.25** — thấp nhất trong 5 class có mẫu.
- **Findings dòng cụ thể**:
  - `p1_calib` C0 dòng 2: Hằng gán auto-rickshaw vàng nhỏ (~40,770)-(99,855) thành `Car`, ref là `ThreeWheeler` (R04)
  - `r1_craft` 167700 L4+R6: Hằng gán Truck cho auto-rickshaw vàng lớn (9,915)-(261,1174), ref ThreeWheeler
  - `r1_craft` 199770 L6+R8: Hằng gán Truck cho rickshaw đỏ có mái (364,804)-(531,944), ref ThreeWheeler
  - `r2_qa` 145860 L2: nghi auto-rickshaw vàng cam nhỏ (237,853)-(282,910) đang bị gán Truck
  - `r2_qa` 167700 L6+L7+L8: 3 box Truck ở giữa xa, nghi có rickshaw
- **Root cause**: rule chỉ mô tả bằng chữ; annotator không phân biệt được rickshaw có mái cứng với minibus/Truck khi vật xa và có mái đóng kín. Cần ảnh crop minh hoạ.

## 4. Đề xuất ảnh minh hoạ đi kèm rule

Nếu patch được thông qua, thêm 3 ảnh crop vào `docs/02-rules-vi.md`:
1. Rickshaw vàng cổ điển không mái (từ `assets/images/adasind_167700.jpg` crop L4) → dán nhãn ThreeWheeler
2. Rickshaw đỏ có mái vải kín (từ `assets/images/adasind_199770.jpg` crop L6) → dán nhãn ThreeWheeler
3. Minibus/Bus nhỏ (từ ảnh có Bus thật, chưa có trong slice B3) → dán nhãn Bus để đối chiếu

## 5. `rules_version` mới và hiệu lực

- **v1.0.0 → v1.1.0** (minor bump — thêm ví dụ + cây quyết định cho rule hiện có, không đổi ngữ nghĩa gốc)
- **Hiệu lực từ**: round `rework` trở đi. Findings trước rework giữ nguyên `rules_version=v1.0.0` để truy được ngữ cảnh gốc.
- **Đề xuất train ngắn** cho các cohort tiếp theo: 15 phút clinic đầu buổi với 6 ảnh crop rickshaw/truck/bus để calibrate mắt trước khi bắt đầu P2.
