# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm

- Khóa/lớp: VinUni AI20k - K4-L2
- Tên nhóm: Hang-Duan-Manh
- Repo Public: https://github.com/BuiHangvn/K4-L2-Day11-Hang-Duan-Manh-SVM360-Fisheye-Lab-Student
- Máy giữ hồ sơ chính / người quản lý: manh
- Slice chung lấy từ mode.json: B2-edge
- Tên định danh vai A dùng cho --self: manh
- Kênh trao đổi nội bộ: [Điền kênh: Zalo / Discord / Teams]
- Đại diện nộp (vai C): [Họ tên, MSSV của bạn C]
- Commit chốt bài: [Sẽ điền sau khi chốt P6]

## 2. Ba vai chính

| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Mạnh | [Điền MSSV] | manh | Parking/C0/slice B2-edge, self-QC, lock, rework | submission/r1_craft/lock.txt (Mã: 133E-FE7F), findings.csv |
| B · QA độc lập | [Điền tên bạn B] | [Điền MSSV] | [duan/hang] | Review trước reference, finding QA, kiểm lại ca sửa | submission/r2_qa/qa_review.md, findings.csv (round r2_qa) |
| C · Chẩn đoán & điều phối | [Điền tên bạn C] | [Điền MSSV] | [hang/duan] | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp | submission/r3_diag/, sampling_plan, decision_log, check |

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | mode.json, slice B2-edge | Kiểm tra slice chung và môi trường CVAT | Đã hoàn thành |
| P2 · Khóa bản đầu | A → B, C | submission/r1_craft/annotations.xml, lock.txt, mã: 133E-FE7F | Kiểm tra 18 box, 9 polygon, 9 checklist selfqc | Đã hoàn thành, bàn giao cho B |
| P3 · Chốt QA mù | B → C, A | qa_review.md, findings.csv (r2_qa), screenshots | [B điền sau khi chạy QA] | [Đang tiến hành] |
| P4 · Quyết định sửa | C → A, B | local_quality, model_compare, decision_log.csv | [C điền sau phân xử] | [Chờ P3] |
| P5 · Kiểm bản sửa | A → B → C | annotations-v2.xml, lock2.txt, delta.md | [B kiểm lại box đã sửa] | [Chờ P4] |
| P6 · Chốt nộp | A, B → C | manifest.json, commit chốt | [Cả nhóm duyệt trước khi nộp] | [Chờ P5] |

## 4. Bất đồng và phối hợp

- Một ca đã phân xử: [Sẽ điền ở P4: Frame/object/rule; ý kiến A/B; bằng chứng; quyết định]
- Ca còn mở: [Nếu không còn thì ghi "không có"]
- Đóng góp của A/B/C vào kế hoạch và exit ticket: [A hỗ trợ giải trình nhãn; B soát screenshot; C soạn kế hoạch]
- Thay đổi phân công nếu có: không có

## 5. Xác nhận trước khi nộp

- [ ] A xác nhận nhãn và export đúng phiên bản: Mạnh / submission/r1_craft/lock.txt
- [ ] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: [Tên B]
- [ ] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: [Tên C]
- [ ] manifest.json tại commit chốt có failed_gates rỗng.
- [ ] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [ ] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.
