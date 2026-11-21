# Danh sách lớp v0 — Đặng Minh Quân · 2A202603171 · G01

Viết tiếp từ slide 54 bài giảng và Cabin Guideline v1.0 (§4.1–4.8). Mỗi dòng trả lời một câu: **model cần nhận ra cái này để làm gì?**

## 1. DMS — Driver Monitoring System

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` (`seat`) | driver | Xác nhận có tài xế trong ghế; là mốc để suy dây an toàn và tay. Không có tài xế thì không có gì để giám sát. | Có |
| 2 | `person` (`seatbelt`) | worn_correct / worn_incorrect / not_worn / unknown | Cảnh báo không thắt dây; `worn_incorrect` là ca nguy hiểm (dây vòng sau lưng). | Một phần — dây tối trên áo tối thành `unknown` |
| 3 | `face` (box) | — | Định vị khuôn mặt; mọi tín hiệu bên dưới (mắt, điểm mốc, hướng nhìn) đều cắt từ box này. | Có |
| 4 | `face.eye_left` / `face.eye_right` | open / closed / unknown | Phát hiện buồn ngủ và ngủ gật: mắt nhắm lâu (kiểu PERCLOS) và tần suất chớp. Đặt riêng từng mắt vì một mắt có thể bị che. | Có, trừ khi chói LED hoặc kính tối |
| 5 | `face.glasses` | none / clear / sun | Quyết định tín hiệu mắt có đáng tin hay không. `sun` nghĩa là mắt coi như không quan sát được, hệ thống phải chuyển sang ước lượng theo hướng đầu. | Một phần — kính râm có thể trông trong suốt trên NIR |
| 6 | `face.gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện mất tập trung: mắt rời khỏi đường khi đang lái. `lap` + `phone` là mẫu "vừa lái vừa dùng điện thoại". | Một phần — cần hướng đầu, chi tiết NIR thấp |
| 7 | `face13` | 13 điểm mốc, mỗi điểm có `occluded` / `outside` | Tư thế đầu (pitch/yaw/roll), hình học chớp mắt và ngáp, hướng nhìn — nhờ đó hướng nhìn đo được thay vì đoán. | Có, với các điểm bị che được đánh cờ |
| 8 | `hand` | `whose` driver/passenger · `side` left/right · `on_wheel` yes/no/unknown | Kiểm tra tay có trên vô lăng và mức sẵn sàng tiếp quản; đồng thời là mốc cho `phone` (`held`). | Có |
| 9 | `phone` | held / mounted / lap | Phát hiện dùng điện thoại — lớp vi phạm an toàn và pháp lý, không phải lớp đồ vật. | Có theo hình dạng; màn hình tối không phát sáng trên NIR |
| 10 | tag `frame_meta` | modality · light · camera_blocked | Cổng kiểm tra khung: model phải biết khung này có dùng được không (ir hay rgb, đêm, ống kính bị che) trước khi tin bất kỳ dự đoán nào. | Không áp dụng (tag, không phải đối tượng) |

## 2. OMS — Occupant Monitoring System

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` (`seat`) | rear_left / rear_middle / rear_right / front_passenger | Trạng thái có người theo từng ghế — nền tảng của toàn bộ cabin sau. | Có |
| 2 | `person` (`seatbelt`) | not_worn / unknown *(SVIRO không có ca thắt dây)* | Nhắc thắt dây hàng sau. | Thường không — dây tối, tương phản thấp |
| 3 | `face` (chỉ box) | — | Xác nhận chỗ ngồi là người lớn chứ không phải hành lý, và ghép mặt với ghế để nhắc dây an toàn. | Có |
| 4 | `child_seat` | `seat` · `occupied` no / unknown | Có ghế trẻ em và ghế đó trống hay không; phục vụ nhắc "trẻ bị bỏ trong xe" và tắt túi khí. | Có (hình dạng) |
| 5 | `infant_seat` | `seat` · `facing` rear / front | Phát hiện ghế em bé quay về phía sau — ca quên trẻ nguy hiểm nhất — và tắt túi khí. | Có (hình dạng) |
| 6 | `object` | `seat` · `type` bag / box / cup / other | Tách đồ đạc khỏi người ngồi để hành lý không bị đếm thành trẻ; đồng thời báo vật không được cố định. | Hình dạng được, `type` hay suy thành `other` |
| 7 | tag `frame_meta` | modality · light · camera_blocked | Cùng cổng kiểm tra khung như DMS: camera sau bị che hoặc quá tối thì không được báo "cabin trống" sai. | Không áp dụng (tag) |

Ghế trống **không vẽ box**; đếm trong ghi chú khung (§4.8).

## 3. Quy tắc đã áp khi viết danh sách này

1. Tên ghế theo **xe**, không theo ảnh: trên ảnh OMS hàng sau, bên trái ảnh = `rear_right`.
2. Trái/phải của mắt và tay theo **người**; trên ảnh D## xoay, mắt trái của tài xế có y lớn hơn.
3. `unknown` khi bằng chứng thị giác không đủ — không suy từ mắt kia, từ ảnh cùng cặp, hay từ tên file.
4. Box bám sát phần nhìn thấy, sát mép.
5. Vật nằm trong ghế trẻ em lấy **hai box** (`child_seat` + `object`) trên cùng `seat`.
6. `face13` chỉ trên ảnh D## và chỉ khi mặt rộng ít nhất 60 px; mọi điểm bị che mang `occluded` hoặc `outside`.
7. `seatbelt = worn_correct` không thể xuất hiện trên SVIRO; chỉ `not_worn` hoặc `unknown`.
8. `_ir` của SVIRO là IR **mô phỏng**: không có chói LED, không có đồng tử sáng, không có suy giảm theo khoảng cách, nên không được ghi các hiện tượng đó từ khung S##.

## 4. Hai câu hỏi leo thang

**Câu 1. Khi một vật hoặc một bộ phận trên mặt chỉ nhìn thấy một phần, mà phần bị che lại chính là phần quyết định lớp, thì người gán nhãn nên đặt biên ở đâu?**
- Khung ví dụ: `G01 D01` — một điểm góc của `face13` nằm sau vệt phản xạ LED trên tròng kính, ước lượng được vị trí nhưng không nhìn thấy; `G01 S03_ir` — cái cốc nằm trên ranh giới `rear_right` / `rear_middle`, mép dưới bị lưng ghế che.
- Cách tôi tạm gán: thuộc tính thì `unknown`; điểm `face13` mang `occluded` khi ước lượng được vị trí với độ tin cậy, và `outside` khi không xác định được. Không điểm nào được đặt "cho đủ 13".

**Câu 2. Khi một người ngồi cùng vị trí ghế với ghế em bé hoặc ghế trẻ em đã lắp, box nào được giữ?**
- Khung ví dụ: `G01 S02_rgb`, `G01 S07_rgb`, `G01 S08_rgb` — `person` và `infant_seat` / `child_seat` trùng nhau trên cùng một ghế.
- Cách tôi tạm gán: `§4.8` nói một box mỗi ghế, nhưng `§4.2` liệt kê cả hai lớp mà không nói lớp nào sống khi chúng trùng nhau. Tôi giữ box `person`, đặt ghế mang `occupied = unknown` ở chỗ không nhìn được bên trong, và đưa câu này ra coach. Cần chốt trong v1.1.

## 5. Tham chiếu

Cabin Guideline v1.0, §4.1–4.8 (nhãn, thuộc tính, sáu quy tắc chung, mặt / mắt / hướng nhìn, tay / điện thoại / dây an toàn, ghế trẻ em và đồ vật). Ngày 21 (DMS) và Ngày 22 (OMS) sẽ mở rộng danh sách này thành v1.x.
