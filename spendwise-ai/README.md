# Học cùng dự án SpendWise AI

Repo này chứa bài học, bài tập và nhật ký tự học. [Krev1/spendwise-ai](https://github.com/Krev1/spendwise-ai) chứa code, test, đặc tả, thiết kế và kế hoạch dự án.

## Bắt đầu từ đâu?

1. Đọc [bài 00 — GitHub và môi trường](lessons/00_git_and_environment.md).
2. Làm [bài 01 — Python và thu–chi](lessons/01_python_and_money.md).
3. Ghi kết quả và câu trả lời của bạn vào [nhật ký học](progress.md).

Xem [lộ trình học](learning_path.md), [cách Mentor hướng dẫn](mentor_guide.md), [hướng dẫn bảo vệ](defense_guide.md) và [prompt Mentor](MENTOR_PROMPT.md). Bài 02 đã được soạn để đọc CSV và code domain/services; bài 03–07 sẽ viết khi triển khai phần tương ứng.

## Dùng hai repo trên Windows

Trong một thư mục chứa các dự án của bạn, clone mỗi repo một lần:

```powershell
git clone https://github.com/Krev1/spendwise-ai.git
git clone https://github.com/Krev1/Learn.git
```

Nếu đã có repo local, dùng thư mục đó. Giữ `Learn` và `spendwise-ai` cạnh nhau để lệnh sao chép bài tập hoạt động. Ví dụ:

```text
thu-muc-cua-ban/
  spendwise-ai/             # Code, test, .venv và tài liệu SDD
  Learn/spendwise-ai/       # Bài học, bài tập và nhật ký
```

Làm theo [README dự án](https://github.com/Krev1/spendwise-ai#readme) để tạo `.venv` nếu chưa có. Đọc bài học trong Learn nhưng chạy các lệnh Python từ thư mục `spendwise-ai`, nơi chứa `.venv` và code. CSV luyện tập được sao chép sang `spendwise-ai/local/`; không sửa CSV mẫu đang dùng cho test.

Code tham chiếu luôn nằm trong repo dự án; không duy trì một bản code riêng trong Learn. Mỗi bài mới cần ghi TASK/REQ liên quan và commit code đã dùng để người học có thể đối chiếu.

## Phạm vi hiện tại

Đã chuẩn bị bài 00, 01 và [02 — CSV và validation](lessons/02_csv_and_labels.md), bài tập hư cấu và lộ trình. Chưa xác nhận người học hoàn thành bài; chưa có mô hình tự huấn luyện hoặc kết quả AI. Các mục tiêu chất lượng trong tài liệu dự án là mục tiêu đề xuất.

Hai repo công khai. Chỉ ghi ví dụ hư cấu, kết quả và nhật ký đã loại thông tin cá nhân; không đưa giao dịch thật, khóa truy cập hoặc database vào bài học.
