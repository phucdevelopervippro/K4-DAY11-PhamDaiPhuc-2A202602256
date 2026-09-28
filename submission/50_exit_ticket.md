# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. **Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy tắc riêng? Vì sao?**
   Đây không phải lỗi `DUPLICATE` 2D thông thường. Mỗi camera fisheye trong hệ thống SVM có góc nhìn quang học và bộ tham số hiệu chỉnh (calibration) riêng biệt, do đó một vật thể vắt qua vùng chồng lấp (seam) xuất hiện dưới dạng 2 hình ảnh bị méo ở 2 tọa độ ảnh 2D local khác nhau là hiện tượng vật lý tự nhiên. Cần một quy tắc riêng: giữ nguyên 2 box 2D local trên từng camera để phục vụ huấn luyện detector local, đồng thời liên kết chúng bằng một `global_track_id` duy nhất trên không gian 3D/BEV. Lỗi `DUPLICATE` chỉ tính khi trên cùng một camera duy nhất xuất hiện 2 box chồng đè cho cùng 1 đối tượng.

2. **Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.**
   - **Giữ cùng Track ID:** Khi phương tiện di chuyển liên tục trong tầm nhìn của camera và thời gian bị che khuất tạm thời (occlusion) ngắn hơn ngưỡng quy định (ví dụ $<5$ frame).
   - **Thêm Keyframe:** Khi phương tiện có sự thay đổi đột ngột về hướng di chuyển, góc nén méo fisheye chuyển vùng (từ Center sang Edge), hoặc thay đổi thuộc tính (`truncated` / `occluded`).
   - **Trạng thái Outside:** Khi phương tiện di chuyển ra khỏi hoàn toàn vành tròn kính mắt cá hoặc bị che khuất $100\%$ không còn điểm đặc trưng.
   - **Bằng chứng cần thiết trước khi nối track qua hai camera:** (1) Đồng bộ mốc thời gian timestamp chính xác giữa các cảm biến; (2) Ma trận hiệu chỉnh ngoại tham số (Extrinsic Calibration Matrix) hợp lệ để chiếu tọa độ lên mặt phẳng BEV 3D; (3) Vectơ vận tốc và hướng di chuyển nhất quán của đối tượng tại ranh giới seam.

3. **Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`), bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?**
   - **Tình huống thực tế:** Tại frame `adasind_167700.jpg`, đối tượng `L2+R4`, tôi gán nhãn `Car` do nhìn từ xa thân xe khá nhỏ ngắn, trong khi Reference gán `Truck` vì xe có phần thùng chứa hàng phía sau (dạng xe tải nhỏ Tata Ace).
   - **Cách đã xử lý:** Tôi không tự ý sửa đè nhãn bản khóa mà giữ nguyên lập luận phân tích trong `findings.csv` với mã `why=E2_guideline_gap`, gắn cờ `action=escalate` và tạo ticket đề xuất quy chuẩn tại `submission/30_escalation_ticket.md`.
   - **Bài học & Thay đổi nếu làm lại:** Nếu làm lại slice này, tôi sẽ đối chiếu kỹ bảng quy chuẩn R04 ngay ở lượt gán nhãn đầu tiên (P1/P2) và kiểm tra các ví dụ đối chứng trong `assets/worked/index.html` để phân biệt chính xác giữa công năng thực tế của xe thương mại nhỏ với kích thước biểu kiến của thân xe trên ảnh méo fisheye.
