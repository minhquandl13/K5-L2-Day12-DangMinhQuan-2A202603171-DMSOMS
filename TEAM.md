# TEAM — Ngày 12

Mã bộ ảnh: **G01** · Hình thức: **cá nhân**

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Đặng Minh Quân | 2A202603171 | có — 22/22 (`--expect 22`, 0 lỗi) | Nguyễn Tuấn Hưng (cùng mã G01, CVAT job 66, tài khoản `HungNguyen` / `hungnn91296@gmail.com`) | 40/40 dòng đã điền verdict + luật + hành động |

Ca hai bên vẫn không thống nhất và đã hỏi coach (ghi tên ảnh + đối tượng):

- `S06_ir` / `S06_rgb` · `object` · bạn có túi ở `rear_middle`, tôi không có → tôi thiếu, chưa kịp bổ sung
- `S08_ir` / `S08_rgb` · `infant_seat.facing` · tôi `rear`, bạn `front` → hướng ghế em bé trên ảnh OMS, cần coach chốt
- `D04` · `face.glasses` · tôi `none`, bạn `clear` → clear/none trên ảnh NIR, cần coach chốt
- `D03` · `face13.right_eye_*` · tôi `outside`, bạn `occluded` → hai cờ loại trừ nhau, tôi theo Q1

## Cá nhân

### DangMinhQuan-2A202603171
- Phần mình làm (ngoài 22 ảnh của mình): gán đủ 22 khung một mình (không chia việc), tự chạy cả ba checkpoint, tự soát export ngoài script để tìm hai lỗi script không bắt được (lật tên ghế và lệch giữa rgb với ir), viết `ir_log.csv` từ chính khung của mình, viết danh sách lớp v0 kèm hai câu hỏi leo thang. Phần mở rộng: gán S09–S10 trong task riêng và export thành `export_extra.zip`.
- Điều mình học được hôm nay: tên ghế thuộc về cái xe, còn trái/phải của mắt và tay thuộc về con người — hai quy ước ngược nhau nằm trong cùng một guideline; và `unknown` là một phán đoán về khả năng nhìn thấy, không phải giá trị mặc định cho an toàn.
