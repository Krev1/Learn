# Bài tập 02 — Đọc hợp đồng và sửa CSV

Liên quan [bài 02](../lessons/02_csv_and_labels.md). Dùng dữ liệu hư cấu và giữ CSV mẫu của dự án nguyên vẹn.

## Chuẩn bị

Từ thư mục repo dự án `spendwise-ai`, với Learn nằm cạnh nó:

```powershell
New-Item -ItemType Directory -Path local -Force | Out-Null
Copy-Item -LiteralPath ..\Learn\spendwise-ai\exercises\transactions_practice.csv -Destination local\lesson_02.csv
.\.venv\Scripts\python.exe -B scripts/preview_transactions.py local/lesson_02.csv --month 2026-10
```

Sửa bản trong `local/`; không sửa `examples/transactions_sample.csv`. Nếu đã có bài tự làm trong `local/lesson_02.csv`, giữ bản đó và dùng tên file mới để không ghi đè bài.

## Mỗi lần chỉ thay một điều

| Tình huống | Việc bạn thử | Ghi nhận trước khi chạy |
|---|---|---|
| Tiền | Đổi một khoản chi thành 12.5 | Chương trình nên chấp nhận hay từ chối? |
| Ngày | Đổi ngày thành 2026-02-30 | Chuỗi đúng định dạng đã đủ chưa? |
| ID | Sao chép một dòng và giữ nguyên transaction_id | Vì sao tổng chi có thể bị sai nếu không phát hiện? |
| Dấu phẩy | Đổi mô tả thành `"Cơm, trà đá"` | Số cột phải còn là bao nhiêu? |
| Nhãn | Đổi category của một khoản thu thành an_uong | Thu có thuộc bộ nhãn khoản chi không? |
| Tháng | Chạy lại với --month 2026-11 | Tháng chưa có giao dịch nên hiển thị gì? |

Với từng tình huống: ghi dự đoán, chạy, ghi output và `$LASTEXITCODE`, rồi phục hồi CSV hợp lệ trước khi thử tình huống kế tiếp. Không sửa nhiều yếu tố cùng lúc khiến bạn không biết lỗi đến từ đâu.

## Tự xây CSV nhỏ

Tạo `local/my_three_transactions.csv` với header đúng, một khoản thu, hai khoản chi và ba ID khác nhau. Số tiền/mô tả do bạn tự chọn nhưng đều hư cấu. Tính thu, chi, chênh lệch bằng tay; chạy CLI để đối chiếu.

Thử cho một khoản chi thiếu category. Quan sát tổng tiền và `pending_expense_categories`; giải thích vì sao preview vẫn tính tiền nhưng ứng dụng tương lai chưa được lưu giao dịch hoàn tất.

## Bằng chứng của bạn

Ghi vào [nhật ký học](../progress.md): CSV hư cấu đã viết, lệnh chạy, dự đoán, kết quả, lỗi đã sửa và ít nhất ba câu trả lời ở cuối bài 02. Không đưa dữ liệu tài chính thật hoặc đường dẫn chứa thông tin riêng tư lên Learn công khai.
