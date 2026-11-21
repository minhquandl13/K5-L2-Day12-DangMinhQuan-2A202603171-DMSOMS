# Báo cáo Lab Ngày 12 — Gán nhãn cabin DMS & OMS (bộ ảnh G01)

**Học viên:** Đặng Minh Quân · **MSSV** 2A202603171 · **Hình thức:** cá nhân
**Bộ ảnh:** G01 (SVIRO, BMW X5 · Drive&Act NIR) · **Guideline:** Cabin v1.0
**Phạm vi nộp:** E1–E5 của thư mục `K5-L2-DAY12-DangMinhQuan-2A202603171-DMSOMS`

---

## 1. Mục tiêu

Gán nhãn 22 khung hình cabin theo **một** guideline chung, để cùng một bộ luật áp được cho cả giám sát hàng ghế sau (OMS) lẫn giám sát người lái (DMS); sau đó tự kiểm, đối chiếu với bạn, rồi chuyển danh mục nhãn thành bản nháp danh sách lớp cho model DMS/OMS.

Hai bộ ảnh trong G01 kéo theo hai hướng ngược nhau — đó chính là phần khó thật sự của buổi lab:

- **SVIRO** là ảnh dựng. Ảnh `_ir` của bộ này là IR **mô phỏng** (kênh đỏ dùng làm độ sáng), nên không hề có chói LED, mắt sáng hay ánh sáng yếu dần theo khoảng cách. Trong SVIRO **không ai thắt dây an toàn**.
- **Drive&Act** là NIR thật, bị xoay khoảng 90° theo hướng lắp camera, có hiện tượng IR thật, và người lái đang làm việc khác trong xe thử nghiệm chạy chế độ tự lái.

## 2. Dữ liệu

| Tập con | Nguồn | Số khung | Độ phân giải | Loại ảnh |
|---|---|---|---|---|
| S01–S08 | SVIRO (dựng), hàng ghế sau 3 chỗ | 8 RGB + 8 IR-sim = **16** | 960 × 640 | rgb / ir (mô phỏng) |
| D01–D06 | Drive&Act, `a_column_driver`, NIR | **6** | 1280 × 1024 | ir (thật) |
| S09–S10 (mở rộng) | SVIRO, task riêng `Day12-G01-extra` | 4 | 960 × 640 | rgb / ir (mô phỏng) |
| **Tổng cốt lõi** | | **22** | | |

## 3. Cách làm

Trên mỗi khung, thứ tự cố định: tag `frame_meta` trước (modality, light, camera_blocked) → box OMS → chuỗi DMS (`person` → `face` → `face13` → `hand` → `phone`) → lưu. Hai quy ước gọi tên được giữ tách biệt có chủ ý:

- **Ghế gọi theo xe.** Camera OMS đặt ở táp-lô nhìn về phía sau xe, nên ảnh bị lật: trái ảnh = `rear_right`, phải ảnh = `rear_left`.
- **Mắt và tay gọi theo người.** Trên ảnh D## xoay 90°, trục hai mắt chạy theo y, nên mắt **trái** của người lái có y lớn hơn; không xoay ảnh, chỉ đọc ảnh theo trục đúng.
- `unknown` chỉ được ghi **sau khi** đã phóng to 1:1 và tăng độ sáng hiển thị để kiểm tra — không dùng như giá trị mặc định cho nhanh.

Ảnh `_rgb` của mỗi cảnh được gán trước, shape chép sang ảnh `_ir` rồi soát lại, vì hai bản dựng lệch nhau vài pixel và khác nhau về lượng chi tiết nhìn thấy được.

## 4. Thống kê nhãn (từ `export_final.zip`)

| Nhãn | Số lượng | Ghi chú |
|---|---|---|
| `person` | 22 | 16 box trong ảnh OMS, 6 người lái |
| `face` | 10 | 6 mặt người lái + 4 mặt người lớn trong ảnh OMS |
| `face13` | 6 skeleton | 78 điểm, **31 điểm mang cờ `occluded`**, 5 điểm `outside` — chói kính, tay, chai nước |
| `hand` | 12 | 2 tay mỗi khung người lái |
| `phone` | 1 | D05, `state = held` |
| `child_seat` | 14 | OMS |
| `infant_seat` | 10 | OMS |
| `object` | 4 | cup (S03) / hộp (S04) — túi ở S06 **chưa có**, xem §6 |
| tag `frame_meta` | 22 | đúng một tag mỗi ảnh |

Cấu hình từng cảnh OMS, theo thứ tự **trái → phải trong ảnh**:

| Cảnh | Nội dung (thứ tự trong ảnh) |
|---|---|
| S01 | `rear_right` ghế em bé · `rear_left` ghế trẻ em trống |
| S02 | `rear_right` ghế em bé + người lớn · `rear_left` người lớn (+ box `face`) |
| S03 | `rear_right` cốc · `rear_left` người lớn + ghế trẻ em |
| S04 | `rear_right` hộp · `rear_middle` người lớn (+ box `face`) |
| S05 | `rear_right` người lớn + ghế trẻ em · `rear_left` người lớn + ghế trẻ em |
| S06 | `rear_right` ghế trẻ em · `rear_left` ghế em bé (thiếu box túi `rear_middle` — xem §6) |
| S07 | `rear_middle` người lớn + ghế em bé (`facing = front`) |
| S08 | `rear_right` ghế em bé + người lớn · `rear_middle` ghế trẻ em · `rear_left` ghế trẻ em |

## 5. Kết quả tự kiểm (E2)

| File | Phạm vi | Kết quả |
|---|---|---|
| `check_cpA.txt` | S01–S02, expect 4 | 0 lỗi · 0 cảnh báo |
| `check_cpB.txt` | D01–D02, expect 2 | 0 lỗi · 0 cảnh báo |
| `check_final.txt` | toàn bộ 22 ảnh, expect 22 | **0 lỗi · 0 cảnh báo** |
| `check_extra.txt` | S09–S10, expect 4 | 0 lỗi · 0 cảnh báo |

`python -m zipfile -l export_final.zip` chỉ liệt kê `annotations.xml` (49 KB) — không kèm ảnh, đúng yêu cầu export.

## 6. Soát ngoài script (những lỗi script không bắt được)

Script không có đáp án, nên các lỗi "im lặng" được kiểm trực tiếp trên XML theo toạ độ và thuộc tính:

| Nội dung kiểm | Kết quả |
|---|---|
| `modality` khớp tên file | 22/22 |
| `light = unknown` trên mọi ảnh dựng | 16/16 |
| Chuỗi đối tượng của `_rgb` và `_ir` cùng cảnh trùng nhau | 8/8 cảnh khớp |
| Chiều lật ghế (box bên trái ảnh = `rear_right`) | nhất quán trong cả 16 khung OMS |
| `seatbelt` trên SVIRO chỉ có `not_worn` | 16/16, không có `worn_*` |
| Không còn sót `seat = driver` trên box OMS | 0 trường hợp |
| Không có `occupied = yes` trên `child_seat` | 0 trường hợp |
| `face13` đủ 13 điểm mỗi skeleton | 6/6 |
| Mắt trái nằm dưới mắt phải (y lớn hơn) trên ảnh xoay | 6/6 |
| `face13` chỉ trên D## và chỉ khi mặt ≥ 60 px | đạt (mặt người lái nhỏ nhất 363 px) |

## 7. Vấn đề phát hiện được và hệ quả

| # | Phát hiện | Guideline | Trạng thái |
|---|---|---|---|
| 1 | Sáu người lớn bên OMS thiếu box `face` (S02 `rear_right`, S03 `rear_left`, S05 cả hai bên, S07, S08) — 12 box tính cả rgb và ir | §4.2 "người lớn → `person` + `face`" (trừ khi mặt quay đi, §4.5) | đã ghi nhận, không sửa trong bản nộp |
| 2 | Một `person` và một ghế trẻ em/ghế em bé trùng cùng một vị trí ghế ở 5 khung (S02, S05, S07, S08) | §4.8 một box mỗi ghế | đã nêu thành câu hỏi leo thang Q2 |
| 3 | Cốc ở S03 nằm đúng ranh giới `rear_right` / `rear_middle` | §4.8 `seat` phải chính xác | cần soát lại một khung |
| 4 | Mắt ghi `unknown` ở 4/6 khung người lái (D01, D03, D04, D05-trái) dù `glasses = clear` | §4.5 chỉ `unknown` khi thật sự không thấy mí | đã ghi nhận; D04 đã đổi sang `unknown` cho khớp cờ che |
| 5 | D02 `gaze_zone = other` | §4.5 phải ghi lý do cho `other` | lý do đã ghi trong `ir_log.csv` |

Bốn mục đầu đều **không** bị `cabin_check` phát hiện: script không có đáp án về vị trí ghế và không có luật "người lớn thiếu box `face`". Đó chính là lý do phải soát thêm.

## 8. Ảnh hồng ngoại làm mất gì (tóm tắt E4)

Bảy quan sát trong `ir_log.csv`, tách theo nguồn:

- **IR_sim (S##):** không vẽ dây an toàn; ruột ghế em bé/ghế trẻ em không có vân nên không xác nhận được trống; túi và lưng ghế phía sau nhoè thành một khối; ghế em bé + hai ghế trẻ em dính thành một mảng tối. Hệ quả: `seatbelt` và `occupied` chỉ có thể `unknown` hoặc suy theo luật, không đọc được từ ảnh.
- **real_IR (D##):** vòng LED phản xạ xoá mất một mép mí (D01 → `eye_right = unknown`); vùng đùi chỉ được chiếu bởi ánh đồng hồ (D03 → không vẽ `phone`); nhưng D06 cho thấy mí khép vẫn đọc được khi đường mí còn tách khỏi vùng mắt — nên `closed` ở D06 là hợp lý, còn `unknown` ở D01 **không** phải chính sách chung.

Khác biệt này quan trọng cho huấn luyện: model chỉ học trên SVIRO sẽ không bao giờ thấy dây an toàn đã thắt, nên không học được lớp `worn_correct` mà chính nó phải dự đoán. Đây là lỗ hổng đã nêu trong danh sách lớp.

## 9. Danh sách lớp v0 (E5)

9 mục DMS (person/seat, seatbelt, face, eye_left–eye_right, glasses, gaze_zone, face13, hand, phone) cộng tag `frame_meta`; 6 mục OMS cộng tag. Mỗi dòng nêu rõ model dùng tín hiệu đó để làm gì và tín hiệu còn sống được trên IR hay không. Hai câu hỏi leo thang đều kèm khung ví dụ cụ thể (D01, S03_ir / S02_rgb, S07_rgb, S08_rgb).

## 10. Review chéo (E3) — đã chạy

`cabin_check diff export_final.zip <peer export>` giữa export của tôi (CVAT job 41, `minhquan`) và export của bạn cùng mã G01 nhận qua CVAT (job 66): **22 ảnh chung, 40 điểm khác**. Cả 40 dòng trong `disagreement_log.csv` đã điền `verdict` / `guideline_rule` / `action`.

Ba nhóm chính:

- **Thiếu box (12 điểm):** `S06` `object` — peer có túi `rear_middle`, tôi không có → **peer đúng**; tôi ghi nhận là lỗi thiếu, không kịp sửa.
- **Cờ che trên `face13` (20 điểm):** peer bật `occluded` cho cả 8 điểm mắt ở D02/D05 và đặt `eye_* = unknown`; tôi để `0` + `open` khi thấy mí. Theo Q1 (`occluded` = biết vị trí nhưng bị che, `outside` = không xác định được vị trí), cách của tôi đúng hơn; ngưỡng "che một phần" là chỗ hai bên lệch nhau.
- **Giá trị thuộc tính (8 điểm):** `gaze_zone` (tôi `other`, peer `road`/`lap`), `glasses` ở D04 (`none` vs `clear`), `infant_seat.facing` ở S08 (`rear` vs `front`), `seatbelt` ở D01 (`worn_correct` vs `not_worn`). `gaze_zone` và `seatbelt` tôi giữ nguyên kèm lý do; `glasses` và `facing` đưa vào danh sách hỏi coach vì guideline không đủ để chốt.

Không sửa gì theo peer nếu guideline không nói rõ — đó là lý do bản nộp chỉ có `export_final.zip`, không có `export_final_v2.zip`; các điểm còn lệch được nêu thành câu hỏi cho coach chứ không đoán.

## 11. Hạn chế

- Không có đáp án cho vị trí ghế và lớp của ghế; tính đúng của `seat` được lập luận từ guideline và từ việc rgb/ir khớp nhau, không phải được chứng minh.
- Hai người gán giống nhau chỉ chứng minh **nhất quán**, không chứng minh **đúng** (RUBRIC) — hạn chế này áp dụng cả cho phép so rgb/ir của chính tôi.
- Phạm vi chỉ 8 cảnh dựng, nên các cấu hình hiếm (trẻ ngồi trong ghế trẻ em, dây an toàn thắt ở hàng sau) không có trong nhãn.

## 12. Nguồn gốc và tuân thủ

Không đưa ảnh cabin hay ảnh người lái cho bất kỳ công cụ AI nào, và không có nhãn nào do máy tạo: phần soát ở §6 chạy trên XML export, theo toạ độ và giá trị thuộc tính, không đọc pixel. Việc dùng AI được khai trong `prompts.md`, gồm cả lần lỡ gửi một ảnh chụp màn hình trước khi áp đúng luật. Ảnh chỉ nằm trên máy và trong CVAT local, đã bị `.gitignore` chặn; repo chỉ chứa export XML/zip, log và markdown (RULES §2 và §5).

## 13. Tài liệu tham chiếu

Guideline Cabin v1.0 (`GUIDE.md` §3–4), `CHECKPOINTS.md`, `COMMON_ERRORS.md`, `SUBMISSION.md`, `RUBRIC.md`, `RULES.md`. Dữ liệu: SVIRO (DFKI/IEE, CC BY-NC-SA 4.0, IR mô phỏng); Drive&Act (Martin et al., ICCV 2019, chỉ dùng cho học tập và nghiên cứu).
