# Khai báo dùng AI — Đặng Minh Quân · 2A202603171

Nguyên tắc chi phối mọi dòng dưới đây: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào, và không có nhãn nào do AI tạo ra.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Trợ lý AI Doubao (bản desktop) | Bài nộp phải có những file nào và đặt ở đâu, đọc từ `SUBMISSION.md` / `RUBRIC.md` | Chép các mẫu trong `templates/` vào thư mục gốc và đổi đúng tên file được yêu cầu | Tôi tự đọc lại `SUBMISSION.md`; danh sách tên file khớp với cây thư mục trong đó |
| 2 | Trợ lý AI Doubao | Các bước tạo task trong CVAT (project → task → 22 file, nhãn thừa hưởng từ project) | Dùng như danh sách kiểm cho Task 0; task tạo ra đúng 22 khung và 1 job | Trang task của CVAT ghi Frames: 22; `cabin_check` đọc được 22 mục ảnh |
| 3 | Trợ lý AI Doubao | Cách gọi tên ghế và cách xác định trái/phải của mắt, tay (quy ước theo xe so với theo người, ảnh D## bị xoay) | Dùng để vẽ một trang đối chiếu của riêng mình; mọi quyết định vẫn lấy trên chính khung ảnh | Đọc chéo `GUIDE §3`, `§4.4`, `COMMON_ERRORS`; rồi kiểm bằng số trên export (`left_eye_*` có y > `right_eye_*` ở cả 6 khung D) |
| 4 | Trợ lý AI Doubao | Một `export_final.zip` đã làm có đúng guideline không (công cụ chỉ đọc XML, trả về toạ độ box và giá trị thuộc tính, không liên quan tới ảnh) | Dùng kết quả soát để tìm các lỗi script không thấy: 4 người lớn thiếu box `face`, ghế mang đồng thời `person` và ghế trẻ em, mắt ghi `unknown` ở 5/6 khung tài xế | Tôi tự mở lại từng khung bị đánh dấu trong CVAT trước khi chấp nhận |
| 5 | Trợ lý AI Doubao | Góp ý chữ nghĩa cho `class_list_v0.md` (cấu trúc, cột còn thiếu, một câu hỏi leo thang đã được guideline trả lời sẵn) | Tôi tự viết lại danh sách theo đúng cột của mẫu; thay một câu hỏi mà `§4.4` quy tắc 3 đã trả lời | So với `templates/class_list_v0.md` và `GUIDE §4.2/§4.8` |
| 6 | Trợ lý AI Doubao | Có một lần tôi gửi ảnh chụp màn hình một khung D## khi đang hỏi trục trái/phải (vi phạm RULES §3, ghi ở đây để minh bạch) | Không lấy nhãn nào từ câu trả lời đó; sau ấy chỉ đọc nội dung guideline | Quy tắc này được nhắc lại trong `GUIDE §4.4` và `COMMON_ERRORS`, tôi theo đúng phần chữ |

Không dòng nào ở trên tạo ra một nhãn. Mọi box, thuộc tính và điểm `face13` trong `export_final.zip` đều do tôi vẽ trong CVAT.
