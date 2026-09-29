# Escalation ticket

## Ticket 1

- **Frame:** adasind_102750.jpg
- **Ảnh chụp:** assets/images/adasind_102750.jpg
- **Expected impact:** Giảm thiểu tranh cãi giữa annotator và reviewer đối với các phương tiện ở xa, đồng thời chuẩn hóa tiêu chí đánh giá chất lượng gold set khi đưa vào huấn luyện mô hình thị giác máy tính SVM 360.
- **Owner:** guideline
- **Recommendation:** Bổ sung quy tắc vào Guideline v1.1.0: đối với các phương tiện nằm ở vùng center/mid nhưng bị mờ nhòe do khoảng cách xa (chiều cao xấp xỉ ngưỡng 40 px), nếu không đủ chi tiết nhận dạng thì cho phép bao phủ bởi ignore_region hoặc không tính lỗi spurious/missing khi đánh giá.

