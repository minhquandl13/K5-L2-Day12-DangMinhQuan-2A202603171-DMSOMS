# Self-review — Đặng Minh Quân · 2A202603171 · G01

## 1. Four frames read as a stranger

| Frame | Object | Error type | Note | Fixed? |
|---|---|---|---|---|
| S02_rgb | `person` at `rear_right` | `missing` | Người lớn bên trái ảnh không có box `face`; chỉ người `rear_left` có. §4.2 yêu cầu box `face` cho mọi người lớn. | Không — đã ghi nhận, không kịp sửa |
| S05_rgb | `person` + `child_seat` ở cả hai bên | `extra` | Hai box chiếm cùng một vị trí ghế. §4.8 cho phép một box mỗi ghế, hai box chỉ khi có vật nằm trong ghế trẻ em. | Không — đã ghi nhận |
| S07_rgb | `person` + `infant_seat` ở `rear_middle` | `wrong_class` | Box người nằm trong box ghế em bé. Guideline không nói box nào giữ lại khi trùng; đã nêu thành câu hỏi leo thang Q2 trong `class_list_v0.md`. | Không — chờ coach |
| D01 | `face.eye_right` | `guess` | Dùng `unknown` dù kính trong và một phần mép mí vẫn thấy dưới vệt phản xạ. | Không — bản nộp vẫn là `unknown` |

## 2. The error I repeated most today

**Error:** reaching for `unknown` on the eyes as soon as a reflection appeared.

**Why I did it:** a wrong `open`/`closed` feels more damaging than a lazy `unknown`, so I took the safe-looking option without actually looking. That converts a hard case into a missing label, which is worse for a drowsiness model than an honest attempt.

**How to avoid it next time:** before writing any eye value, zoom to 1:1 and raise brightness/contrast in CVAT's Image settings, then answer one question — "is a lid line present?" If yes, the value is `open` or `closed`; only when the line is genuinely absent does it become `unknown`.

## 3. Ghi chú về `disagreement_log`

Peer diff đã chạy: `export_final.zip` (CVAT job 41, tài khoản `minhquan`) so với export của bạn cùng mã G01 nhận qua CVAT (job 66). Kết quả **40 điểm khác trên 22 ảnh chung**, cả 40 dòng đã điền `verdict` / `guideline_rule` / `action`.

Ba nhóm đáng chú ý:

- **`S06_ir` / `S06_rgb` · `object`:** peer có túi ở `rear_middle`, tôi không có → **peer đúng**, tôi thiếu box. Không kịp bổ sung trước hạn, đã ghi nhận đây là lỗi thiếu của mình.
- **`S08_ir` / `S08_rgb` · `infant_seat.facing`:** tôi `rear`, peer `front`. Ảnh OMS chụp từ hàng ghế trước nên ghế quay về phía camera là rear-facing; tôi giữ `rear` và đưa vào danh sách hỏi coach.
- **`D02`, `D03`, `D05` · `face13`:** peer bật `occluded` cho cả mắt và đặt `eye_* = unknown`. Tôi để điểm không bị che là `0` và `open` khi thấy mí, theo Q1 (occluded = biết vị trí nhưng bị che; outside = không xác định được vị trí; `unknown` không phải mặc định).

Điều tôi học được từ diff: cùng một guideline vẫn cho ra hai kết quả khác nhau ở chỗ **ngưỡng "bị che một phần"** và ở chỗ **suy `gaze_zone` từ ngữ cảnh thay vì từ mí mắt**. Cả hai không phải lỗi script, và cũng không phải lỗi của ai — đó là ca cần coach chốt bằng một con số.

Những gì tôi đã tự soát và **vẫn còn mở** (script không bắt được): thiếu box `face` cho người lớn ở S02/S03/S05/S07/S08, túi ở S06, và việc `person` trùng ghế với `child_seat`/`infant_seat` ở S05/S07.
