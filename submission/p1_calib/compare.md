# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_019560.jpg
- L7+R4 mid ATTRIBUTE
- L5 center SPURIOUS
- L6 center SPURIOUS
- L8+R5 center WRONG_CLASS

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 3 | 2 | 1 | 3 |
| mid | 2 | 2 | 0 | 0 |
| edge | 1 | 1 | 0 | 0 |
