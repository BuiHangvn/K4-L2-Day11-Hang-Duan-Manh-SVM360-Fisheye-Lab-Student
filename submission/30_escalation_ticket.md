# Escalation ticket

## Ticket 1 · Reference nội tại mâu thuẫn — 199770 R4

- **Frame**: `adasind_199770.jpg`
- **Object**: `R4` trong `data/_ref/B3-dense.xml` dòng 106
  - Class: `ThreeWheeler`
  - Box: `(935, 990)-(1080, 1300)` (145×310px, chạm mép phải, cao 310px)
  - `truncated=true`, `edge_zone=false`
- **Vấn đề**: Box object R4 nằm **HOÀN TOÀN** trong polygon `ignore_region` reason=`ego_body` cũng của cùng reference:
  - Polygon ego_body ref: `(924, 813)-(1080, 1552)` (156×739px rectangle)
  - Box R4 (935-1080, 990-1300) hoàn toàn nằm bên trong polygon (924-1080, 813-1552)
  - **Vi phạm R09 của chính reference** — "không box nào được nằm ≥50% trong một polygon ignore_region"
- **Ảnh chụp**: `submission/screenshots/199770-R4-ego-body-conflict.jpg` — vùng góc phải đáy với overlay ego_body + box R4
- **Expected impact**:
  - Học viên vẽ đúng (không có box vùng ego_body) → findings báo `MISSING` không hợp lý (kỳ vọng đúng bị phạt)
  - Học viên vẽ box theo ref → findings báo `IGNORE_SCOPE` (box bị ignore, không có tác dụng)
  - Cả hai kết quả đều sai — không có cách vẽ nào cho học viên tránh finding false positive
  - Nếu để nguyên, mọi học viên chạy slice B3-dense sẽ có 1 dòng finding nghi ngờ mà không có action đúng
- **Owner**: `data_ops` (người sản xuất teaching reference)
- **Recommendation**:
  - **Option A** (khuyên): thu hẹp polygon ego_body ref để bao đúng vùng thân camera thấy được — có thể bên phải chỉ ôm (1050, 813)-(1080, 1552) chứ không rộng đến x=924, để lộ ra vùng có ThreeWheeler R4 làm object hợp lệ
  - **Option B**: xoá box R4 khỏi ref, vì phần rickshaw đằng sau ego_body bị che khuất >50% theo hình học
  - Sau khi sửa: bump version teaching reference (v1.0 → v1.1), thông báo lớp, invalidate manifest hash cũ

---

## Ticket 2 · Class không rõ — 199770 R7

- **Frame**: `adasind_199770.jpg`
- **Object**: `R7` trong `data/_ref/B3-dense.xml` dòng 121
  - Class: `Truck`
  - Box: `(211, 816)-(275, 861)` (64×45px, zone mid)
  - `truncated=false`, `occluded=false`, `edge_zone=false`
- **Vấn đề**: Vật xa mờ, box chỉ 64×45px. Nhìn ảnh phóng to, kiểu dáng nghi ngờ là **auto-rickshaw có mái** (ThreeWheeler theo R04.1 đề xuất) chứ không phải xe tải:
  - Kích thước nhỏ + vùng phố Ấn Độ + mật độ rickshaw cao → xác suất là ThreeWheeler cao hơn Truck
  - Cả annotator L và model M đều bỏ sót (`R7 R_only MISSING`) — có thể do vật quá nhỏ và mờ khó nhận class
- **Ảnh chụp**: `submission/screenshots/167700-rework-classes.jpg` — dùng chung với ticket 1 vì cùng slice; ảnh này cho thấy trạng thái sau rework của các class trong slice để làm bằng chứng đối chiếu (chưa có ảnh crop riêng cho vùng R7 tại thời điểm nộp — nếu Lab Coach yêu cầu, sẽ chụp bổ sung round sau)
- **Expected impact**:
  - Nếu ref sai class (Truck → phải là ThreeWheeler) thì finding `E1_annotator_error` cho học viên là oan — học viên có thể đã cân nhắc vật này nhưng do class ambiguous nên bỏ
  - Nếu ref đúng class Truck thì cần bổ sung ví dụ Truck ở zone mid có kích thước tương tự trong `docs/02-rules-vi.md` để annotator không lặp lại lỗi bỏ sót
- **Owner**: `data_ops` (rà lại class ref) + `guideline` (thêm ví dụ nếu ref đúng)
- **Recommendation**:
  - Lab Coach mở ảnh gốc `adasind_199770.jpg` phóng to 4× vùng (200, 800)-(290, 880)
  - Quyết định class chuẩn dựa trên kiểu dáng bánh + buồng
  - Nếu Truck đúng: thêm ảnh crop này vào R04 làm ví dụ Truck xa nhỏ
  - Nếu ThreeWheeler: sửa ref, bump version, thông báo lớp

---

## Tổng kết escalation

Cả 2 ticket đều thuộc slice B3-dense frame 199770 và đều liên quan **giới hạn của teaching reference**:
- Ticket 1: mâu thuẫn nội tại (ref vi phạm chính rule R09) — cần sửa reference
- Ticket 2: giới hạn phân giải/mờ + gap R04 → cần rà class hoặc bổ sung ví dụ

Không escalate cho bất kỳ dòng nào thuộc slice C0 hay B3-dense frame 145860/167700 — các ca ở đó đã có thể quyết định trong scope học viên (rework hoặc keep_with_reason).
