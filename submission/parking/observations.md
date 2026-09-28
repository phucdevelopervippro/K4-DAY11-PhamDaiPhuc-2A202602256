# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): Các vạch sơn trắng chia ô đỗ rõ nét ở tiền cảnh góc dưới và bên phải khung hình.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: Bỏ qua các vạch sơn mờ ở hậu cảnh xa gần chiếc ô tô màu đỏ và vạch phân làn lối xe chạy chung vì không phải ranh giới phân định ô đỗ riêng lẻ.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: Dừng trước bánh xe của xe màu đỏ ở hậu cảnh, trước hàng rào cây xanh và mép cỏ bao quanh bãi; không xuyên qua gầm xe hay vùng bị che khuất.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): Vết phản quang/nước loang và vết sơn mờ ở trung tâm bãi đỗ xe có cần gán ignore hay không.
