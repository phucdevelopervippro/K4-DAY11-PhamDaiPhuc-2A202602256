# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| adasind_167700.jpg (Edge zone) | 3 ca (WRONG_CLASS, MISSING, IGNORE_SCOPE) | Mật độ xe máy và người đi bộ đông đúc sát lề đường, nhầm lẫn giữa xe tải nhỏ SCV và ô tô con; mô hình YOLO26m bị mù hoàn toàn xe ba bánh ThreeWheeler ở mép kính fisheye | Dòng findings `r3_diag` (`L6+R6`, `L2+R4`), screenshot `submission/screenshots/adasind_167700_truck_scv.png` và quy tắc R04 |
| adasind_212280.jpg (Edge & Mid zone) | 2 ca (ATTRIBUTE, MISSING) | Xe buýt bị cắt khuyết ở mép kính mắt cá đứng sát dải viền đen `lens_border`, các phương tiện xa khó phân định ngưỡng $H=40\text{ px}$ | Dòng findings `r3_diag` (`L3+R3`), screenshot `submission/screenshots/adasind_212280_edge_bus.png` và cờ `truncated=true` theo R05/R06 |

Giới hạn của kết luận từ ba frame ADASIND: Lát cắt 3 frame chỉ cung cấp mẫu khảo sát nhỏ cục bộ trên một camera fisheye đơn phía trước, chưa đại diện cho toàn bộ 50.000 frame trong điều kiện thời tiết/ánh sáng đa dạng và chưa phản ánh được sai số cross-camera trên hệ thống SVM 360 độ.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi: Lấy mẫu dải đều giữa các khung giờ và điều kiện môi trường (normal/hard), áp dụng khoảng lùi thời gian (stride $\ge 30$ frame) để tránh đếm trùng lặp các frame liên tiếp cùng một ngữ cảnh. Kế hoạch này giúp phát hiện tối đa các ca biên góc khuất (Hard cases) cần soi kỹ, nhưng không dùng để tính toán tỷ lệ lỗi tổng thể (Error Rate) do mẫu không được phân bổ theo phân phối ngẫu nhiên đồng đều.
