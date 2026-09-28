# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 10 | 3 | 1 | 2 | 3 | MISSING (2) |
| mid | 6 | 4 | 1 | 2 | 3 | MISSING (3) |
| edge | 2 | 0 | 0 | 2 | 3 | — |

## Nhận xét

- **Phân bố lỗi giữa các vùng:** Vùng Edge cho thấy sự đối lập rõ rệt nhất giữa nhãn người gán Annotator (L) và Model (M YOLO26m). Annotator gán chính xác $100\%$ đối tượng ở vùng Edge (0 missing, 0 spurious), trong khi Model M bị mù/mất dấu hoàn toàn (0 matched, 2 missing, 3 spurious). Nguyên nhân gốc rễ là do vùng Edge chịu độ cong méo fisheye cực đại làm phương tiện bị nén dạng vòng cung, kết hợp với vành đen `lens_border` cắt khuyết đối tượng. Mô hình YOLO26m gốc chưa được huấn luyện trên dữ liệu ống kính mắt cá nên bị suy giảm độ tin cậy nghiêm trọng ở vùng mép kính.
- **Tác động của ngưỡng IoU:** Khi tăng ngưỡng IoU từ 0.5 lên 0.7, số lượng match ở vùng center/mid giảm mạnh (từ 7 xuống 4 ở center). Lý do là trên ảnh fisheye gốc, bounding box hình chữ nhật của phương tiện bị cong viền có độ lệch hình học tự nhiên so với box phẳng chuẩn. Ở ngưỡng IoU cao (0.7), các sai lệch nhỏ về mép box do độ cong fisheye khiến bộ so khớp loại bỏ nhiều cặp box hợp lệ.
- **Giới hạn số đo:** Phân vùng center/mid/edge được tính toán thuần túy theo khoảng cách bán kính pixel từ vị trí đối tượng tới tâm vành kính $(cx, cy)$. Chỉ số này không đồng nhất với khoảng cách thực tế 3D (gần/xa) của xe trong không gian giao thông thực, vì một phương tiện ở gần xe ego nhưng nằm sát viền kính mắt cá vẫn bị xếp vào vùng Edge.
