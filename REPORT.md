# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602085
- Ngày / CVAT local: 2026-09-17 / CVAT local
- Công cụ đã dùng: Brush / Polygon

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Ghi chú QC cấu trúc (script `inspect_submissions`, tương đương notebook tự kiểm): cả 9 ZIP đều OK (đủ mask PNG / JSON đọc được, labelmap khớp classes của từng task).

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `000000181542.jpg`, object annotation id=1 trong export COCO — class `person`, vùng lớn phía trước/giữa khung (bbox khoảng x=32…640, y=109…462).
- Class và quy tắc tôi dùng để chọn biên: class `person`; chỉ tô phần người nhìn thấy, dừng ở mép thân/quần áo, không kéo mask sang xe/motorcycle sát cạnh và không đoán phần bị che.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: không dùng.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `cp3_thin` / `839f7736-abe28069.jpg` / toàn task (label + mask export)
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: sai lớp (và lần export đầu thiếu PNG mask)
- Bằng chứng tôi nhìn thấy: ZIP đầu chỉ ~439B, không có `SegmentationClass/*.png`; ZIP sau có mask nhưng `labelmap.txt` toàn bicycle/bus/car/person/truck — trong khi task cần `pole`, `traffic sign`, `sky`, `road`. Script báo `label ngoài classes.json` và thiếu class đúng.
- Quy tắc và hành động sửa: tạo lại / gắn đúng `data/checkpoints/cp3_thin/cvat-labels.json`, vẽ lại theo class thin-structure, Save, export lại Segmentation mask 1.1.
- Sau sửa đã Save và export lại chưa? Đã — `submissions/cp3_thin.zip` hiện có PNG mask và labelmap đúng 4 class; QC cấu trúc = OK.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): đã chạy `inspect_submissions` local: `cp3_thin` trước = LỖI (thiếu PNG / sai label), sau = OK; chưa có ground truth nên chưa có điểm IoU. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `hard_panoptic` / `000000350023.jpg` / vùng sky | (A) một mask `sky` duy nhất cho cả khoảng trời; (B) tách thành hai mask `sky` vì bị building/traffic light chia | Stuff thường gộp một vùng liên tục cùng class; export hiện có 2 annotation `sky` (id 6 và 7) | Đã để 2 mask trong ZIP; xin coach xác nhận stuff `sky` có nên gộp một instance/vùng không |
| 2. `cp2_slice` / `000000017627.jpg` / cụm xe sát nhau | (A) gộp các xe cùng class thành một mask; (B) mỗi xe một instance dù sát mép | Quy tắc slice: hai vật cùng class sát nhau vẫn tách; export hiện `bus`×3 + `car`×1 | Chọn tách từng xe (3 bus + 1 car); nhờ xác nhận không còn xe nào bị gộp |
| 3. `cp4_curb` / `7d83710e-4697c3b2.jpg` / ranh road–sidewalk | (A) tô theo màu mặt đường giống nhau; (B) tách theo bó vỉa / chức năng dù màu gần giống | Checkpoint curb yêu cầu biên chức năng; đã export lại với đúng label `road` và `sidewalk` | Chọn (B): tách theo bó vỉa/chức năng; xin coach xác nhận chỗ màu asphalt giống nhau có đúng mép không |
