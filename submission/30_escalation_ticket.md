# Escalation ticket

## Ticket 1

- **Frame:** `adasind_167700.jpg` (đối tượng `L2+R4`, tọa độ $(x=586.5, y=880.1, w=113.8, h=108.2)$)
- **Ảnh chụp:** `submission/screenshots/adasind_167700_truck_scv.png`
- **Expected impact:** Ảnh hưởng đến độ chính xác phân loại phương tiện thương mại nhỏ (Small Commercial Vehicles - SCV) trên khoảng 15% tổng số frame trong tập dữ liệu fisheye đô thị ADASIND, làm suy giảm hiệu năng mô hình nhận diện dòng xe vận tải nhỏ.
- **Owner:** `guideline`
- **Recommendation:** Bổ sung ngay hình ảnh minh họa thực tế cho các dòng xe tải nhẹ thương mại (như Tata Ace, Mahindra Bolero pik-up) vào bảng phân loại R04 trong tài liệu hướng dẫn, quy định thống nhất là class `Truck` bất kể kích thước khung xe nhỏ tương đồng với `Car`.
