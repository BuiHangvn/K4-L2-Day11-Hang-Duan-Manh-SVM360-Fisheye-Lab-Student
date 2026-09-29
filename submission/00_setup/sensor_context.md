# Sensor context

- Rig: Camera fisheye lắp trên phương tiện hai/ba bánh kiểu open-air (auto-rickshaw hoặc
  xe máy chở khách) dùng cho dataset ADASIND, hướng nhìn về phía trước. Không có capo,
  gương hoặc táp lô ô tô con xuất hiện trong khung; các frame slice B3-dense (145860,
  167700, 199770) đều cho thấy góc nhìn thấp gần mặt đường. Chưa có tài liệu rig chính
  thức nên chỉ ghi theo quan sát.
- `ego_body` nhìn thấy: trong 3 frame của slice, phần thân/tay/chân của người ngồi cùng
  camera xuất hiện ở **đáy-trái** khung hình (145860: chân + giày rõ; 167700: vai + tay
  áo caro; 199770: tay + vai áo caro). Frame 199770 còn có thêm một mảng thân người ở
  **đáy-phải** (áo/váy hành khách). Không thấy capo, gương chiếu hậu hay vô lăng — hai
  frame 006840 và 271039 (nêu trong GUIDE) không thuộc slice này.
- Vòng kính (lens circle): hình gần tròn nằm giữa khung hình, chiếm khoảng 90–95%
  chiều rộng và 85–90% chiều cao ảnh; bốn góc khung hình là vùng vignette đen do
  ống kính không phủ tới. Rìa vòng kính có méo (barrel distortion) mạnh, vật ở gần
  mép bị kéo dài; ảnh 145860 cho thấy rõ đường chân trời cong theo vòng kính.
