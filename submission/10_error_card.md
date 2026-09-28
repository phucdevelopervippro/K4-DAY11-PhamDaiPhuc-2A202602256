# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B3 | MISSING | 7 |
| center | B3 | SPURIOUS | 4 |
| center | B3 | WRONG_CLASS | 1 |
| edge | B3 | ATTRIBUTE | 1 |
| edge | B3 | MISSING | 2 |
| edge | B3 | SPURIOUS | 3 |
| edge | B3 | WRONG_CLASS | 1 |
| mid | B3 | BOX_GEOMETRY | 1 |
| mid | B3 | IGNORE_SCOPE | 2 |
| mid | B3 | MISSING | 7 |
| mid | B3 | SPURIOUS | 4 |
| mid | C0 | MISSING | 1 |
| unknown | B3 | IGNORE_SCOPE | 1 |

## Top defects
- MISSING: 17 (ví dụ frame adasind_019560.jpg)
- SPURIOUS: 11 (ví dụ frame adasind_152940.jpg)
- IGNORE_SCOPE: 3 (ví dụ frame adasind_152940.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:** Lỗi nổi bật nhất trong tập dữ liệu là `MISSING` (17 lượt) và `SPURIOUS` (11 lượt). 
  1. Với `MISSING`: Xuất hiện chủ yếu do hiện tượng Domain Gap của mô hình M (YOLO26m chưa được fine-tune trên dữ liệu ống kính mắt cá fisheye), dẫn đến việc mô hình hoàn toàn không nhận diện được phương tiện bị biến dạng cong ở vùng Edge (ví dụ `adasind_167700.jpg` đối tượng `L6` ThreeWheeler và `adasind_212280.jpg` đối tượng `L3` Bus). Ở phía Annotator, lỗi missing xảy ra ở vùng Mid/Edge với các phương tiện nhỏ/mờ ở khoảng cách xa sát ngưỡng $H = 40\text{ px}$.
  2. Với `SPURIOUS`: Do mô hình M nhầm lẫn các vết bóng phương tiện, vạch sơn bãi đỗ và hạ tầng lề đường (lùm cây, cột điện) thành các phương tiện giao thông do chưa nhận biết được dải viền mờ ignore `lens_border`.
- **Cách sửa và ai nhận việc (`owner`):**
  1. `ai_team`: Thực hiện thu thập dữ liệu ảnh fisheye đa dạng, huấn luyện bổ sung (fine-tune) mô hình YOLO với các kỹ thuật Augmentation đặc thù cho ống kính mắt cá (fisheye distortion/undistortion simulation) và tích hợp mask `ignore_region` (`lens_border`, `ego_body`) vào pipeline suy luận.
  2. `annotator`: Tiến hành quy trình tự soát Self-QC 2 lượt đối với các phương tiện nhỏ xa ở dải Mid/Edge theo đúng ngưỡng R01 ($H \ge 40\text{ px}$).
  3. `guideline`: Ban hành tài liệu patch hướng dẫn chi tiết quy chuẩn phân loại xe tải nhỏ thương mại (Tata Ace) theo R04 để tránh nhầm lẫn giữa `Car` và `Truck`.
- **Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule):**
  1. Dòng finding `r3_diag` trong `findings.csv`: frame `adasind_167700.jpg` đối tượng `L6+R6` (`what=MISSING`, `why=E4_model_domain`, `rule_id=R04`), frame `adasind_212280.jpg` đối tượng `L3+R3` (`what=MISSING`, `why=E4_model_domain`, `rule_id=R04`).
  2. Ảnh minh họa trong `submission/screenshots/adasind_167700_truck_scv.png` và `submission/screenshots/adasind_212280_edge_bus.png`.
  3. Quy tắc áp dụng: R01 (ngưỡng chiều cao), R04 (bảng 6 class phương tiện), R06 (quản lý ignore_region).
