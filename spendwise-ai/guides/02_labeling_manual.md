# Guideline gán nhãn khoản chi — v0.1

TASK-05–06 / REQ-08,09,13. Ngày: 05/10/2026. [Commit code](https://github.com/Krev1/spendwise-ai/tree/9ac8ed0c4702b30ce4a26b980595526c4031e03a). Nguồn hợp đồng: [spec dữ liệu](https://github.com/Krev1/spendwise-ai/blob/9ac8ed0c4702b30ce4a26b980595526c4031e03a/docs/05_data_and_ml.md). Đây là hướng dẫn nhãn đề xuất, chưa được pilot người thật xác nhận.

## Quyết định trước khi chọn nhãn

Đọc mô tả trước khi nhìn gợi ý AI. Hỏi: đây có phải khoản chi thật theo phạm vi nghiên cứu và đã có quyền dùng không? Nếu thu/hoàn tiền/chuyển giữa tài khoản của cùng người, loại khỏi dataset chi. Nếu rỗng/vô nghĩa/ngoài miền, loại và ghi lý do. Nếu câu ghép hai mục đích, hỏi tách; không lấy nhãn đầu tiên. Nếu thiếu đối tượng, hỏi lại; khoản chi vẫn hợp lệ nhưng không bổ sung được mới ghi `khac`.

Không làm câu dễ hơn bằng cách chèn từ khóa nhãn. Ví dụ người cung cấp viết “Grab”: chưa biết dịch vụ nào, không đổi thành “Grab đi làm” theo suy đoán. Nội dung bổ sung phải đến từ người cung cấp và ghi lịch sử.

## Tám nhãn

| Slug | Bao gồm | Ranh giới |
|---|---|---|
| `an_uong` | Bữa ăn, đồ uống, thực phẩm nấu ăn | Thực phẩm trong siêu thị được xếp ở đây, không dựa vào tên siêu thị |
| `di_chuyen` | Vé/chuyến xe, xăng, gửi xe, sửa xe cá nhân | GrabFood khác Grab chuyến xe; phí gửi bưu kiện là `khac` |
| `nha_o_hoa_don` | Thuê chỗ ở, điện, nước, internet nhà, cước điện thoại | Khách sạn/chuyến du lịch cần chốt chính sách trước, chưa tự ép vào thuê nhà |
| `hoc_tap` | Học phí, giáo trình, dụng cụ/khóa học mục đích học | Laptop chỉ học tập khi mô tả nói rõ phục vụ học; mua laptop chung là `mua_sam` |
| `mua_sam` | Quần áo, đồ dùng, mỹ phẩm, hàng hóa không thuộc lớp chuyên biệt | Thuốc/thiết bị y tế rõ nghĩa sang `suc_khoe`; sữa rửa mặt ở nhà thuốc vẫn `mua_sam` |
| `giai_tri` | Phim, nhạc, trò chơi, vui chơi, truyện đọc giải trí | Sách luyện thi là học tập; truyện đọc thư giãn là giải trí |
| `suc_khoe` | Khám, thuốc, xét nghiệm, vật dụng y tế rõ mục đích | “Nhà thuốc” chỉ nơi mua, chưa chứng minh mua thuốc; gym cần quyết định chính sách riêng |
| `khac` | Khoản chi hợp lệ ngoài bảy lớp hoặc còn thiếu ngữ cảnh sau hỏi | Không dùng cho income, câu rỗng, chuyển tiền nội bộ hoặc câu không phải chi |

## Ví dụ đã phân tích, đều hư cấu

| Mô tả | Hành động/nhãn | Lý do |
|---|---|---|
| `GrabFood cơm trưa` | `an_uong` | Có đối tượng đồ ăn |
| `Grab đi làm` | `di_chuyen` | Có mục đích chuyến đi |
| `Grab` | Hỏi lại; nếu không bổ sung → `khac` | Chưa rõ dịch vụ, vẫn phải biết đây là khoản chi |
| `siêu thị mua rau` | `an_uong` | Biết đã mua thực phẩm |
| `siêu thị mua nước giặt` | `mua_sam` | Biết đã mua đồ dùng |
| `Shopee` | Hỏi vật đã mua | Nơi mua không quyết định nhãn |
| `kem dưỡng da mua ở nhà thuốc` | `mua_sam` | Mỹ phẩm, không tự suy ra điều trị |
| `thuốc ho theo đơn` | `suc_khoe` | Nêu thuốc điều trị |
| `mua laptop` | `mua_sam` | Chưa có mục đích chuyên biệt |
| `mua laptop để làm bài tập lập trình` | `hoc_tap` | Nêu rõ mục đích học |
| `mua truyện đọc cuối tuần` | `giai_tri` | Đọc giải trí |
| `cơm trưa và vé phim` | Tách hai khoản hoặc loại nếu không thể tách | Một câu chứa hai nhãn khác nhau |
| `nhận lương`, `được hoàn tiền` | Loại | Không phải expense training sample |
| `chuyển từ ví sang tài khoản của tôi` | Loại | Chuyển nội bộ, không phải khoản chi tiêu |
| `...` hoặc `đặt lịch họp nhóm` | Loại | Không có mô tả khoản chi hợp lệ |

`khac` vẫn là lớp trong miền, không bảo đảm mô hình phát hiện mọi ngoài miền. Với chính sách chưa rõ, tạo quyết định guideline mới và rà các mẫu liên quan; không tự xem một kết quả đoán là quy tắc.

## Gán độc lập và xử lý bất đồng

1. Khóa guideline v0.1 và một batch pilot 50–100 mẫu có quyền. Mỗi người gán nhãn riêng, không xem cột của nhau hoặc nhãn AI.
2. Dùng `annotation_review.csv` trong thư mục riêng. Người A/B ghi slug hoặc quyết định hỏi/tách/loại ở trường phù hợp; chưa thống nhất thì chưa export thành nhãn cuối.
3. So A/B để tìm bất đồng. Reviewer đối chiếu quy tắc và thông tin do người cung cấp bổ sung; ghi nhãn cuối, lý do, người chốt và ngày. Không điền “human_verified” khi reviewer chưa làm.
4. Chỉ sau khi quyết định thực được lưu, export phần hợp lệ sang schema ML sáu cột. Giữ raw/provenance/ledger riêng, không công khai.
5. Nếu sửa guideline, ghi v0.2 và lý do; rà lại lớp/câu bị ảnh hưởng, tăng phiên bản dataset. Không sửa test theo lỗi model sau khi đã dùng test để kết luận cùng một vòng nghiên cứu.

Đồng thuận thô = số mẫu A/B cùng quyết định trên cùng batch / số mẫu cả hai đã gán. Báo cả tử/mẫu và cách xử lý loại/hỏi lại. Khi có hai người gán độc lập thật có thể thêm [Cohen's kappa](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.cohen_kappa_score.html). Ví dụ 42/50 = 84% chỉ là bài tính minh họa, không phải số của dự án. Hai người có thể cùng sai; agreement không thay ground truth có căn cứ.

Chỉ một người tự gán thì ghi “single annotator”, reviewer coverage thực và giới hạn; không tạo tên A/B giả. Seed v0.1 đang có 0 human-reviewed. Bạn có thể rà trước 20 mẫu để tìm vấn đề, nhưng đó chưa phải nghiên cứu đồng thuận hai người.

## Checklist một mẫu trước khi export

- Quyền/đồng ý nghiên cứu có trong ledger riêng, đúng nguồn thật/hư cấu.
- Câu mô tả một khoản chi, trong phạm vi, đã rà thông tin nhận dạng.
- Nhãn theo guideline; bất đồng/mơ hồ đã có quyết định thật.
- ID ổn định; nhóm là người hoặc họ mẫu, không đổi theo nhãn/điểm model.
- Trùng/gần trùng có quan hệ đã audit; có hồ sơ gộp/loại.
- Annotation status phản ánh việc đã làm, không phải điều mong muốn.

Đọc [phương pháp thu thập](01_dataset_collection.md), [template](../templates/README.md) và làm [bài thực hành](../lessons/02b_dataset_construction.md).
