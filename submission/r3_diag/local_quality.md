# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `1b14d134058adb8e0197e654480848a81b917ca025fe5eff497cbc7e9a8a5763`; slice `B3-center`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_152940.jpg, adasind_167700.jpg, adasind_212280.jpg. Frame thiếu trong export: không.
TP=11; FP=2; FN=7; số lần đối chiếu=19; mean IoU của TP=0.811.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.579 | 0.921 | 0.789 |
| precision | 0.846 | 0.750 | 0.000 |
| recall | 0.611 | 0.604 | 0.000 |
| jaccard | 0.550 | 0.576 | 0.000 |
| dice | 0.710 | 0.659 | 0.000 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 5 | 1 | 3 | 0.789 | 0.833 | 0.625 | 0.556 | 0.714 |
| Bus | 1 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Car | 0 | 0 | 1 | 0.947 | 0.000 | 0.000 | 0.000 | 0.000 |
| Pedestrian | 2 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 1 | 0 | 1 | 0.947 | 1.000 | 0.500 | 0.500 | 0.667 |
| Truck | 2 | 1 | 2 | 0.842 | 0.667 | 0.500 | 0.400 | 0.571 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_152940.jpg | 3 | 0 | 3 | 0.500 | 1.000 | 0.500 |
| adasind_167700.jpg | 5 | 2 | 4 | 0.500 | 0.714 | 0.556 |
| adasind_212280.jpg | 3 | 0 | 0 | 1.000 | 1.000 | 1.000 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Bus | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|---:|
| Bike | 5 | 0 | 0 | 0 | 0 | 0 | 3 |
| Bus | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| Car | 0 | 0 | 0 | 0 | 0 | 1 | 0 |
| Pedestrian | 0 | 0 | 0 | 2 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 0 | 0 | 1 | 0 | 1 |
| Truck | 0 | 0 | 0 | 0 | 0 | 2 | 2 |
| <extra> | 1 | 0 | 0 | 0 | 0 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
