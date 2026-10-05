# Hướng dẫn tổ chức học theo task

Các TASK/REQ và kế hoạch kỹ thuật nằm trong [repo dự án](https://github.com/Krev1/spendwise-ai/blob/main/docs/03_implementation_plan.md).

## Bài tập cho P1–P6

### P1

**Bài tập và tự giải thích:** viết một hàm nhận tổng thu/tổng chi và trả chênh lệch; giải thích `str` so với `int`, return so với print, exception và lý do nhập sai phải dừng ghi dữ liệu.

### P2

**Bài tập và tự giải thích:** gán nhãn riêng một nhóm câu, đối chiếu guideline và ghi bất đồng; chứng minh CSV giao dịch khác dataset train; sửa một CSV có ngày không tồn tại, ID trùng và mô tả chứa dấu phẩy.

### P3

**Bài tập và tự giải thích:** tính precision/recall/F1 của một lớp từ TP/FP/FN; chỉ ra feature nào được học từ train; đọc hai cặp nhãn nhầm nhiều; giải thích vì sao score 0,85 chưa có nghĩa dự đoán đúng 85%.

### P4

**Bài tập và tự giải thích:** mô phỏng lỗi ở insert thứ hai; chứng minh sau rollback không có insert thứ nhất; đổi tên cùng file CSV rồi nhập lại và đếm dòng; giải thích vì sao hash nguồn giữ nguyên khi đổi nhãn đã xác nhận.

### P5

**Bài tập và tự giải thích:** demo một khoản chi bị gợi ý sai nhưng được sửa trước lưu; giải thích từng bước từ browser đến service → model → xác nhận → SQLite → dashboard; đổi file model thành không tồn tại và vẫn nhập một giao dịch.

### P6

**Bài tập và tự giải thích:** viết một trang phân tích lỗi thật, một trang đóng góp/giới hạn và demo khi AI không hoạt động; trả lời câu hỏi “Mô hình thua baseline ở một lớp thì kết luận của bạn thay đổi thế nào?”.

## 7. Mẫu nhật ký học và thực nghiệm

```text
Ngày:
Task / REQ:
Tôi muốn kiểm chứng điều gì:
Tôi đã chạy lệnh hoặc thao tác nào:
Kết quả thực tế và nơi lưu bằng chứng:
Tôi đã hiểu được:
Điểm chưa hiểu / lỗi gặp phải:
Quyết định thay đổi và lý do:
Câu hỏi bảo vệ tôi có thể tự trả lời:
Bước tiếp theo:
```

Đối với thực nghiệm AI, bổ sung phiên bản dữ liệu, split, seed, cấu hình, baseline, model, thời gian chạy và metrics. Không thay đổi nhiều yếu tố cùng lúc nếu muốn biết cải tiến đến từ đâu.

## 8. Nhịp học đề xuất theo mốc, chưa gắn thời hạn

| Mốc | Người học cần làm được | Vai trò hỗ trợ chính |
|---|---|---|
| M0 — Môi trường và Python cơ bản | Chạy Python, đọc CSV, viết hàm nhỏ và hiểu số nguyên VND | Mentor + Developer |
| M1 — Dữ liệu và baseline | Gán nhãn theo hướng dẫn, đọc phân bố lớp và dựng baseline đơn giản | Data Engineer + ML Researcher |
| M2 — Mô hình tự huấn luyện | Giải thích TF-IDF, train/validation/test và đọc confusion matrix | ML Researcher + Mentor |
| M3 — Ứng dụng dữ liệu | Nhập/sửa/xóa, lưu SQLite và kiểm tra tổng tiền | Developer + QA |
| M4 — Tích hợp AI và CSV | Từ mô tả nhận gợi ý, xác nhận, import nguyên tử và không thêm lại cùng batch | Developer + ML Researcher + QA |
| M5 — Thử nghiệm và báo cáo | Phân tích sai, kiểm chứng bằng dữ liệu được phép, đối chiếu rubric và luyện demo | QA + Mentor + Project Owner |

Chỉ ước lượng lịch cụ thể sau khi biết số giờ mỗi tuần và cấu hình máy. Một mốc có thể quay lại bước dữ liệu hoặc thiết kế khi xuất hiện bằng chứng mới; phải ghi lý do và cập nhật tài liệu liên quan.

## 9. Các câu hỏi luyện bảo vệ xuyên suốt

- Người dùng đang mất công ở bước nào, và tính năng của bạn giảm công việc đó ra sao?
- Vì sao chọn tám danh mục; quy tắc gán nhãn câu mơ hồ là gì?
- Dữ liệu nào tự tạo, dữ liệu nào thực và bạn có quyền sử dụng chúng thế nào?
- Vì sao dùng macro-F1 cùng các chỉ số theo lớp thay vì chỉ báo accuracy?
- Bạn tránh câu gần trùng xuất hiện ở cả train và test bằng cách nào?
- Mô hình tốt hơn baseline ở đâu, và có lớp nào kém hơn không?
- Điểm mô hình `0.85` có nghĩa gì, và vì sao không gọi là xác suất đúng 85%?
- Một người sửa nhãn có lập tức làm mô hình học lại không? Quy trình thực tế là gì?
- Vì sao tổng số tiền do code tính, và bạn kiểm chứng import nguyên tử thế nào?
- Nếu dữ liệu chỉ là các câu bạn tự viết, kết luận nào chưa thể đưa ra?

Trả lời các câu hỏi này bằng code, dữ liệu và kết quả đã lưu. Phản biện AI là bài tập luyện; yêu cầu chính thức vẫn cần đối chiếu với giảng viên và rubric trường.
