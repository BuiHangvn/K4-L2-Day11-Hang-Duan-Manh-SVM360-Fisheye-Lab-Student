# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| adasind_102750.jpg | 3 ca: 1 WRONG_CLASS (L4 Truck vs ThreeWheeler), 1 MISSING (R5 Truck), 1 SPURIOUS (L3) | Là frame có tỷ lệ lỗi cao nhất trong slice B2-edge (Accuracy chỉ 42.9%), nhiều xung đột class nghiêm trọng ở vùng edge và mid. | Ảnh gốc, `submission/r3_diag/local_quality_conflicts.csv`, overlay HTML |
| adasind_082170.jpg | 1 ca MISSING (R5 Bike ở rìa mép ngoài kính) | Cần kiểm tra hiện tượng mất đối tượng ở vùng rìa kính mắt cá (edge zone) do méo quang học và cắt biên mạnh. | Ảnh gốc, `submission/r1_craft/compare.md`, checklist selfqc |

Giới hạn của kết luận từ ba frame ADASIND: Bộ 3 frame là một mẫu quá nhỏ được trích xuất trong cùng một điều kiện thời tiết và cung đường, không đủ tính đại diện thống kê để đánh giá độ tin cậy của toàn bộ hệ thống SVM 360 trong thực tế.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh
như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi: Phân bổ đều theo 4 camera (front, rear, left, right) với 2 phân tầng normal/hard. Để tránh tương quan chuỗi thời gian giữa các frame liên tiếp trong cùng một video clip, cần áp dụng bước nhảy thời gian (time-stride) tối thiểu 5-10 giây giữa các frame được chọn. Kế hoạch này là phương pháp lấy mẫu hướng rủi ro (risk-based sampling) nhằm tìm kiếm và phát hiện các trường hợp lỗi biên (edge cases) và điểm mù ghép nối (seamline), không phải là lấy mẫu ngẫu nhiên đồng đều (uniform random sampling), do đó chỉ dùng để soi lỗi chứ không đo lường tỷ lệ lỗi khách quan của mô hình trong vận hành thực tế.
