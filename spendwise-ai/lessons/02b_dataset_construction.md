# Bài 02b — Đọc, truy vết và audit một dataset

TASK-05–06, REQ-08,09,13. [Commit code đã dùng](https://github.com/Krev1/spendwise-ai/tree/9ac8ed0c4702b30ce4a26b980595526c4031e03a). Chuẩn bị: đọc bài 01/02, dùng `.venv` trong repo dự án. Không cần train để học bài này.

## Đích của buổi học

Bạn phân biệt CSV giao dịch với CSV mô hình, giải thích `source/is_synthetic/group_id`, tìm được nguồn của một mẫu và hiểu giới hạn của validator. Đọc [phương pháp thu thập](../guides/01_dataset_collection.md) và [nhãn](../guides/02_labeling_manual.md) trước thực hành. Dataset/code ở repo dự án, bài học ở Learn.

## Bước 1 — Chạy đối chiếu và đọc báo cáo

Từ root checkout `spendwise-ai` sau `git pull`:

```powershell
.\.venv\Scripts\python.exe -B scripts/collect_reference.py
.\.venv\Scripts\python.exe -B scripts/build_seed_dataset.py
.\.venv\Scripts\python.exe -B scripts/validate_dataset.py data/seed/v0.1/expense_descriptions_vi.csv
```

Đầu ra mong đợi của bản code tham chiếu: builder `reproduced_exactly`, 356 mẫu, 0 thật; validator có 319 `author_synthetic`, 37 `public_synthetic`, đủ tám lớp, 33 nhóm, 179 câu khi gộp khác dấu. Chưa có metric. Cột `real_evaluation_readiness` ghi chưa đánh giá; pass cấu trúc không có nghĩa sẵn sàng bảo vệ chất lượng AI.

Collector kiểm tra hash CSV/license đã lưu, không thu người thật. Thêm `--verify-online` để đọc lại tại commit nguồn khi có mạng. Builder không có `--write` thì chỉ so byte, không sửa snapshot. Validator chỉ đọc, không sửa câu hay gán nhãn. Các đường dẫn truyền từ root repo; script được viết để import code đúng cả khi cwd khác.

## Bước 2 — Tự đọc bằng Python

Tạo `local/inspect_seed.py` trong repo dự án (`local/` bị ignore), chép rồi tự gõ lại đoạn ngắn:

```python
import csv
from collections import Counter
from pathlib import Path

path = Path("data/seed/v0.1/expense_descriptions_vi.csv")
with path.open(encoding="utf-8", newline="") as stream:
    rows = list(csv.DictReader(stream))

print("Số hàng:", len(rows))
print("Theo nhãn:", Counter(row["label"] for row in rows))
print("Theo nguồn:", Counter(row["source"] for row in rows))
print("Số nhóm:", len({row["group_id"] for row in rows}))
```

Chạy `.\.venv\Scripts\python.exe -B local/inspect_seed.py`. `Path` biểu diễn đường dẫn; `with` đóng file khi đọc xong; `newline=""` để csv xử lý newline; DictReader cho mỗi hàng là dict theo header; list giữ các hàng để xem nhiều lần; Counter đếm giá trị; set loại nhóm trùng. Cờ trong DictReader là chuỗi `"true"`/`"false"`: `bool("false")` vẫn True vì chuỗi không rỗng, nên không dùng `bool()` để parse cờ.

Đối chiếu số bạn in với audit_report. Hãy sửa script để chỉ in ID năm mẫu công khai, không cần in toàn dữ liệu riêng tư khi sau này đọc real.

## Bước 3 — Truy vết một mẫu

Chọn một `pub_pfa_*` trong main CSV. Tìm ID đó ở provenance: dòng nguồn, commit, transformation và lý do nhãn. Mở source selection ở dòng nguồn, rồi xem mô tả tiếng Anh trong reference. Viết lại đường đi bằng lời của bạn và chỉ ra phần dịch, phần suy luận nhãn, phần giữ nhóm. ID `_nd` là biến thể bỏ dấu của cùng mẫu, không phải người thứ hai.

Một hash giống nhau giúp kiểm tra nội dung khớp, không chứng minh người thật đã chi hay nhãn đúng. `human_reviewed_records=0` trong audit là tình trạng hiện tại; bạn cần ghi hồ sơ rà thực tế để cập nhật trong phiên bản tiếp theo.

## Bước 4 — Làm hỏng bản copy để hiểu lỗi

Tạo `local/seed_experiment.csv` bằng bản copy seed, giữ file gốc để test. Mỗi lần chỉ đổi một điều, dự đoán trước rồi chạy validator:

1. Đổi `label` thành `food` → lỗi nhãn ngoài tám slug.
2. Đổi cờ một dòng `public_synthetic` thành `false` → lỗi nguồn/cờ không nhất quán.
3. Sao chép một hàng, giữ nguyên ID → lỗi record_id trùng.
4. Đổi ID nhưng giữ nguyên câu và nhãn → lỗi trùng văn bản chuẩn hóa; thay nhãn → lỗi xung đột.
5. Chuyển một câu không dấu sang group mới → lỗi biến thể giao nhóm.

```powershell
.\.venv\Scripts\python.exe -B scripts/validate_dataset.py local/seed_experiment.csv
$LASTEXITCODE
```

Mong đợi exit 1 và lỗi có số dòng, không phải JSON thành công. Mỗi phép thử bắt đầu từ bản copy sạch. Không sửa bản seed hoặc dùng `--write` chỉ để làm lỗi biến mất. Khi thật sự cập nhật recipe, phiên bản và hồ sơ nhãn phải giải thích thay đổi.

## Bước 5 — Gán nhãn và tự giải thích

[03_annotation_practice.csv](../exercises/03_annotation_practice.csv) có 20 câu hư cấu. Copy vào `local/`, điền `decision`, `proposed_label`, `reason`. Các cột của bài tập khác schema ML; bài tập chứa cả trường hợp cần hỏi/tách/loại để bạn tập quyết định trước. Đừng chạy nó bằng validator dataset và cho rằng header sai là bài tập sai.

Sau đó rà 20 mẫu seed trong file copy, ghi ID, quyết định và căn cứ. Bạn có thể phản biện nhãn dự thảo. Gửi kết quả đã bỏ thông tin riêng tư hoặc ghi nhật ký; chưa có sản phẩm này thì Mentor chưa xác nhận bạn hiểu.

## Câu hỏi để chuẩn bị bảo vệ

1. 356 hàng có phải 356 giao dịch Việt Nam thật không? Số người thật/nhãn human-reviewed hiện bao nhiêu?
2. Vì sao cần cả record_id và group_id? Cặp có dấu/không dấu nên chia thế nào?
3. Vì sao CVS Pharmacy không tự có nhãn sức khỏe? Bạn xử lý Grab thế nào?
4. Có giấy phép MIT thì có đồng nghĩa nhãn đúng và dữ liệu đại diện Việt Nam không?
5. Pass validator kiểm tra điều gì và chưa kiểm tra được điều gì?
6. Tại sao không cho source/group_id/label vào vectorizer?
7. Vì sao dịch mô tả English không biến `is_synthetic` thành false?
8. Khi có dữ liệu thật, làm sao giữ người mới trong test và tránh fit TF-IDF trên test?

Ghi lệnh, kết quả bạn thực chạy, mẫu lỗi và câu trả lời bằng lời của bạn vào [progress.md](../progress.md). Kết quả test code không thay bài tự làm.
