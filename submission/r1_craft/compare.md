# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_152940.jpg
- R2 center MISSING
- R5 mid MISSING
- R6 mid MISSING
## adasind_167700.jpg
- L8 mid IGNORE_SCOPE
- L9 mid IGNORE_SCOPE
- L2+R4 center WRONG_CLASS
- L5+R9 mid BOX_GEOMETRY
- R2 mid MISSING
- R8 center MISSING
## adasind_212280.jpg

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 10 | 7 | 3 | 1 |
| mid | 6 | 2 | 4 | 1 |
| edge | 2 | 2 | 0 | 0 |
