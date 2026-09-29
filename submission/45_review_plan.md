# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Class Truck vs ThreeWheeler cho auto-rickshaw (tất cả frame ADASIND) | 3 WRONG_CLASS trên B3-dense (167700 L4, 199770 L6, 145860 L2) + 1 trên C0 (L5) = 4/8 tổng WRONG_CLASS; confusion matrix ThreeWheeler→Truck 2/4 = 50% | Đây là lỗi mapping rule R04 lặp lại nhiều frame và nhiều slice — nếu không sửa guideline sẽ tái phát cho annotator sau; giả thuyết E1_annotator_error kết hợp E2_guideline_gap (rule không có ảnh ví dụ) | `submission/findings.csv` các dòng calib+r1_craft có rule_id R04; `submission/r3_diag/local_quality_confusion.csv`; ảnh crop auto-rickshaw vàng/đỏ có mái |
| Ego_body scope ở khung fisheye có passenger bên phải (frame 167700, 199770) | 2 IGNORE_SCOPE P0 (167700 L5 Pedestrian, 199770 L4 Truck) — cả hai đều vẽ box class object trên vùng ego_body passenger bên phải; C0 cũng bỏ ego_body ban đầu | Sai R07 mức P0 = sai phạm vi dữ liệu; nếu chưa fix thì ảnh hưởng mọi compare/local-quality của mọi bài học viên có passenger cùng camera; cần checklist "vẽ ego_body cho MỖI vị trí thân người trước khi vẽ box" | `submission/findings.csv` các dòng rule_id R07; `submission/r1_craft/annotations.xml` polygon ego_body sau rework; `submission/rework/delta.md` số spurious giảm 2→0 center |

**Giới hạn của kết luận từ ba frame ADASIND**: chỉ 20 in-scope box trên 3 frame của 1 slice B3-dense của 1 camera ADASIND (Ấn Độ). Không có mẫu Bus, ít mẫu Car, không có Pedestrian trong slice cần rework nhiều. Không kết luận được về cảnh đêm, mưa, biển báo. Zone edge chỉ 4 ref → mẫu quá nhỏ để chứng minh E4_model_domain.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ 200 frame ở `45_sampling_plan.csv`: (1) tránh chọn nhiều frame liên tiếp trong cùng cảnh (dùng khoảng cách tối thiểu ~30 frame theo timestamp để coi là ca độc lập); (2) mỗi camera front/rear/left/right cần ≥1 ca mỗi condition: ngày, đêm, mưa, ngược sáng, seam với camera lân cận; (3) hard slice phải có mỗi camera ≥1 ca có ≥2 object nhỏ zone edge để test khả năng model fisheye. Kế hoạch 200 frame này **chỉ giúp tìm ca cần soi**, chưa đo được tỷ lệ lỗi thật vì (a) 200/50000 = 0.4% không đủ representative theo phân phối condition thật, (b) chưa có label ground truth cho 200 frame — cần cycle qua gold set (46_gold_set_plan.md) trước khi báo số. Peer agreement 3 người trên slice ADASIND không thay reference đa người cho 4 camera.
