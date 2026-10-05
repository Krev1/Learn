# Bài tập CSV

`transactions_practice.csv` là bản sao 10 giao dịch hư cấu từ [CSV mẫu của dự án](https://github.com/Krev1/spendwise-ai/blob/main/examples/transactions_sample.csv). Đây là dữ liệu thực hành, không phải dataset nghiên cứu AI.

Thực hiện [bài 01](../lessons/01_python_and_money.md): sao chép file sang `spendwise-ai/local/`, thêm khoản chi 45.000 VND, dự đoán tổng rồi chạy chương trình. Tiếp tục thử ngày không tồn tại, ID trùng và tiền âm; ghi kết quả vào [nhật ký](../progress.md).

Mã đọc CSV và test nằm trong [repo dự án](https://github.com/Krev1/spendwise-ai/tree/main/examples). Không sửa CSV mẫu của dự án để làm bài tập vì test đang kiểm chứng tổng tiền của file đó.

## Bài dataset và nhãn

[03_annotation_practice.csv](03_annotation_practice.csv) có 20 tình huống hư cấu, cột câu trả lời để trống. Copy vào `spendwise-ai/local/`, điền decision keep/ask/split/exclude, proposed_label khi đủ thông tin và reason. Header bài tập khác ML schema vì có cả câu ngoài phạm vi để tập xử lý. Đọc [bài 02b](../lessons/02b_dataset_construction.md) và [guideline](../guides/02_labeling_manual.md). Không điền dữ liệu thật vào repo Learn.
