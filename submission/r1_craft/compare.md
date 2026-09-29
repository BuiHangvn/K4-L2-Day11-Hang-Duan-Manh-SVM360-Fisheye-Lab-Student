# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_145860.jpg
- L2 mid IGNORE_SCOPE
## adasind_167700.jpg
- L5 mid IGNORE_SCOPE
- L1+R9 mid BOX_GEOMETRY
- L4+R6 edge WRONG_CLASS
- L8+R4 center WRONG_CLASS
## adasind_199770.jpg
- L4 mid IGNORE_SCOPE
- L1+R5 edge BOX_GEOMETRY
- L6+R8 center WRONG_CLASS
- R3 mid MISSING
- R4 mid MISSING
- R6 edge MISSING
- R7 mid MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 9 | 7 | 2 | 2 |
| mid | 7 | 3 | 4 | 1 |
| edge | 4 | 1 | 3 | 2 |
