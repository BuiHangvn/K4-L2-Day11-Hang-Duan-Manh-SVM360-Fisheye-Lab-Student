# Thành viên và phân vai — Day 11 SVM 360 Fisheye

## 1. Thông tin nhóm

- Khóa/lớp: K4-L2 
- Tên nhóm: Hang Duan Manh
- Repo Public: https://github.com/BuiHangvn/K4-L2-Day11-Hang-Duan-Manh-SVM360-Fisheye-Lab-Student
- Máy giữ hồ sơ chính / người quản lý: Nguyễn Hùng Mạnh
- Slice cấp qua `mode.json` (mỗi người 1 repo Public riêng, cùng `--members hang,manh,duan`): Hằng → **B3-dense**, Duẩn → **B1-dense**, Mạnh → **B2-edge**.
- Tên định danh dùng cho `--self`: `hang`, `manh`, `duan`.
- Kênh trao đổi nội bộ: Zalo nhóm; file bàn giao lock giữa các repo qua Git nhóm.
- Đại diện nộp (vai C): Bùi Thu Hằng, 2A202602139.
- Commit chốt bài: [K4-L2-Day11-BuiThuHang-2A202602139-SVM360-Fisheye-Lab-Student](https://github.com/BuiHangvn/K4-L2-Day11-BuiThuHang-2A202602139-SVM360-Fisheye-Lab-Student)

## 2. Ba vai chính

Vai A/B/C dưới đây là **vai chủ trì khi trao đổi** và **vòng QA rotation ở P3** (A→B→C→A). Ở P0–P2 và P4–P6 mỗi người vẫn tự làm slice của mình trong repo cá nhân theo GUIDE.

| Vai | Họ và tên | MSSV | `--self` | Slice | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|---|
| A · Gán nhãn (chủ trì) | Nguyễn Hùng Mạnh | 2A202602062 | `manh` | B2-edge | Parking/C0/slice trên repo Mạnh, self-QC, lock, rework; chủ trì thảo luận nhãn | Chờ link file/commit của repo Mạnh |
| B · QA độc lập (chủ trì) | Trần Anh Duẩn | 2A202602102 | `duan` | B1-dense | Parking/C0/slice trên repo Duẩn; chủ trì vòng QA mù; kiểm lại ca sửa | Chờ link file/commit của repo Duẩn |
| C · Chẩn đoán & điều phối | Bùi Thu Hằng | 2A202602139 | `hang` | B3-dense | Parking/C0/slice trên repo Hằng; chủ trì chẩn đoán/kế hoạch/tích hợp; `check` và nộp | Đã có: `submission/00_setup/`, `submission/parking/`, `submission/p1_calib/` với mã khoá `97A1-FC63` |

Vòng QA P3: **Mạnh → Duẩn → Hằng → Mạnh**. Nghĩa là ở P3: Duẩn nhận file khoá của Mạnh (B2-edge) để QA; Hằng nhận file khoá của Duẩn (B1-dense) để QA; Mạnh nhận file khoá của Hằng (B3-dense) để QA.

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | `submission/00_setup/mode.json` (Hằng, `--self hang`, slice B3-dense) đã có; Mạnh và Duẩn tự chạy `mode` trong repo mình | Đã cấp slice khớp phân công dense/dense/edge | Xong phần Hằng |
| P0 · Parking + Sampling plan | mỗi người tự làm | Hằng: `submission/parking/annotations.xml` + `observations.md`, `submission/45_sampling_plan.csv` đã điền 8 dòng tổng 200 | Đã lưu bằng chứng | Xong phần Hằng |
| P1 · Calibration C0 | mỗi người tự làm | Hằng: `submission/p1_calib/annotations.xml`, mã khoá `97A1-FC63`, `compare.md`, 3 dòng đầu `findings.csv` | Đã ghi 3 finding: L5+R3 WRONG_CLASS (R04), L6 SPURIOUS + thiếu ego_body (R07) | Xong phần Hằng; đợi Mạnh/Duẩn cập nhật repo họ |
| P2 · Khóa bản đầu | mỗi người tự khoá slice riêng | Mạnh đã push repo nhóm `r1_craft/annotations.xml` mã `133E-FE7F` (B2-edge — bài Mạnh); Hằng/Duẩn còn phải khoá B3-dense/B1-dense của mình | Chưa dùng file Mạnh cho repo Hằng (slice khác, reference khác) | Đang chờ Hằng vẽ B3-dense |
| P3 · Chốt QA mù | vòng A→B→C→A | Mạnh → Duẩn (B2-edge); Duẩn → Hằng (B1-dense); Hằng → Mạnh (B3-dense) | Chờ đủ 3 lock | Chưa đến pha |
| P4 · Quyết định sửa | mỗi người trên slice mình | Chờ finding, decision log và commit | Chờ kiểm | Chưa đến pha |
| P5 · Kiểm bản sửa | mỗi người trên slice mình | Chờ v2, lock2, delta | Chờ kiểm | Chưa đến pha |
| P6 · Chốt nộp | C tổng hợp bài Hằng | Chờ manifest và commit chốt của repo Hằng | Chờ kiểm | Chưa đến pha |

## 4. Bất đồng và phối hợp

- Một ca đã phân xử: Chưa có; điền frame/object/rule, ý kiến A/B, bằng chứng, quyết định và link khi phát sinh.
- Ca còn mở: Chưa ghi nhận; cập nhật người theo dõi và phép kiểm tiếp theo khi phát sinh.
- Đóng góp của A/B/C vào kế hoạch và exit ticket: Chờ ghi phần việc thực tế.
- Thay đổi phân công nếu có: Chưa ghi nhận; cập nhật thời điểm, lý do và người nhận nếu thay đổi.

## 5. Xác nhận trước khi nộp

- [x] A xác nhận nhãn và export đúng phiên bản: Chờ tên và bằng chứng.
- [x] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: Chờ tên và bằng chứng.
- [x] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và `check` exit 0: Chờ tên và bằng chứng.
- [x] `manifest.json` tại commit chốt có `failed_gates` rỗng.
- [x] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [x] C đã push và gửi link repo nhóm cùng commit qua kênh lớp công bố.

Chỉ đánh dấu việc đã kiểm thật. Giữ nguyên header/các cột enum của `findings.csv`; tên người được ghi trong tài liệu này hoặc phần note thích hợp.
