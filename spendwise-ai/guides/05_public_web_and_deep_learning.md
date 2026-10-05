# Hướng dẫn05 — Bắt đầu với ngân sách, website và Deep Learning

Ngày: 06/10/2026. Loại: **kiến thức chuẩn bị theo SDD**, chưa là bài giải thích code web/neural đã triển khai. SW-REQ-01…14, SW-TASK-01…16. Snapshot code cũ đã đọc khi lập kế hoạch: `1af601f813f42171e61ef36238f3f84db3178551`. SHA tài liệu SDD: `e2221c7de66d0ac676cf3d6936d64c65a930e045`.

## 1. Ngữ cảnh và hai luồng

Bạn làm một mình, Windows, RAM 32 GB, RTX 3080 10 GB do bạn báo, chưa smoke CUDA, chưa có lịch sử riêng/deadline/giờ tuần cố định, phí thêm 0 đồng. Website nhiều tài khoản có ngân sách tháng tổng/nhóm và hai cảnh báo; đồ án bắt buộc neural phân loại chi tiếng Việt tự train.

Chat dự án làm code/test/SDD/evidence trong `spendwise-ai`, không chờ bạn học xong. Chat học viết Learn, chọn SHA thực cho buổi học, thực hành trong checkout riêng. Spec mới/test pass không chứng minh tính năng đã code hoặc bạn đã hiểu.

[SDD dự án](https://github.com/Krev1/spendwise-ai/tree/main/docs/sdd-v0.2) · [Prompt dự án](https://github.com/Krev1/spendwise-ai/blob/main/prompts/START_HERE.md) · [Prompt học](../MENTOR_PUBLIC_WEB_PROMPT.md).

## 2. Bạn thực hiện từ bước một

1. **Đọc trang đầu SDD và yêu cầu**, viết bằng lời mình: ai dùng, cảnh báo nào hữu ích, dữ liệu nào quyết định số tiền. Chưa cần học hết Django/PyTorch ngay.
2. **Hỏi giảng viên rubric thực**: neural nào bắt buộc; train từ đầu hay fine-tuning được tính; baseline/metric/report/demo cần gì. Ghi câu trả lời thật, không tự ghi trường đã duyệt.
3. **Bắt đầu nhật ký chi tiêu private trên máy**: ngày, loại thu/chi, int VND, mô tả ngắn, nhãn. Không ghi số tài khoản/tên người không cần thiết. Cuối ngày đánh dấu đã ghi đầy đủ/chưa đầy đủ; ngày không chi cũng xác nhận. Không gửi file thật lên GitHub/chat. Chưa có app vẫn ghi bằng công cụ bạn đã có.
4. **Hai prompt đúng hai chat**: dự án dùng START_HERE, bắt đầu SW-TASK-01 kiểm kê; học dùng MENTOR_PUBLIC_WEB_PROMPT và bắt đầu ôn Python/CSV/tiền hiện có. Bạn tự tạo chat nếu muốn, lượt này không tạo chat thay bạn.
5. **Ôn code bằng dữ liệu hư cấu** theo lesson00/01/02 hiện có: dự đoán tổng trước chạy, giải thích int/validation/confirm. Dùng checkout thực hành/môi trường riêng theo [study_workflow](../study_workflow.md), không cài/train vào checkout triển khai.
6. **Thu pilot khi chuẩn bị được consent/guideline** theo [guide01](01_dataset_collection.md), [guideline02](02_labeling_manual.md), [templates](../templates/README.md). Bạn tự chọn/mời người tự nguyện; AI không tự liên hệ hoặc giả người gán nhãn thứ hai. Thiếu nhãn/real data vẫn học/core/smoke được.
7. **Sau mốc code mới**, Mentor đọc handoff+commit và tạo bài file/hàm thật → lý do → lệnh → lỗi → tự làm → câu hỏi bảo vệ. Ghi phần chưa hiểu, không làm bài thay bạn.

Seed 356 câu hiện có chỉ smoke; nhãn AI chưa thành dữ liệu thật kiểm chứng. Không cần bắt đầu bằng tải dataset khổng lồ hoặc transformer.

## 3. Hiểu ba phần khác nhau

| Phần | Nhiệm vụ | Cần học/bằng chứng |
|---|---|---|
| Ngân sách/cảnh báo | Cộng expense xác nhận, so giới hạn | int/date/CRUD/transaction/owner/tests |
| Neural classifier | Description Việt → một trong 8 nhãn gợi ý | embedding/conv/logits/loss/backprop/overfit, train/val/test thật |
| Forecast pace-v1 | Chi tới ngày chốt ÷ số ngày × ngày tháng | coverage/missingness/seasonality/backtest; công thức không là neural |

Ví dụ **hư cấu**: ngân sách ăn uống 1.000.000, chi 800.000 → gần giới hạn 80%. Hết ngày 10 trong tháng 30 ngày, chi đã chốt 1.000.000 và ghi đủ ngày 1–10 → xu hướng 3.000.000. Chi không đều khiến ước tính sai; thực tế và forecast phải hiển thị riêng.

“Học phí” AI xếp mua sắm làm tổng chi vẫn đúng nhưng cảnh báo nhóm học tập sai. Vì vậy user xác nhận và model đánh giá theo lớp; mạng neural không cộng tiền thay code.

## 4. Lộ trình bám mốc code

| Mốc | Nội dung học | Bài tự làm |
|---|---|---|
| Domain/CSV hiện có | function/list/dataclass/exception/int/CSV/test | Thêm khoản hư cấu và giải thích lỗi validate |
| SW-TASK-03 | budget/phần trăm/ngày/tháng/thiếu dữ liệu | Giải thích80% / 100% và ngày không có dòng chưa chắc không chi |
| SW-TASK-04…07 | HTTP/form/session/CSRF/ORM/FK/owner/atomic | Chỉ ra nơi kiểm tra quyền, giải thích rollback/reimport |
| SW-TASK-08…10 | consent/annotation/leakage/TF-IDF/baseline | Gán nhãn20 câu hư cấu, giải thích duplicate leakage |
| SW-TASK-11 | tensor/embedding/CNN/loss/optimizer/early stop | Vẽ shape batch, giải thích train/validation/PAD/UNK |
| SW-TASK-12…13 | metrics/model card/forecast backtest | Đọc confusion matrix thật, phân tích lỗi cụ thể |
| SW-TASK-14…16 | public/evidence/bảo vệ | Demo hư cấu, trả lời limits bằng evidence |

Mốc chưa có code ghi “chuẩn bị lý thuyết”; chỉ đổi thành bài implementation sau đọc file/test/SHA thực. Nếu chưa giải thích được ghi not_verified, không giữ dự án chờ học.

## 5. Câu hỏi buổi đầu

- Vì sao chênh lệch thu–chi không phải số dư ngân hàng?
- Vì sao model score 0,95 vẫn cần xác nhận?
- Vì sao không lấy label/group_id/amount làm feature classifier?
- Tập test thiếu lớp không chứng minh được điều gì?
- Deep Learning dùng ở đâu; forecast phần nào chưa neural?
- Một tuần chưa ghi đủ làm forecast hiểu nhầm thế nào?

Bạn tự trả lời khi học; tài liệu này không ghi bạn đã hiểu/làm xong bài nào.
