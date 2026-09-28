# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Ngã tư đông đúc, ngược sáng chiếu thẳng, giao lộ nhiều xe cắt ngang | Biến dạng rìa kính mắt cá nén ngang phương tiện; chói sáng làm nhầm nhãn rider và pedestrian | Bám trên không gian ảnh fisheye gốc (2D fisheye image space); giữ intrinsic/extrinsic calibration | Review độc lập 3 vai (Annotator, QA, Diagnostician), đối chiếu IoU 3D và chiếu thử lên mặt phẳng BEV |
| rear | Đèn pha chói lọi ban đêm từ xe phía sau, xe di chuyển bám đuôi cực gần (<1m) | Quầng sáng đèn pha làm phình box quá rộng; phương tiện bị cắt khuyết bởi vành kính tròn bên dưới | Thân xe ego cản sau; giữ mặt phẳng ground calibration | Duyệt thủ công 2 dải viền kính, kiểm tra cờ truncated=true bám sát viền xe thật |
| left | Xe máy tạt hông sát xe ego, phương tiện vắt qua vùng seam giữa camera trước và hông trái | Xuất hiện đồng thời trên 2 camera lân cận với góc méo co giãn chiều ngang lớn gây trùng nhãn | Vùng chồng lấp seam area; camera extrinsic pose trên gương trái | Ghép nối đồng thời 4 camera tại mốc $t$, kiểm tra tính nhất quán Global Object ID cross-camera |
| right | Xe hai bánh/người đi bộ nhô ra đột ngột từ góc khuất ngõ hẹp và xe đỗ lề đường | Khuất tầm nhìn bởi chướng ngại vật cố định, độ phân giải thấp ở rìa kính fisheye | Góc mù lề phải; camera extrinsic pose trên gương phải | Duyệt chuỗi thời gian video sequence inspection để không bỏ sót frame bắt đầu xuất hiện |

- **Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):** Cần tiến hành refresh gold set khi: (1) Thay đổi thông số phần cứng hệ thống camera (thay ống kính, đổi vị trí gắn hoặc thay đổi độ phân giải cảm biến); (2) Hiệu chỉnh lại thông số calibration extrinsic sau quá trình bảo trì/vận hành xe; (3) Bump phiên bản luật gán nhãn mới (ví dụ nâng cấp từ `rules_version` `v1.0.0` lên `v1.1.0`).
- **Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:** Khi một phương tiện vắt qua vùng giao nhau (seam) của 2 camera lân cận (ví dụ camera Front và camera Left), policy quy định tuyệt đối không tự động gộp thành 1 box 2D chung. Hệ thống phải giữ nguyên 2 box 2D local trên từng ảnh camera gốc kèm theo chứng cứ: (1) Ma trận chiếu 3D/BEV trùng khớp tọa độ thực; (2) Timestamp đồng bộ hoàn hảo; (3) Gán chung mã định danh `global_track_id`.
- **Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:** Đánh giá chất lượng trên 1 camera đơn lẻ chỉ chứng minh tính đúng đắn trên không gian 2D local của camera đó, hoàn toàn không phát hiện được các lỗi hệ thống 360 độ như: trùng lặp phương tiện giữa 2 camera lân cận ở vùng seam, lệch tọa độ chiếu không gian 3D/BEV do sai số calibration ngoại tham số extrinsic, và sự đứt gãy chuỗi vết vết di chuyển (Tracking ID breakdown) khi xe di chuyển quanh vòng xe.
