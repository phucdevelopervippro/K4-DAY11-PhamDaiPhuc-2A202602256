# Guideline patch

- **Rule mới đề xuất:** 
  1. **Quy tắc gán vật thể bị cắt bởi mép kính tròn fisheye (Edge truncation):** Đối với các phương tiện nằm vắt qua viền kính mắt cá, bám sát ranh giới phần nhìn thấy được bên trong vành tròn kính, bật thuộc tính `truncated=true`. Tuyệt đối không vẽ box vượt ra ngoài dải đen viền kính `lens_border`. Nếu phương tiện bị dải viền kính che quá 50% diện tích, matcher sẽ tự động bỏ qua (don't-care).
  2. **Quy tắc ranh giới `ignore_region` (`ego_body`):** Polygon `ego_body` phải bao phủ hoàn toàn phần tay lái, gương chiếu hậu và thân xe ego ở góc dưới bên trái/phải. Polygon `ego_body` tuyệt đối không được chồng lấp quá 50% diện tích lên bất kỳ bounding box của phương tiện giao thông đang di chuyển.
  3. **Quy tắc xe thồ và phương tiện thương mại nhỏ (SCV):** Xe tải nhỏ thùng hở/thùng kín (Tata Ace, Mahindra Bolero pik-up) quy định cố định là class `Truck` (R04). Người đẩy/dắt xe hai bánh phải được gán thành 2 nhãn riêng biệt: 1 box `Pedestrian` cho người và 1 box `Bike` cho xe.
- **Áp dụng cho:** `Car`, `Truck`, `ThreeWheeler`, `Bike`, `Pedestrian`, `ignore_region.reason` (`ego_body`, `lens_border`, `crowd_or_group`).
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật hiện tại v1.0.0 chưa quy định cụ thể ranh giới phủ tối đa giữa polygon `ego_body` và box phương tiện lưu thông sát cạnh xe, gây ra tình trạng tranh chấp IoU giữa QA và Annotator; đồng thời v1.0.0 chưa làm rõ định danh xe tải nhỏ dạng pickup dẫn tới tỷ lệ gán nhầm sang `Car` cao.
- **`rules_version` mới:** `v1.1.0`
- **Hiệu lực từ:** Vòng gán nhãn rework và các đợt kiểm thử QA tiếp theo (`r3_diag` / `rework`).
