# Escalation ticket

## Ticket 1 — Reference nội tại mâu thuẫn ở frame 199770 R4

- **Frame**: `adasind_199770.jpg`, object_ref `R4` trong `data/_ref/B3-dense.xml` dòng 106
- **Ảnh chụp**: `submission/screenshots/199770-R4-ego-body-conflict.jpg` (vùng góc phải đáy với overlay ego_body + box R4)
- **Expected impact**: Ảnh hưởng tính đúng cho học viên slice B3-dense. Box ref `ThreeWheeler` (935,990)-(1080,1300) nằm HOÀN TOÀN trong polygon `ignore_region` reason=ego_body cũng của ref (924,813)-(1080,1552). Theo R09, box object không được nằm ≥50% trong ignore_region — ref hiện tại vi phạm chính rule R09. Nếu học viên vẽ đúng (không có box ở vùng ego_body), findings sẽ báo MISSING không hợp lý; nếu học viên vẽ box thì sẽ bị IGNORE_SCOPE. Cả hai kết quả đều sai.
- **Owner**: `data_ops` (người sản xuất teaching reference)
- **Recommendation**: Quyết định 1 trong 2 — (a) thu hẹp polygon ego_body ref để box R4 nằm ngoài, hoặc (b) xoá box R4 vì thân camera che khuất phần vật. Sau khi sửa, bump version teaching reference và thông báo lớp.

## Ticket 2 — Class không rõ ở frame 199770 R7

- **Frame**: `adasind_199770.jpg`, object_ref `R7` trong `data/_ref/B3-dense.xml` dòng 121
- **Ảnh chụp**: `submission/screenshots/167700-rework-classes.jpg` (cho thấy trạng thái sau rework của các class trong slice — dùng làm bằng chứng đối chiếu; ticket 2 tận dụng chung ảnh với ticket 1 vì cùng slice B3-dense và chưa có ảnh phóng to riêng cho vùng R7 tại thời điểm nộp)
- **Expected impact**: Ref gán `Truck` (211,816)-(275,861) 64×45px zone mid nhưng nhìn ảnh phóng to, vật xa và mờ, có thể là **ThreeWheeler** (auto-rickshaw có mái ở xa) chứ không phải xe tải. Cả L và M đều bỏ sót vật này. Nếu ref sai class thì findings E1 cho học viên (missing) là oan; nếu ref đúng thì cần bổ sung ví dụ Truck ở zone mid có kích thước tương tự để phân biệt với ThreeWheeler.
- **Owner**: `data_ops` (rà lại class ref) + `guideline` (thêm ví dụ nếu ref đúng)
- **Recommendation**: Lab Coach mở ảnh gốc phóng to vùng đó, quyết định class chuẩn. Nếu Truck đúng, thêm ví dụ vào R04. Nếu ThreeWheeler, sửa ref.
