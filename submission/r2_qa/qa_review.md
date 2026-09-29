# QA review · B2-edge

**Mã khóa:** `133E-FE7F`  
**Vai:** B – QA độc lập (Duẩn), kiểm bản XML R1 do Mạnh khóa.  
**Nguồn:** ba ảnh gốc trong `assets/images/`, `submission/r2_qa/qa_overlay.html`, `submission/r1_craft/annotations.xml`, `docs/02-rules-vi.md` (v1.0.0).  
**Phạm vi:** 3 frame; 18 box (5 + 7 + 6). Không dùng teaching reference hoặc model để đưa ra nhận xét P3.

## Kết quả rà soát từng box

| frame | object_ref | rule_id | nhận xét | ảnh bằng chứng |
|---|---|---|---|---|
| adasind_069450.jpg | L1 ThreeWheeler | R02, R04 | Một xe ba bánh phía trước bên trái; overlay có box bao vùng xe nhìn thấy. Chưa thấy lỗi rõ từ ảnh gốc. | [Ảnh gốc](../../assets/images/adasind_069450.jpg) · [Overlay](qa_overlay.html) |
| adasind_069450.jpg | L2 Pedestrian | R01, R02 | Người thứ nhất trong cụm bên phải, cao hơn ngưỡng 40 px; box tách riêng. Chưa thấy lỗi rõ. | [Ảnh gốc](../../assets/images/adasind_069450.jpg) · [Overlay](qa_overlay.html) |
| adasind_069450.jpg | L3 Pedestrian | R01, R02 | Người thứ hai trong cụm bên phải, box tách riêng; chưa thấy lỗi rõ. | [Ảnh gốc](../../assets/images/adasind_069450.jpg) · [Overlay](qa_overlay.html) |
| adasind_069450.jpg | L4 Pedestrian | R01, R02 | Người thứ ba trong cụm bên phải, box tách riêng; chưa thấy lỗi rõ. | [Ảnh gốc](../../assets/images/adasind_069450.jpg) · [Overlay](qa_overlay.html) |
| adasind_069450.jpg | L5 ThreeWheeler | R02, R05 | Xe ba bánh sát trái có box và `occluded=true`, `truncated=false` trong XML; ảnh có vật thể tiền cảnh nên không kết luận thuộc tính che khuất sai. Cần người C phân xử nếu muốn thay đổi hình học. **Lỗi đã xác nhận trong hồ sơ:** finding `r1_craft` ghi `BOX_GEOMETRY` nhưng dẫn `R05` thay vì quy tắc hình học `R02`. | [Ảnh gốc](../../assets/images/adasind_069450.jpg) · [Overlay](qa_overlay.html) · [XML](../r1_craft/annotations.xml) |
| adasind_082170.jpg | L1 Bike | R02, R03 | Xe hai bánh sát phía trái, `occluded=true`; box được ghi trong XML. Cần giữ phân biệt với xe bên cạnh L6 khi đối chiếu gần, chưa kết luận trùng box. | [Ảnh gốc](../../assets/images/adasind_082170.jpg) · [Overlay](qa_overlay.html) |
| adasind_082170.jpg | L2 ThreeWheeler | R02, R04 | Xe ba bánh màu vàng phía trái; box bám vùng xe thấy được, chưa thấy lỗi rõ. | [Ảnh gốc](../../assets/images/adasind_082170.jpg) · [Overlay](qa_overlay.html) |
| adasind_082170.jpg | L3 Bike | R01, R03 | Xe hai bánh nhỏ phía trái tâm ảnh; box cao khoảng 68 px, không vi phạm H=40. Chưa thấy lỗi rõ. | [Ảnh gốc](../../assets/images/adasind_082170.jpg) · [Overlay](qa_overlay.html) |
| adasind_082170.jpg | L4 Bike | R03 | Có một box bao gồm người lái và xe hai bánh ở vùng giữa lệch phải, không thấy box Pedestrian trùng rider. **Lỗi đã xác nhận trong hồ sơ:** finding `r1_craft` dẫn `R04` cho quy tắc gộp rider; đúng phải dẫn `R03`. | [Ảnh gốc](../../assets/images/adasind_082170.jpg) · [Overlay](qa_overlay.html) · [XML](../r1_craft/annotations.xml) |
| adasind_082170.jpg | L5 ThreeWheeler | R02, R04 | Xe ba bánh màu xanh lớn bên phải; box bao phần xe nhìn thấy, chưa thấy lỗi rõ. | [Ảnh gốc](../../assets/images/adasind_082170.jpg) · [Overlay](qa_overlay.html) |
| adasind_082170.jpg | L6 Bike | R02, R03 | Box xe hai bánh bên trái nằm gần L1, có `occluded=true`. Ảnh cho thấy phương tiện ở vị trí kế bên nhưng cần ảnh phóng lớn nếu nhóm muốn phân xử quan hệ với L1; chưa đủ căn cứ ghi DUPLICATE. | [Ảnh gốc](../../assets/images/adasind_082170.jpg) · [Overlay](qa_overlay.html) |
| adasind_082170.jpg | L7 Bike | R02, R03 | Xe hai bánh ở mép đường bên phải, `occluded=true`; chưa thấy lỗi nhãn rõ. | [Ảnh gốc](../../assets/images/adasind_082170.jpg) · [Overlay](qa_overlay.html) |
| adasind_102750.jpg | L1 ThreeWheeler | R01, R04 | Xe ba bánh nhỏ phía trái, box cao khoảng 62 px; chưa thấy lỗi rõ. | [Ảnh gốc](../../assets/images/adasind_102750.jpg) · [Overlay](qa_overlay.html) |
| adasind_102750.jpg | L2 Truck | R02, R05 | Xe tải bị cắt bởi biên phải (`xbr=1080`), XML đã đặt `truncated=true`; **không có lỗi thiếu truncated**. **Lỗi hồ sơ:** finding `r1_craft` dẫn `R07` (ego_body) thay vì `R05` (truncated). | [Ảnh gốc](../../assets/images/adasind_102750.jpg) · [Overlay](qa_overlay.html) · [XML](../r1_craft/annotations.xml) |
| adasind_102750.jpg | L3 ThreeWheeler | R01, R04 | Xe ba bánh nhỏ cạnh L1, box cao khoảng 45 px; `occluded=true`. Chưa thấy lỗi rõ. | [Ảnh gốc](../../assets/images/adasind_102750.jpg) · [Overlay](qa_overlay.html) |
| adasind_102750.jpg | L4 ThreeWheeler | R02, R05 | Xe ba bánh sát biên trái (`xtl=0`), XML đã đặt `truncated=true`; chưa thấy lỗi attribute. | [Ảnh gốc](../../assets/images/adasind_102750.jpg) · [Overlay](qa_overlay.html) |
| adasind_102750.jpg | L5 ThreeWheeler | R01, R04 | Xe ba bánh nhỏ phía trái, cao khoảng 58 px; chưa thấy lỗi rõ. | [Ảnh gốc](../../assets/images/adasind_102750.jpg) · [Overlay](qa_overlay.html) |
| adasind_102750.jpg | L6 ThreeWheeler | R01, R04 | Xe ba bánh nhỏ gần giữa, cao khoảng 45 px; chưa thấy lỗi rõ. | [Ảnh gốc](../../assets/images/adasind_102750.jpg) · [Overlay](qa_overlay.html) |

## Finding đã xác nhận ở vòng r2_qa

| frame | object_ref | cell | what | rule_id | kết luận và bằng chứng |
|---|---|---|---|---|---|
| adasind_069450.jpg | L5 | L_only | STRUCTURE | R02 | Dòng `r1_craft` mô tả hình học nhưng trích nhầm R05. Đối chiếu `submission/findings.csv` và `docs/02-rules-vi.md`. |
| adasind_082170.jpg | L4 | L_only | STRUCTURE | R03 | Dòng `r1_craft` về rider/Bike trích nhầm R04; quy tắc đúng là R03. |
| adasind_102750.jpg | L2 | L_only | STRUCTURE | R05 | Dòng `r1_craft` về truncated trích nhầm R07; quy tắc đúng là R05; XML đã bật truncated. |

Ba finding này là **lỗi trích dẫn quy tắc trong hồ sơ P2**, không phải ba lỗi gán nhãn đã được chứng minh. Chúng được ghi theo mẫu `r2_qa`, `cell=L_only`, `what=STRUCTURE`, `severity=P3`; cột `why` để trống để người C chẩn đoán ở P4. **Không sửa bản XML đã khóa** và không ghi sai lỗi attribute cho các trường hợp XML đã đặt đúng.

## Bàn giao

Đã rà soát ba ảnh và đủ 18 box bằng ảnh gốc, overlay và XML mã `133E-FE7F`; ghi nhận ba lỗi trích dẫn rule cần Hằng đối chiếu và chỉnh trong hồ sơ sau khi chốt P3. Các nghi vấn về hình học L5 ảnh 069450 và sự phân biệt L1/L6 ảnh 082170 **chưa được kết luận là lỗi**; nếu nhóm muốn xử lý, cần chụp ảnh phóng lớn hoặc phân xử tại P4. Giữ nguyên 4 dòng Calibration và 3 dòng `r1_craft` của Mạnh.