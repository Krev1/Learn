# Thu thập và xây dựng dữ liệu khoản chi tiếng Việt

Ngày: 05/10/2026. TASK-05–06; REQ-08, REQ-09 (chuẩn bị dữ liệu), REQ-13.
Code tham chiếu: [SpendWise AI, commit 9ac8ed0c4702b30ce4a26b980595526c4031e03a](https://github.com/Krev1/spendwise-ai/tree/9ac8ed0c4702b30ce4a26b980595526c4031e03a).
Mục tiêu: tự giải thích được một mẫu đi từ nguồn → làm sạch → nhãn → kiểm tra → snapshot, và biết vì sao có dữ liệu chưa đủ để kết luận mô hình tốt.

## 1. Bạn đang có gì để bắt đầu?

Đã xây [dataset v0.1](https://github.com/Krev1/spendwise-ai/blob/9ac8ed0c4702b30ce4a26b980595526c4031e03a/data/seed/v0.1/expense_descriptions_vi.csv): **356 mô tả tiếng Việt, 8 nhãn; 319 tự tạo và 37 chuyển ngữ từ nguồn hư cấu công khai**. Có 160 câu nền tự tạo, 19 câu nền chuyển ngữ và 177 biến thể bỏ dấu. Hai trường hợp bỏ dấu không đổi câu được bỏ qua. Toàn bộ vẫn là dữ liệu hư cấu, nhãn `ai_draft`. Chưa có người tham gia, người duyệt nhãn, tập test thật hoặc model.

Không cần ngồi viết 1.200 câu bằng AI để có vẻ đủ dữ liệu. Seed giúp học đọc CSV, audit và chạy thử kỹ thuật. Mô tả thật dùng cho nghiên cứu cần được cung cấp tự nguyện, có quyền sử dụng và đa dạng cách viết. Mục tiêu khoảng 1.200 thật trong spec là mục tiêu thu thập tương lai, không phải số mẫu đã có.

## 2. Bắt đầu bằng câu hỏi nghiên cứu

“Từ mô tả khoản chi tiếng Việt ngắn, mô hình tự huấn luyện có thể gợi ý một trong tám danh mục cho sinh viên/người mới đi làm chưa xuất hiện trong tập học không?”

Đầu vào chỉ là mô tả. Đầu ra một nhãn. Không cần số tiền, ngày, tên, tài khoản hay sao kê. Loại thu nhập, hoàn tiền, chuyển giữa tài khoản của cùng người và câu ngoài miền. Với câu ghép hai mục đích, hỏi tách khoản chi. Viết các quyết định trước khi gán hàng loạt, xem [guideline nhãn v0.1](02_labeling_manual.md).

`khac` là khoản chi hợp lệ ngoài bảy lớp hoặc thiếu ngữ cảnh sau khi hỏi lại. Nó không phải thùng chứa dữ liệu rỗng, thu nhập hay mọi câu tiếng Việt bất kỳ.

## 3. Tìm và quyết định dùng nguồn công khai

Quy trình đã làm: tìm theo “Vietnamese expense description classification”, “phân loại khoản chi tiếng Việt”, “personal finance synthetic transactions”; kiểm tra README, cột dữ liệu, loại thật/hư cấu, giấy phép, ví dụ và commit. Không tải dữ liệu chỉ vì miễn phí đọc trên web. Bảng sau ghi kết quả khảo sát, không phải khẳng định mọi dataset đều đã được tìm hết.

| Nguồn đã khảo sát | Quyết định | Lý do |
|---|---|---|
| [personal-finance-analyzer](https://github.com/lakshitrajput21/personal-finance-analyzer) | Thu snapshot làm tham khảo hư cấu | README nói demo hư cấu; MIT ở repo; có description nhưng tiếng Anh, không có nhãn |
| [ari510-expense-categorization](https://github.com/hoppypilot87/ari510-expense-categorization) | Không nhập vào dataset này | Dữ liệu mô phỏng và trọng tâm đặc trưng số/hồ sơ; không khớp đầu vào chỉ mô tả tiếng Việt |
| [ViReceipt information extraction](https://github.com/duongttr/vireceipt-information-extraction) | Không nhập | Bài toán trích thông tin hóa đơn/OCR, không trực tiếp là tám nhãn; quyền dataset cần kiểm tra riêng |
| [Underserved fraud v2](https://huggingface.co/datasets/Nachammai41/underserved-persona_conditioned-fraud-v2) | Không nhập | Nguồn hư cấu phục vụ nhãn gian lận, khác mục tiêu |
| [Aurora Gate expense challenge](https://www.kaggle.com/competitions/aurora-gate-expense-categorization-challenge/data) | Không tải/tái phân phối | Chưa xác minh đầy đủ điều khoản/giấy phép phù hợp và ánh xạ nhãn |

Trong phạm vi tìm kiếm này chưa xác minh được nguồn sẵn có khớp cả tiếng Việt, tám lớp và quyền tái sử dụng rõ. Vì thế chọn dữ liệu tham khảo hợp lệ + seed tự tạo + quy trình thu thật; không nói rằng đã thu được dữ liệu chi tiêu Việt Nam thực tế.

Nguồn đã chọn được khóa tại commit `5d727c66bf2beba91a54d5cda043b6b2eca97ec3`. Repo nguồn có [MIT license](https://github.com/lakshitrajput21/personal-finance-analyzer/blob/5d727c66bf2beba91a54d5cda043b6b2eca97ec3/LICENSE), chưa thấy giấy phép dataset riêng. Giữ nguyên copyright **2026 Surabhi Pradhan**, notice và URL/commit. Username của repo không thay thế thông tin copyright. README công khai có thể nói “synthetic”, còn phân bố có thể vẫn thiếu đại diện hoặc nhãn sai; giấy phép không xác nhận chất lượng.

## 4. Chúng ta đã biến nguồn thành dữ liệu như thế nào?

1. Lưu CSV demo 100 dòng và license; không chạy code hay model bên thứ ba. Manifest giữ URL, commit, ngày thu và SHA-256 cả raw bytes lẫn phiên bản UTF-8/LF.
2. Trong nguồn này số tiền âm biểu thị khoản chi. Chỉ dùng dấu để chọn phạm vi, không đưa amount/currency/date vào mô hình. Có 6 dòng không âm là thu/hoàn tiền nên loại.
3. Bỏ các token số ở cuối mô tả demo, như `Steam Games 9598` → `Steam Games`, ghi gốc và quyết định trong source selection. Quy tắc chỉ áp dụng nguồn hư cấu này; không tự bỏ số trong mô tả thật như `khóa học Python 101`.
4. Chọn 19 mô tả có ánh xạ dự thảo. Ví dụ `Subway Sandwiches` → `bánh sandwich Subway` → `an_uong`; `MTA Transit` → `giao thông công cộng MTA` → `di_chuyen`. Không thêm địa điểm, khoản tiền hoặc mục đích học tập không có trong câu gốc. `Netflix Subscription` có suy luận thương hiệu; ghi điều đó để rà nhãn.
5. Loại 52 dòng chưa rõ/chưa có ánh xạ. `CVS Pharmacy` không chứng minh đã mua thuốc thay vì mỹ phẩm; `Target Store` không nêu vật đã mua. Không tự ép các mẫu này vào lớp có vẻ hợp lý để tăng số lượng.
6. Gộp 23 dòng nguồn cùng mô tả canonical; giữ nguồn từng dòng tham chiếu vào một ID. Gộp làm thay đổi tần suất gốc, nên dataset câu unique không còn đại diện tần suất giao dịch nguồn.
7. Soạn 160 câu tiếng Việt hư cấu theo 32 họ kịch bản; thêm bản bỏ dấu có `transformation=diacritic_removed`. Giữ bản có dấu và không dấu cùng nhóm. Bản chuyển ngữ vẫn có `source=public_synthetic,true`; dịch sang tiếng Việt không làm nó thành giao dịch thật của người Việt.
8. Kiểm tra hợp đồng, xuất main CSV/provenance/source selection/audit, đóng phiên bản v0.1. Chưa tạo train/validation/test ở bước này.

Xem [recipes và công cụ](https://github.com/Krev1/spendwise-ai/tree/9ac8ed0c4702b30ce4a26b980595526c4031e03a/data). Không có nhãn có sẵn từ nguồn; toàn bộ nhãn được dự án đề xuất bằng AI, chưa thành ground truth được người xác nhận. Để đổi trạng thái, cần lưu ai rà, ngày, guideline, quyết định thực và bất đồng; không chỉ đổi chữ `ai_draft` hàng loạt.

## 5. Hiểu hai loại CSV và metadata

CSV giao dịch phục vụ ứng dụng: `transaction_id,date,transaction_type,amount_vnd,description,category`.
CSV ML phục vụ nghiên cứu: `record_id,description,label,group_id,source,is_synthetic`.

| Trường ML | Ý nghĩa | Lỗi cần tránh |
|---|---|---|
| `record_id` | ID mẫu duy nhất, 1–64 ký tự ASCII chữ/số/`_`/`-` | Dùng số tài khoản/tên người làm ID |
| `description` | Câu UTF-8, có chữ, 1–300 ký tự, đã rà nhận dạng | Chèn nhãn vào câu hoặc giữ PII |
| `label` | Một trong tám slug | Coi nhãn AI chưa duyệt là nhãn người |
| `group_id` | Mã người thật hoặc nguồn/họ mẫu hư cấu | Tạo group mới cho mỗi biến thể để dễ random split |
| `source` | `volunteer`, `author_synthetic`, `public_synthetic` | Nguồn public có nghĩa là thật |
| `is_synthetic` | `false` chỉ với volunteer thật; hai nguồn hư cấu luôn `true` | Đổi thành false sau khi dịch/viết lại |

Ví dụ hư cấu:

```csv
record_id,description,label,group_id,source,is_synthetic
demo_a,ăn trưa cơm gà,an_uong,tpl_meal,author_synthetic,true
demo_b,an trua com ga,an_uong,tpl_meal,author_synthetic,true
```

Provenance là bảng phụ nối bằng ID: trạng thái nhãn, họ câu, hash chuẩn hóa, cách biến đổi, dòng/commit nguồn. Giữ sáu cột chính ổn định; đừng cho metadata như `label`, `source`, ID hay nhóm vào TF-IDF. Mẫu thật cần ledger đồng ý và hồ sơ gán nhãn riêng tư; validator không thể tự kiểm chứng đồng ý hay người rà.

## 6. Thu dữ liệu thật với công cụ hiện có

Đây là **quy trình đề xuất chưa thực hiện**, không có người nào đã được liên hệ trong phiên này. Dùng file CSV hoặc biểu mẫu offline, không cần API trả phí. Bắt đầu pilot 3–5 người tự nguyện, mỗi người khoảng 15–30 mô tả khoản chi thực đã bỏ thông tin cá nhân. Nếu pilot thuận lợi mới mở rộng, ví dụ 10–20 người trong nhiều tuần. Các số này để lập kế hoạch, không phải thống kê đã thu.

Trước thu, điền [mẫu lời mời/đồng ý](../templates/consent_and_invitation.md) theo yêu cầu trường. Người cung cấp có thể bỏ qua câu nhạy cảm và từ chối. Đồng ý nghiên cứu và đồng ý công khai là hai lựa chọn riêng; mặc định không công khai. Không hỏi tên ngân hàng, số tiền, ảnh sao kê, tên người, số tài khoản, email hoặc số điện thoại trong mô tả.

Cho người tham gia tự viết theo cách họ thường ghi. Yêu cầu họ nghĩ về các khoản đã chi thực, không nhìn danh sách seed rồi viết lại. Ghi nguồn hồi tưởng/tự ghi để nhận biết sai lệch. Không tự sửa ngữ pháp và ép mẫu đều đủ dài; câu ngắn, không dấu và viết tắt tự nhiên rất cần. Nếu dùng câu hỏi theo từng lớp để bổ sung lớp thiếu, phải ghi `sampling_mode=category_prompted` trong ledger, vì phân bố đó không còn là mẫu tự nhiên.

Mỗi người có mã `p001`, `p002`... và giữ cùng `group_id` qua mọi lần cung cấp. Mã là pseudonym, không bảo đảm ẩn danh hoàn toàn. Nếu cần liên hệ để rút dữ liệu, để bảng liên hệ ngoài dataset/Git; chỉ thu phần liên hệ thật sự cần. Không đưa giao dịch từ ứng dụng vào nghiên cứu chỉ vì người đó đã dùng ứng dụng.

Từ root repo dự án, tạo khu vực riêng và sao chép các template **trống** lần đầu:

```powershell
New-Item -ItemType Directory -Force -Path data/private
Copy-Item -LiteralPath ..\Learn\spendwise-ai\templates\collection_sheet.csv -Destination data/private/collection_sheet.csv
Copy-Item -LiteralPath ..\Learn\spendwise-ai\templates\consent_ledger.csv -Destination data/private/consent_ledger.csv
Copy-Item -LiteralPath ..\Learn\spendwise-ai\templates\annotation_review.csv -Destination data/private/annotation_review.csv
```

Các lệnh copy dùng khi file đích chưa có; không chạy lại để ghi đè dữ liệu đã điền. Trường trong [template README](../templates/README.md) được giải thích từng cột. `.gitignore` dự án đã bỏ `data/private/`; trước mọi commit vẫn xem `git status`. Không dùng `git add -f` để đưa bản thật lên repo. CSV mở bằng Python/editor UTF-8 hoặc nhập từ Excel với cột mô tả kiểu text; không chạy nội dung ô như lệnh/công thức. Dự án không cần số tiền để tạo dữ liệu ML.

## 7. Làm sạch, gán nhãn và kiểm tra

Giữ bản thu thô riêng, chỉ làm việc trên bản copy. Rà tên người, địa chỉ, đơn vị nhỏ, thời điểm đặc thù, số liên hệ/tài khoản và thông tin sức khỏe có thể nhận dạng. Thay bằng mô tả tối thiểu còn ý nghĩa nếu có thể, rồi duyệt lại; xóa các mẫu không thể giảm nhận dạng hoặc không được đồng ý. Log riêng chỉ giữ ID/lý do, không lặp lại PII.

Chuẩn hóa NFC, lowercase và khoảng trắng để tìm trùng. Ví dụ `ĂN  trưa` và `ăn trưa` cùng khóa. Không xóa dấu cho cấu hình mô hình chính, không sửa mọi lỗi gõ. Với mỗi duplicate, chọn canonical theo quy tắc trước, giữ liên kết bản gộp; nếu nhãn xung đột, dừng và giải quyết theo hướng dẫn, không giữ cả hai.

Hai người gán độc lập một tập pilot 50–100 câu trước khi làm cả bộ. Đừng cho người B nhìn nhãn A hoặc gợi ý AI trước khi gán. Ghi nhãn A/B, người chốt, lý do, ngày và guideline. Nếu chỉ có một người, ghi đúng giới hạn và mời giảng viên/bạn học rà một phần khi có thể; không báo mức đồng thuận giả. Xem [quy trình cụ thể](02_labeling_manual.md).

Khi đủ điều kiện nghiên cứu, export sáu cột vào file riêng trong `data/private/`; label chỉ nhận tám slug và provenance ghi trạng thái duyệt thực. `source=volunteer,is_synthetic=false` chỉ khi câu đến từ khoản chi thật có quyền và đồng ý sử dụng. Trộn hư cấu/real cần báo số lượng riêng; không dùng nhãn AI thuần làm test ground truth.

Validator in số nhóm/lớp, số thật/hư cấu và một số cảnh báo email/URL/số dài. Không phát hiện được mọi tên người hoặc suy luận danh tính. Pass nghĩa là cấu trúc hợp lệ, chưa chứng nhận sạch PII, đúng nhãn, đủ đại diện hay sẵn sàng test.

## 8. Tại sao không random từng hàng?

Nếu `ăn trưa cơm gà` ở train còn `an trua com ga` ở test, mô hình đã gặp gần như cùng ý tưởng. 356 hàng trong seed chỉ có 179 câu nền khi bỏ dấu; chúng không phải 356 quan sát độc lập. Nguồn công khai được giữ chung một nhóm; 32 nhóm còn lại là họ kịch bản, không phải 32 người.

Với dữ liệu thật, giữ mỗi người trong một tập để đánh giá người mới. Văn bản trùng giữa người và họ mẫu có quan hệ đã được xác nhận có thể nối nhóm thành thành phần hiệu lực; không nối mọi câu chỉ vì có chung từ “cơm”. Audit near-duplicate cần ý kiến người và nguồn, không chỉ điểm similarity. [scikit-learn: grouped cross-validation](https://scikit-learn.org/stable/modules/cross_validation.html#cross-validation-iterators-for-grouped-data).

Sau khi kiểm tra nhóm, TASK-08 mới tạo split gần 70/15/15 seed 42 và kiểm tra đủ lớp, giao ID/nhóm/văn bản/họ mẫu. Mục tiêu test thật ≥20 mô tả khác nhau/lớp từ nhiều người giữ riêng là điều kiện thu thập đề xuất, chưa được đáp ứng. Không phá nhóm để đủ tỷ lệ hoặc chọn seed vì điểm đẹp. TF-IDF chỉ `fit` trên train; chọn model/ngưỡng bằng validation, test cuối giữ riêng. [scikit-learn: tránh leakage](https://scikit-learn.org/stable/common_pitfalls.html#data-leakage).

## 9. Đóng phiên bản và trình bày trong đồ án

Một snapshot cần: nguồn/quyền/consent, guideline, số trước/sau làm sạch, lý do loại, phân bố lớp, số người và nhóm, trạng thái nhãn, checksum, commit và lệnh tái tạo. Hash thay đổi khi nội dung thay đổi; hash đúng không biến nhãn sai thành đúng. Dữ liệu thật/split/ledger riêng chỉ được lưu theo quyền; public repo dùng seed hư cấu.

Nếu người tham gia rút mẫu trước khóa dữ liệu, tìm ID/nhóm và loại khỏi snapshot đang chuẩn bị. Nếu mẫu đã tham gia train, ghi phiên bản dataset/model ảnh hưởng và tạo bản thay thế/huấn luyện lại khi cần; xóa một dòng CSV không tự xóa tác động khỏi model cũ. Không hứa dữ liệu đã công khai có thể bị thu hồi khỏi mọi bản sao.

Cách viết trung thực hiện tại: “Xây dựng seed 356 mô tả hư cấu có nguồn và công cụ audit tái lập; chưa thu dữ liệu thật, chưa duyệt nhãn độc lập hoặc đánh giá mô hình.” Khi có kết quả thực, thay bằng số đã đo và giữ bằng chứng. Không viết “356 giao dịch Việt Nam thực” hoặc “AI đạt độ chính xác cao” từ file này.

## 10. Thực hành tiếp theo

Làm [bài 02b — đọc và audit dataset](../lessons/02b_dataset_construction.md), tự gán [20 tình huống hư cấu](../exercises/03_annotation_practice.csv) và ghi câu trả lời vào [nhật ký](../progress.md). Việc học đọc file và pilot dữ liệu thật có thể tiến hành song song; không chờ thu đủ dữ liệu mới học code.

Các nguồn web trong hướng dẫn được kiểm tra ngày 05/10/2026. Tài liệu này giải thích quy trình hiện tại, không thay thế quy định nghiên cứu/đồ án của trường.
