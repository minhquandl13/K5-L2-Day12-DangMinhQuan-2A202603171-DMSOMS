# TEAM — Day 12 (individual mode)

Image bundle: **G01** · Format: **individual** (not a group)

| Full name | Student ID | All 22 frames labelled? | Cross-review with (peer diff) | Disagreement rows handled |
|---|---|---|---|---|
| Đặng Minh Quân | 2A202603171 | yes — 22/22 (`--expect 22`, 0 errors) | peer export nhận qua CVAT (job 66, `HungNguyen` / `hungnn91296@gmail.com`) | 40/40 dòng đã điền verdict + luật + hành động |
| | | | | |

Cases where my peer and I still disagree and asked the coach (frame name + object):

- `S06_ir` / `S06_rgb` · `object` · peer có túi ở `rear_middle`, tôi không có → tôi thiếu, chưa kịp bổ sung
- `S08_ir` / `S08_rgb` · `infant_seat.facing` · tôi `rear`, peer `front` → hướng ghế em bé trên ảnh OMS, cần coach chốt
- `D04` · `face.glasses` · tôi `none`, peer `clear` → clear/none trên NIR, cần coach chốt
- `D03` · `face13.right_eye_*` · tôi `outside`, peer `occluded` → hai cờ loại trừ nhau, tôi theo Q1

## Individual section

### DangMinhQuan-2A202603171
- What I did beyond my own 22 frames: labelled the full set alone (no split), ran all three checkpoints myself, audited the export beyond the script for the two silent errors (`seat` mirroring and rgb/ir drift), wrote `ir_log.csv` from my own frames, and wrote the class list v0 with the two escalation questions. Optional extra: labelled S09–S10 in a separate task and exported `export_extra.zip`.
- What I learned today: seat names belong to the car while eye and hand sides belong to the person — two opposite conventions inside one guideline; and `unknown` is a judgement about visibility, not a safe default.
