# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 202603050
- Ngày / CVAT local: 11/11/2026 / CVAT local (http://localhost:8080)
- Công cụ đã dùng: Polygon, Brush, Intelligent Scissors, SAM (Segment Anything)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

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

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, object là người lái xe máy (`person`) ở vị trí trung tâm / tiền cảnh của ảnh (tọa độ bbox `[190, 120, 449, 338]`).
- Class và quy tắc tôi dùng để chọn biên: Class `person`. Quy tắc biên: Chỉ gán nhãn phần thân thể thực sự nhìn thấy được của người lái (đầu đội mũ bảo hiểm, áo khoác, hai cánh tay cầm lái và chân); dừng mask chính xác tại mép tiếp giáp với yên xe, tay lái và thân xe máy (`motorcycle`), không vẽ lấn sang vùng của xe máy và không suy đoán hay vẽ phần cơ thể bị che khuất phía sau thân xe.
- Nếu dùng gợi ý sau đó: Vùng gợi ý AI/SAM ban đầu có xu hướng gộp luôn một phần yên xe và tay lái màu sẫm vào mask người do màu áo và vật tương đồng; tôi đã dùng công cụ Polygon chuyển chế độ chỉnh sửa điểm (edit points) để tỉa lại đường biên khít theo rìa vải áo và trả lại phần tay lái cho đối tượng `motorcycle`.
- Nếu không dùng gợi ý: Đã dùng kết hợp Polygon thủ công và chỉnh sửa biên sau gợi ý để đảm bảo ranh giới sắc nét giữa người và phương tiện.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `cp2_slice`, ảnh `000000017627.jpg`, cụm hai xe ô tô con (`car`) đỗ sát cạnh nhau ở phía lề đường bên phải.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: gộp-tách (gộp nhầm hai instance riêng biệt thành một mask).
- Bằng chứng tôi nhìn thấy: Hai xe ô tô cùng màu đỗ sát nhau, gợi ý ban đầu tạo ra một polygon bao trùm cả hai xe; khi phóng to (zoom 400%), nhìn thấy rõ khe hở hẹp và đường bóng đổ giữa đuôi xe phía trước và đầu xe phía sau.
- Quy tắc và hành động sửa: Quy tắc: Các đối tượng cùng class nếu là hai thực thể rời rạc thì bắt buộc phải tách thành hai instance riêng biệt; hành động: xóa mask gộp, dùng công cụ Polygon vẽ viền riêng cho xe thứ nhất men theo khe phân cách, sau đó vẽ instance mới cho xe thứ hai, Save task và export lại.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại thành công vào file `submissions/cp2_slice.zip`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Khi chạy script đánh giá trên dữ liệu Ground Truth của 3 tier:
- `easy_semantic`: Đạt mIoU **0.746** (15.4 / 20 điểm), trong đó lớp `road` đạt IoU rất cao 0.973, `sky` đạt 0.958, `building` đạt 0.796, độ phủ (coverage) đạt 88.0%.
- `hard_panoptic`: Đạt PQ **0.352** (10.1 / 30 điểm), chất lượng phân đoạn biên (SQ) đạt mức tốt 0.644 (các lớp xe tải truck SQ 0.823, bus SQ 0.893, road SQ 0.821, building SQ 0.665, sky SQ 0.933).
- `medium_instance`: Đạt Mean Matched IoU **0.700** trên các vật thể được gán nhãn, nhưng do thiếu các đối tượng nhỏ/ở xa (submitted 28 vs GT 71) khiến recall@0.5 đạt 0.197, dẫn đến metric tổng hợp (mean_matched_IoU x recall) đạt 0.138, dưới ngưỡng floor 0.40 nên nhận 0.0 / 32 điểm.
Tổng điểm 3 tier đạt **25.5 / 82 điểm**. Sáu checkpoint đều hoàn thành 100% (6/6 ảnh) và vượt qua kiểm tra cấu trúc `inspect_submissions.py` (tối đa 18 điểm chờ coach chấm). Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `cp4_curb` - ảnh `7d83710e-4697c3b2.jpg` (khu vực mép bó vỉa chuyển tiếp vỉa hè và lòng đường) | Gán dải bó vỉa bê tông vào `road` (vì màu sắc tương đồng với mặt đường) hay gán vào `sidewalk` (phần phân cách dành cho người đi bộ). | Phân định theo chức năng cấu trúc và gờ cao thay vì cảm quan màu sắc bề mặt; bó vỉa thuộc kết cấu mép vỉa hè. | Quyết định gán mép bó vỉa vào `sidewalk`. (Hỏi coach: trường hợp mép bó vỉa bị mài mòn phẳng ngang mặt đường thì ranh giới nên căn theo vạch sơn hay gờ nứt?) |
| 2. `cp1_holes` - ảnh `000000144300.jpg` (bánh xe và nan hoa của xe đạp `bicycle` lớn tiền cảnh) | Khoét rỗng từng khe giữa các nan hoa và khung xe để lộ nền, hay phủ kín toàn bộ bánh xe thành một mask liền mạch. | Quy tắc checkpoint 1 nêu rõ: "Holes: windows/gaps stay inside the mask — do NOT cut them out", chi tiết bên trong vật không được khoét lỗ tùy tiện. | Quyết định phủ liền toàn bộ diện tích bánh xe và nan hoa thành một mask `bicycle` hoàn chỉnh, không khoét thủng các khe nhỏ bên trong. |
| 3. `cp5_occlusion` - ảnh `000000336232.jpg` (chiếc ô tô `car` bị cột và cành cây che chắn ngang thân xe) | Tách thành hai instance `car` riêng biệt do hai phần pixel bị đứt đoạn, hay giữ chung một instance duy nhất. | Quy tắc instance segmentation: Một vật thể vật lý bị che khuất một phần vẫn chỉ là một instance duy nhất, không vẽ lấn qua vật che. | Quyết định tạo 1 instance duy nhất với dạng multi-polygon bao gồm 2 phần nhìn thấy tách biệt, không vẽ xuyên qua cột/cây che khuất. |

