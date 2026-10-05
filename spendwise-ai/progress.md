# Nhật ký học SpendWise AI

Ngày chuẩn bị: 05/10/2026. Tiến độ kỹ thuật nằm ở [repo dự án](https://github.com/Krev1/spendwise-ai/blob/main/progress.md).

| Bài | Trạng thái bài học | Trạng thái người học |
|---|---|---|
| [00 — GitHub và môi trường](lessons/00_git_and_environment.md) | Đã soạn | Chưa có câu trả lời tự giải thích |
| [01 — Python và thu–chi](lessons/01_python_and_money.md) | Đã soạn, có CSV luyện tập | Chưa có bài tự làm/kết quả do người học gửi |
| [02 — CSV và validation](lessons/02_csv_and_labels.md) | Đã soạn, có bộ tình huống thực hành; code đạt 50 test | Chưa có bài tự làm/câu trả lời |
| [02b — Xây dữ liệu và audit](lessons/02b_dataset_construction.md) | Đã soạn phương pháp, nhãn, template trống và 20 câu bài tập; code có seed 356 câu hư cấu | Chưa có bài tự làm/rà nhãn của người học |
| 03–07 | Dự kiến theo lộ trình | Chưa bắt đầu |

## Mẫu ghi sau một buổi

```text
Ngày thực hiện:
Bài / TASK / REQ:
Commit code dự án đã dùng:
Lệnh tôi đã chạy:
Dự đoán trước khi chạy:
Kết quả thực tế:
Bài tập tôi tự sửa:
Câu trả lời bằng lời của tôi:
Điều tôi chưa hiểu:
Nhận xét Mentor sau khi đọc bài của tôi:
Bước tiếp theo:
```

Để trống phần kết quả cho đến khi bạn thực hiện. Chỉ ghi mức hiểu sau khi có lời giải thích hoặc bài tự làm; test code pass không chứng minh người học đã hiểu. Không lưu dữ liệu tài chính thật trong nhật ký công khai.

## Chuẩn bị dữ liệu — 05/10/2026

TASK-05–06 có code/seed/provenance trong dự án, [commit tham chiếu](https://github.com/Krev1/spendwise-ai/commit/9ac8ed0c4702b30ce4a26b980595526c4031e03a). Bộ test phần mềm 86 passed + 11 subtests; không phải mức hiểu của người học. Learn có hướng dẫn phương pháp và nhãn, template trống, bài 02b và 20 tình huống.

Thống kê kỹ thuật: 356 hư cấu (319 tự tạo, 37 chuyển ngữ), 0 thật, 33 nhóm, nhãn `ai_draft`, 0 người duyệt. Chưa thu pilot người thật, chưa train/test model. Chờ người học chạy lệnh, tự gán câu và giải thích hạn chế trước khi ghi mức hiểu.

## D13 — Tiến độ học riêng — 05/10/2026

Chat dự án triển khai độc lập; chat học bám [mapping](learning_map.json)/[bàn giao](https://github.com/Krev1/spendwise-ai/blob/6d28c6ab2502e29e202290433fdd374bc4417155/docs/PROJECT_HANDOFF.md). Mốc code học ban đầu 9ac8ed0c4702b30ce4a26b980595526c4031e03a; lần này chỉ chuẩn bị workflow/prompt/mapping/ignore. Chưa tạo practice checkout/môi trường, chưa chạy bài hoặc nhận câu trả lời mới từ người học. Mọi ô mức hiểu ở trên giữ chưa xác minh; tiến độ kỹ thuật không bị chặn bởi trạng thái này.
