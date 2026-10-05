# Học theo dự án đang phát triển độc lập

Quyết định D13, ngày 05/10/2026. Học bám đầu ra thật của dự án; code có thể tiến lên trong khi người học ôn một phiên bản cố định. [Bàn giao kỹ thuật tại commit giao thức](https://github.com/Krev1/spendwise-ai/blob/6d28c6ab2502e29e202290433fdd374bc4417155/docs/PROJECT_HANDOFF.md); [bản hiện hành](https://github.com/Krev1/spendwise-ai/blob/main/docs/PROJECT_HANDOFF.md).

## 1. Hai chat và quyền ghi

| Chat | Nơi làm việc | Trách nhiệm |
|---|---|---|
| Dự án | spendwise-ai | SDD → code → tests → bằng chứng → commit/bàn giao; tiếp tục task đủ đầu vào |
| Học | Learn/spendwise-ai | Đọc nguồn theo commit → bài học → bài tự làm → phản hồi; không sửa sản phẩm |

Hai chat có prompt riêng: [dự án](https://github.com/Krev1/spendwise-ai/blob/main/prompts/START_HERE.md), [Mentor](MENTOR_PROMPT.md). Người dùng mở chat khi muốn làm việc với luồng đó. Việc đồng bộ mô tả ở đây là đọc file/commit vào đầu buổi, không phải một automation hoặc gửi tin nhắn qua chat.

## 2. Chọn mốc học

Mentor đọc bàn giao tại commit đã publish, xem code/test và đối chiếu [learning_map.json](learning_map.json). Chọn mốc phù hợp phần người học chưa hiểu, ghi SHA đầy đủ, TASK/REQ, source file/hàm và mục tiêu bài. Chỉ dùng bằng chứng tương ứng snapshot; không gán kết quả tests mới cho code cũ hoặc nhầm hướng dẫn của hai version.

Mapping ban đầu: 00/01/02/02b dựa code `9ac8ed0c4702b30ce4a26b980595526c4031e03a`. Những bài có link f8e91ee/23c91b0 giữ giá trị tham chiếu lịch sử; Mentor dùng mapping/commit đã chọn và ghi rõ khi cập nhật bài. Baseline/model/SQLite/UI vẫn chưa có ở mốc này; lý thuyết có thể chuẩn bị nhưng không được ghi là đã triển khai.

## 3. Checkout và môi trường thực hành riêng

Chạy từ **root repo Learn**, tạo practice lần đầu; thư mục này được ignore bởi Learn/spendwise-ai/.gitignore. Kiểm tra `spendwise-ai/practice/project` chưa tồn tại trước khi clone; nếu đã có bài đang làm, giữ nó và tiếp tục, không reset/copy đè.

```powershell
New-Item -ItemType Directory -Force -Path spendwise-ai/practice | Out-Null
git clone https://github.com/Krev1/spendwise-ai.git spendwise-ai/practice/project
Set-Location -LiteralPath spendwise-ai/practice/project
git switch --detach 9ac8ed0c4702b30ce4a26b980595526c4031e03a
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.lock.txt
.\.venv\Scripts\python.exe -B scripts/check_environment.py
```

`--detach` mở đúng commit đang học trong bản clone riêng, không đổi checkout kỹ thuật. `.venv` ở bản practice riêng, không dùng/cài vào môi trường của người triển khai. Clone/cài lần đầu cần mạng; giữ lock đã kiểm tra, không tự nâng phiên bản toàn máy. Đây là lệnh hướng dẫn, **chưa có practice checkout/môi trường học được tạo bởi lần cập nhật workflow này**.

Các lệnh bài 00/01/02/02b ghi “root spendwise-ai” nay chạy từ root checkout thực hành `practice/project`, nơi có README/src/examples/scripts/.venv tương ứng snapshot. Không chạy chúng từ root Learn hoặc checkout triển khai. Bài tập cần CSV luyện tập từ Learn: từ root practice/project, đường dẫn là `../../exercises/transactions_practice.csv`; chỉ copy sang local/ khi file đích chưa có. Có thể copy CSV demo bên trong checkout học để sửa. Không copy đè bài tự làm. Lệnh cũ `..\Learn\...` của bố trí hai repo cạnh nhau không dùng trong layout practice này.

Ví dụ sau khi cài trong checkout học:

```powershell
.\.venv\Scripts\python.exe -B scripts/preview_transactions.py examples/transactions_sample.csv --month 2026-10
```

Sửa CSV trong `local/` của practice để thử khoản chi 45.000 đồng; không sửa CSV gốc đang được test. Lưu lời giải/nhật ký đã rà vào Learn; không commit clone, .venv, DB hoặc real data. Trước publish xem `git status` ở root Learn, không dùng add -f để đưa practice lên repo.

## 4. Nhịp học độc lập

Một buổi: đọc mốc → ví dụ/kiến thức → code thật → tự dự đoán → thực hành → tự trình bày → phản hồi. Có thể chia một mốc code thành nhiều buổi; không cần chạy đua với tốc độ dự án. [progress.md](progress.md) chỉ ghi mức hiểu sau lời giải/bài tự làm, khác progress kỹ thuật của dự án.

Dự án có commit mới trong lúc học: giữ snapshot hiện tại đến hết buổi, ghi phiên bản. Đầu buổi sau Mentor đọc bàn giao mới, chọn thêm mốc/mapping và bổ sung bài tương ứng. Không tự `pull/reset` bản practice có bài tự làm. Nếu muốn học mốc khác, dùng bản thực hành khác trong practice/ hoặc commit/sao lưu bài của mình trước khi chuyển phiên bản; không thay source chỉ để kết quả khớp tài liệu.

## 5. Báo lỗi và giới hạn thật

Nếu phát hiện lỗi sản phẩm, lưu phản hồi trong Learn với commit, TASK, lệnh, expected/actual và bước tái hiện đã bỏ PII. Người dùng đưa phản hồi cho chat dự án; Mentor không tự sửa source triển khai hay nhắn sang chat khác. Khi dự án sửa, bài sau tham chiếu commit sửa và giải thích lỗi.

Consent/nhãn người duyệt/dữ liệu thật không thể bỏ qua vì hai luồng độc lập. Seed 356 câu hoàn toàn hư cấu, human-reviewed bằng 0 và chưa có metric. Giới hạn chất lượng AI cần được dạy theo bằng chứng, không coi một bài test phần mềm là chứng minh hiệu quả thực tế.
