# QA review · B3-center

Mã khóa: 1B14-D134

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_152940.jpg | L_ego_body | R08, R09 | Polygon ignore_region (ego_body) tại góc dưới bên trái bao kín tay lái/gương xe camera, không đè quá 50% vào box các phương tiện lưu thông (L1, L2, L3). |
| adasind_167700.jpg | L6 | R04, R06 | Phân loại đúng ThreeWheeler (L6) ở sát vành kính mắt cá với thuộc tính truncated=true; ghép đôi tách biệt xe thồ (L5 Bike) và người đẩy xe (L7 Pedestrian). |
| adasind_212280.jpg | L3 | R01, R06 | Các phương tiện xa (L1, L2) đáp ứng ngưỡng chiều cao H >= 40px; phương tiện L3 Bus bị viền kính mắt cá cắt được bật đúng cờ truncated=true. |
