# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.


- Mã học viên theo lớp: L2A
- Ngày / CVAT local: 11-12/11/2026
- Công cụ đã dùng: CVAT

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | … | … / 1 | 3 |
| cp5_occlusion | … | … / 1 | 3 |
| cp3_thin | … | … / 1 | 3 |
| cp4_curb | … | … / 1 | 3 |
| cp6_coverage | … | … / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: …000000181542.jpg, đối tượng `person` đứng/ngồi ở cụm người bên trái ảnh 
- Class và quy tắc tôi dùng để chọn biên: Class `person`. Quy tắc biên là bám sát đường viền cơ thể, mũ và quần áo của từng người; dừng biên ngay tại ranh giới tiếp xúc với xe máy bên dưới và ranh giới bị vật thể phía trước che khuất, không gộp yên xe, tay lái hay bánh xe vào

- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: không dùng,  toàn bộ mask trong `medium_instance` được vẽ thủ công. Quyết định gán nhãn quan trọng là tách riêng từng  `person` độc lập với nhau và tách rời khỏi `motorcycle` kể cả khi chân người đè lên thân xe, khoét bỏ các khoảng trống nền giữa tay chân và thân người để mask phản ánh đúng  thực tế

- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình. 

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `hard_panoptic`  ảnh `000000350023.jpg` vùng cột đèn giao thông và người đi bộ nằm đè trước tán cây `vegetation`
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: phủ vùng, thứ tự lớp`z_order
- Bằng chứng tôi nhìn thấy: khi vẽ xong các lớp nền lớn (`sky`, `building`, `vegetation`) với `layer` mặc định, một số đối tượng nhỏ phía trước như đèn giao thông (`traffic light`) và người (`person`) bị lớp `vegetation` hoặc `building` che khuất một phần khi hiển thị chồng lớp
- Quy tắc và hành động sửa: hạ `sky` xuống `layer = -1`, giữ `road`/`sidewalk` ở `layer = 0`, `building` ở `layer = 1`, `vegetation` ở `layer = 2`, và nâng toàn bộ `person`, `traffic light` lên `layer = 3`
- Sau sửa đã Save và export lại chưa? Đã Save trên CVAT và export lại file `hard_panoptic.zip`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): … / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| medium_instance` - `000000181542.jpg`, cụm người và xe máy ở giữa bên trái  | Gộp người lái và xe máy thành một đối tượng chung vì hai phần đan xen sát nhau; Tách riêng mask `person` và mask `motorcycle` | trong bộ nhãn có hai lớp độc lập là `person` và `motorcycle` | quyết định tách riêng `person` và `motorcycle` |
| 2 | hard_panoptic` — `000000350023.jpg`, vùng tán cây `vegetation` bên phải có 2 chiếc đèn phát sáng | Tô đặc toàn bộ tán cây thành một mảng `vegetation` liền; Khoét lỗ các khe hở giữa cành lá để trống phần đèn || Tô đặc toàn bộ tán cây |
| 3 |`hard_panoptic` — `000000460147.jpg`, dải mặt đường nhỏ ở ngã rẽ phía xa |Gán nhãn `road` riêng cho dải mặt đường này/ Gộp vào `building` hoặc `sidewalk` do diện tích nhỏ và bị che khuất một phần | Quan sát thấy bề mặt nhựa đường nối tiếp tuyến phố ở xa, ngay cạnh có `car`  và `motorcycle` đang lưu thông. | gán nhãn `road` cho vùng này để giữ đúng lòng đường |

