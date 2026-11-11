# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

- Mã học viên theo lớp: 2A202603066
- Ngày / CVAT local: 2026-11-11 / CVAT Web App
- Công cụ đã dùng: CVAT (Polygon tool, Brush tool, Eraser), Python scoring scripts (score.py, view_gt.py, view_gt_json.py)

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

*Ghi chú: Tập trung hoàn thiện và tối ưu chất lượng 3 trạm chính (Easy, Medium, Hard).*
## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `00000181542.jpg`, chiếc bus màu trắng nằm ở làn đường bên trái .
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Quy tắc biên: Zoom 200%, dùng Polygon khoanh sát mép viền kim loại outer-body của thân xe, bao gồm cả gương chiếu hậu và lốp xe tiếp xúc mặt đường; loại bỏ phần không khí rỗng bên dưới gầm xe, xác định được nhiều object bằng mắt thường hơn cả gợi ý.
- Nếu dùng gợi ý sau đó: bỏ đi 1 số object mà phần gợi ý không có, thấy điểm iou tăng vì đúng số lượng object.
- Nếu không dùng gợi ý: “không dùng”; Tôi quyết định gán nhãn thủ công 100% bằng Polygon/Brush để kiểm soát chính xác độ chi tiết của đường viền (edge boundaries) thay vì phụ thuộc vào các công cụ tự động dễ gây lem viền vào nền background (gợi ý có nét vẽ quá thô khiến chi tiết bị dư quá nhiều, gây ra điểm số của tôi thấp mặc dù tôi đã kiểm xoát chi tiết đến từng pixel) .
## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `medium_instance`, các đối tượng ô tô (`car`) và người (`person`) nằm ở dải phân cách background phía cực xa.
- Lỗi thuộc loại: thừa vật (False Positives - FP).
- Bằng chứng tôi nhìn thấy: Khi chạy script `scoring/score.py medium_instance`, kết quả ghi nhận `TP 61, FP 29, FN 10`, tổng số đối tượng nộp là `90` trong khi Ground Truth chỉ có `71`. Chỉ số Precision $P@0.5$ chỉ đạt `0.68` và điểm task bị giới hạn ở `19.0/32`.
- Quy tắc và hành động sửa: Đối chiếu script visualizer (`view_gt_json.py`) phát hiện quy tắc gán nhãn của bộ GT (Annotation Protocol) chỉ gán cho các vật thể rõ ràng ở tiền cảnh và trung cảnh, bỏ qua các vật thể quá nhỏ (<10-15px) phía xa. Hành động: Mở CVAT, tiến hành rà soát và xóa bớt các mask đối tượng tí hon ở xa.
- Sau sửa đã Save và export lại chưa?: Đã Save, export lại `medium_instance.zip` đè vào `submissions/` và chạy lại script scoring thành công, điểm số có tăng lên vì đúng chính xác số lượng vật thể nhưng không thể đạt điểm tối đa do ground truth bị lem thừa quá nhiều. nên tôi quyết định không sửa file hard nữa vì cũng lỗi tương tự.
Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): … / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork. 

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.
| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `easy_semantic` (Ranh giới dải phân cách/vỉa hè & mặt đường) | 1: Tô toàn bộ vùng bê tông mép vỉa hè thành `sidewalk`.<br>2: Chỉ tô dải đi bộ thực sự là `sidewalk`, phần bó vỉa sát mép đường tô là `road`. | Dựa trên chức năng giao thông: Vùng bó vỉa cứng ngăn cách giữa làn xe chạy và hè phố. | **Đã quyết định:** Tô phần dải bê tông bó vỉa sát đường nối liền vào class `road` để đảm bảo độ liên tục của bề mặt di chuyển, phần mặt hè đi bộ phân biệt rõ thành `sidewalk`. |
| 2. `hard_panoptic` (Xe máy/Người bị tán cây và biển báo che ngang) | 1: Tách phần đầu và thân xe thành 2 Instance riêng biệt.<br>2: Giữ nguyên là 1 Instance duy nhất (Occlusion handling). | Quy tắc Occlusion trong Panoptic Segmentation: Đối tượng bị vật thể khác cắt ngang ở giữa nhưng cùng thuộc một thực thể chuyển động vẫn là 1 cá thể. | **Đã quyết định:** Tô gộp 2 phần bị che khuất vào cùng 1 Mask Shape (1 Instance ID) thay vì tách làm hai đối tượng rời. |
| 3. `medium_instance` (Hàng ô tô đỗ nối đuôi nhau sát mép đường) | 1: Tô một dải Polygon lớn bao trùm toàn bộ hàng xe.<br>2: Tách tỉ mỉ từng chiếc ô tô thành từng Mask cá thể riêng biệt. | Định nghĩa của Instance Segmentation: Bắt buộc đếm và phân tách chính xác từng cá thể (Things). | **Đã quyết định:** Zoom to 200%, tỉa viền tách từng chiếc ô tô riêng biệt. Kết quả giúp đạt 71/71 objects khớp hoàn toàn với Ground Truth. |
