# QA review · B3-dense

Mã khóa: E465-3F46

Cold review chính bản của mình do Duẩn chưa lock B1-dense trong khung thời gian; theo docs/03-roles-rotation-vi.md, thay peer review bằng cold review sau khi nghỉ ≥5 phút. Chỉ dùng rules v1.0.0 (docs/02-rules-vi.md), chưa mở reference/model.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_145860.jpg | L2 | R04 | Box `Truck` (237,853)-(282,910) rất nhỏ (45×57) ở giữa đường phía xa; nhìn ảnh có 1 vật màu vàng-cam trên đường trông giống auto-rickshaw hơn là xe tải — nghi class Truck, có thể phải là `ThreeWheeler` theo R04. |
| adasind_167700.jpg | L4 | R04 | Box `Truck` (9,915)-(261,1174) ở góc trái đáy — mảng lớn 252×259; nhìn ảnh vùng đó có auto-rickshaw màu vàng khá rõ. Nghi class Truck sai — auto-rickshaw phải là `ThreeWheeler` (R04). |
| adasind_167700.jpg | L5 | R02 | Box `Pedestrian` (906,723)-(1080,1420) cao 697px, chạm mép phải, truncated=true. Nhìn ảnh, người đứng cầm xe đạp phía phải chỉ chiếm khoảng nửa dưới; box có vẻ ôm rộng hơn phần thấy được. Cần kiểm geometry R02 có bám sát phần visible không. |
| adasind_167700.jpg | L6+L7+L8 | R04 | Ba box `Truck` liền nhau ở giữa xa đường (466-699 x 868-1004). Nhìn ảnh, cụm xe giữa xa gồm auto-rickshaw và có thể minibus. Nghi ≥1 box đang là Truck nhưng thực chất phải là `ThreeWheeler` theo R04. |
| adasind_167700.jpg | L3 | R03 | Box `Pedestrian` (350,875)-(480,1213) là người áo sọc dắt xe đạp; L10 `Bike` (376,971)-(587,1186) là chiếc xe đạp. Cặp Pedestrian+Bike tách đúng R03 (người dắt = 2 box tách), soát tay OK. Ghi để không bị QA khác nhầm là trùng. |
| adasind_199770.jpg | L4 | R07 | Box `Truck` (928,796)-(1080,1310) truncated=true, cao 500px chạm mép phải. Nhìn ảnh, vùng phải đáy có người ngồi cùng camera (áo/váy). Box này nằm ngay trên polygon `ego_body` phải mới vẽ — nghi ngờ box đang ôm ego_body chứ không phải xe tải thật. Cần kiểm R07 (ego_body scope) và R10 (P0). |
| adasind_199770.jpg | L6 | R04 | Box `Truck` (364,804)-(531,944) ở giữa ảnh — vùng có auto-rickshaw đỏ có mái. Nghi class Truck sai, phải là `ThreeWheeler` (R04). |
| adasind_199770.jpg | L3 | R03 | Box `Pedestrian` (610,828)-(706,1025) là người áo trắng đứng cạnh rickshaw đỏ. Theo R03, nếu người đứng cạnh (không lái) thì cần cặp Pedestrian + phương tiện tách. Chưa thấy box ThreeWheeler ôm chính chiếc rickshaw đỏ cạnh người (nếu L6 là chiếc đó thì class phải sửa). Cần soát cặp tách. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
