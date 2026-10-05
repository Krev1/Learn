# Biểu mẫu trống cho thu thập và gán nhãn

TASK-05–06 / REQ-08,13. [Commit code](https://github.com/Krev1/spendwise-ai/tree/9ac8ed0c4702b30ce4a26b980595526c4031e03a). Các CSV chỉ có header, không có người tham gia hay dữ liệu thật. Không điền real vào repo Learn. Copy lần đầu vào `spendwise-ai/data/private/`, đã được Git ignore; không copy đè một file đang có dữ liệu.

| File | Từng trường và cách dùng |
|---|---|
| [collection_sheet.csv](collection_sheet.csv) | `record_id`: mã mẫu duy nhất; `description`: câu đã rà nhận dạng; `group_id`: mã người giữ cố định; `capture_mode`: `self_recorded`, `recalled` hoặc `category_prompted` theo cách thực thu; `collection_batch`: mã đợt |
| [consent_ledger.csv](consent_ledger.csv) | `group_id`: mã người; `research_consent`: yes/no thật; `consent_record_ref`: mã tham chiếu bằng chứng riêng; `consent_date`: ngày thật; `invitation_version`: phiên bản mục đích đã đồng ý; `publication_consent`: yes/no riêng, mặc định no; `withdrawal_deadline`: mốc đã thông báo; `status`: active/withdrawn theo thực tế |
| [annotation_review.csv](annotation_review.csv) | `record_id`: nối mẫu; `annotator_a_label`, `annotator_b_label`: ghi độc lập, chưa quyết được để trống và dùng decision; `decision`: keep/ask/split/exclude; `final_label`: slug sau chốt, chỉ điền khi keep; `reviewer_code`: mã người rà; `review_date`: ngày thật; `guideline_version`: v0.1 hoặc bản đã dùng; `reason`: căn cứ, không chứa PII |
| [ml_dataset_header.csv](ml_dataset_header.csv) | Header sáu cột để export mẫu giữ lại đã rà; điền source/cờ đúng thực tế, metadata đồng ý/duyệt lưu riêng. File chỉ header sẽ bị validator từ chối vì chưa có bản ghi |
| [consent_and_invitation.md](consent_and_invitation.md) | Mẫu lời mời cần điền trước khi dùng; tách nghiên cứu và công khai, không tự coi im lặng là consent |

Hai annotator phải nhận hai bảng riêng không có nhãn của nhau; bảng annotation_review chỉ tổng hợp sau khi cả hai đã gán. Nếu chỉ có một người, để cột B trống và ghi giới hạn thật, không tạo “annotator B” là một lời gọi AI khác để coi là hai người độc lập.

Template không tự kiểm soát consent, quyền hay ẩn danh. Chủ đồ án rà bằng chứng trước khi export. Các trường public template không yêu cầu số tiền, tên, email hoặc điện thoại. Nếu cần liên hệ người tham gia, bảng liên hệ nằm ngoài dataset/Git và không được đưa vào báo cáo.

Quy tắc raw CSV trong sheet thu thập khác hợp đồng dataset ML; không đưa collection sheet vào validator ML trực tiếp. CSV UTF-8, dấu phẩy trong mô tả phải được quote đúng bằng công cụ CSV. Khi dùng phần mềm bảng tính, nhập mô tả kiểu text, không chạy công thức/lệnh từ nội dung người nhập. Hướng dẫn bước tiếp ở [phương pháp](../guides/01_dataset_collection.md) và [nhãn](../guides/02_labeling_manual.md).
