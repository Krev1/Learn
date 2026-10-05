# Học cùng dự án SpendWise AI

> **Hướng mới06/10/2026:** web nhiều tài khoản/ngân sách/Deep Learning. Đọc [guide05](guides/05_public_web_and_deep_learning.md) và [prompt Mentor v0.2](MENTOR_PUBLIC_WEB_PROMPT.md). Các bài cũ giữ SHA/mức hiểu riêng; spec mới chưa chứng minh code web/neural.


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

Làm theo [README dự án](https://github.com/Krev1/spendwise-ai#readme) để tạo `.venv` nếu chưa có. Theo D13, đọc code dự án nhưng thực hành trong checkout riêng `Learn/spendwise-ai/practice/project/` và `.venv` riêng theo [study_workflow.md](study_workflow.md). Các lệnh bài học ghi root spendwise-ai chạy ở root checkout thực hành, không sửa checkout triển khai. CSV luyện tập nằm trong local/ của bản practice; không sửa CSV gốc đang dùng cho test.

Code tham chiếu luôn nằm trong repo dự án; không duy trì một bản code riêng trong Learn. Mỗi bài mới cần ghi TASK/REQ liên quan và commit code đã dùng để người học có thể đối chiếu.

## Phạm vi hiện tại

Đã chuẩn bị bài 00, 01 và [02 — CSV và validation](lessons/02_csv_and_labels.md), bài tập hư cấu và lộ trình. Chưa xác nhận người học hoàn thành bài; chưa có mô hình tự huấn luyện hoặc kết quả AI. Các mục tiêu chất lượng trong tài liệu dự án là mục tiêu đề xuất.

Hai repo công khai. Chỉ ghi ví dụ hư cấu, kết quả và nhật ký đã loại thông tin cá nhân; không đưa giao dịch thật, khóa truy cập hoặc database vào bài học.

## Thu thập và xây dataset

- [Phương pháp thu thập/xây dữ liệu](guides/01_dataset_collection.md): nguồn đã khảo sát, nguồn được thu, chuyển ngữ, provenance, thu thật tự nguyện, audit và cách chia nhóm.
- [Guideline tám nhãn](guides/02_labeling_manual.md): ranh giới lớp, trường hợp mơ hồ và gán độc lập.
- [Bài 02b — đọc và audit dataset](lessons/02b_dataset_construction.md): lệnh thực hành, giải thích Python, bài sửa lỗi và câu hỏi bảo vệ.
- [Template trống](templates/README.md) và [20 câu luyện gán nhãn](exercises/03_annotation_practice.csv).

Dataset/code ở [repo dự án](https://github.com/Krev1/spendwise-ai/tree/9ac8ed0c4702b30ce4a26b980595526c4031e03a/data). Hiện có 356 mô tả hư cấu thuộc 8 nhãn, 0 mẫu thật; nhãn AI dự thảo, chưa có người duyệt. Có thể học kỹ thuật ngay; chưa có bằng chứng chất lượng phân loại trên chi tiêu thực tế. Người học chưa hoàn thành bài nếu chưa tự chạy/giải thích.

## Hai chat: dự án độc lập, học bám theo commit

Chat dự án dùng [prompt kỹ thuật](https://github.com/Krev1/spendwise-ai/blob/main/prompts/START_HERE.md) và chỉ sửa spendwise-ai. Chat học dùng [MENTOR_PROMPT.md](MENTOR_PROMPT.md) và chỉ sửa Learn/spendwise-ai; bắt đầu bằng bài 00/01 và không chặn tiến độ code.

Đọc [workflow học](study_workflow.md) và [mapping bài ↔ TASK/REQ/commit](learning_map.json). Mentor kiểm tra bàn giao mới ở đầu buổi, chọn SHA cố định và ghi mức hiểu riêng. Chưa tạo chat, môi trường practice hoặc automation trong lần cập nhật này; người dùng có thể tạo hai chat và dán hai prompt riêng.
