# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Phạm Hoàng Minh
- MSSV: 2A202603064
- Email: 26ai.minhph@vinuni.edu.vn
- Lớp: K5-L2-2A
- Link repo bài làm: https://github.com/Adendz87/K5-L2-Day02-PhamHoangMinh-2A202603064-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1wg0Ut3lXfvqdEParTLH0cnAhhq8PgCt6

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

Schema nhãn YOLO

Mỗi đối tượng được biểu diễn bằng một dòng trong file .txt, gồm 5 giá trị:

class_id: ID của lớp đối tượng.

x_center: Tọa độ tâm bounding box theo chiều ngang.

y_center: Tọa độ tâm bounding box theo chiều dọc.

width: Chiều rộng bounding box.

height: Chiều cao bounding box.

Các tọa độ và kích thước bounding box được chuẩn hóa về khoảng từ 0 đến 1 theo kích thước ảnh. Mỗi ảnh có một file nhãn tương ứng, tên file nhãn trùng với tên ảnh.

Ba trường hợp dễ nhầm lẫn khi gán nhãn

1. Truck và van: Cần phân biệt dựa trên hình dáng, kích thước và đặc điểm phần thân xe. Không chỉ dựa vào kích thước tương đối trong ảnh.

2. Bus và truck: Cần chú ý đặc điểm thân xe, khoang hành khách và cấu trúc phương tiện để tránh nhầm lẫn giữa hai lớp.

3. Van và các phương tiện khác: Cần xem xét hình dáng tổng thể và đặc điểm phương tiện trước khi quyết định class.

Cách áp dụng schema
- Xác định class của từng đối tượng trước khi tạo bounding box.

- Bounding box cần bao quanh đối tượng, hạn chế chứa quá nhiều phần nền.

- Kiểm tra tọa độ và kích thước box sau khi gán nhãn.

- Sử dụng cùng một quy tắc phân loại cho các đối tượng tương tự trong toàn bộ tập dữ liệu.

- Kiểm tra lại các trường hợp không chắc chắn bằng cách đối chiếu guideline.

Nội dung cần giảng viên làm rõ

- Tiêu chí phân biệt cụ thể giữa truck và van trong những trường hợp hình ảnh khó quan sát.

- Cách xử lý đối tượng bị che khuất một phần.

- Quy tắc xác định ranh giới bounding box đối với phương tiện có hình dáng không rõ ràng.


## 3. Annotation và self-QC

- Số file nhãn train: 50 file .txt, tương ứng với 50 ảnh train.
- Kết quả checker và lỗi đã xử lý: Checker xác nhận 50/50 file nhãn hợp lệ, không phát hiện lỗi cấu trúc YOLO.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_051.jpg, drive_053.jpg, drive_054.jpg, drive_056.jpg và các ảnh khác đã kiểm tra thực tế.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
- Commit khóa nhãn ban đầu:

-Số file nhãn train: 50 file .txt tương ứng với 50 ảnh train.

-Kết quả checker và lỗi đã xử lý: [CẦN ĐIỀN kết quả checker thực tế, số lỗi phát hiện và số lỗi đã sửa].

- Ảnh `drive_054.jpg`: Phát hiện một đối tượng car bị gán nhầm class thành van. Đã kiểm tra lại và điều chỉnh class thành car.
- Ảnh `drive_056.jpg`: Phát hiện một đối tượng car bị gán nhầm class thành van. Đã kiểm tra lại và điều chỉnh class thành car.
- Ảnh `drive_051.jpg`: Phát hiện một đối tượng car bị gán nhầm class thành van. Đã kiểm tra lại và điều chỉnh class thành car.
- `drive_056.jpg`: Có 1 đối tượng xe nhưng bị vẽ 2 bounding box chồng lên nhau, một box mang class car, box còn lại mang class van.
- Ảnh `drive_053.jpg`: Phát hiện 1 đối tượng bus có 2 bounding box chồng lên nhau, lần lượt mang class bus và van. Cần loại bỏ box trùng và giữ lại class chính xác là bus.
- `drive_060.jpg`: Có xe chưa được mô hình phát hiện (bỏ sót đối tượng).
- `drive_058.jpg`: Mô hình chưa phát hiện hết các xe xuất hiện trong ảnh.
- `drive_054.jpg`: Một xe bị che khuất gần hết nhưng được gán nhãn/dự đoán là truck.


Commit khóa nhãn ban đầu: 8758ab5

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | | | : 457
| FP | | | : 387
| FN | | | : 196
| Precision | | | : 0.5415
| Recall | | | : 0.6998
| F1 | | | : 0.6106
| mean_iou_of_matches | | : 0.8580
| passed | | | : true

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, GPU NVIDIA Tesla T4, Python 3.13.16, Ultralytics 8.4.208, PyTorch 2.14.1+cu130.
- Các tham số thay đổi so với notebook nếu có và lý do: Không thay đổi. Sử dụng YOLO26n, epochs=50, imgsz=640, batch=-1, device=0.
- Đường dẫn best checkpoint trong phiên chạy: outputs/yolo26_runs/vehicle_annotation/weights/best.pt.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | | : 0.319
| Recall | |    : 0.307
| mAP50 | |     : 0.274
| mAP50–95 | |  : 0.201

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?

Lớp bus có Recall thấp nhất, bằng 0.000. Trong tập eval có 2 đối tượng bus nhưng mô hình không nhận diện đúng được đối tượng nào. Ảnh minh họa là ảnh eval có chứa bus trong thư mục assets/eval_predictions/visuals/

2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?

   Cần kiểm tra vị trí tâm, chiều rộng, chiều cao và độ bao sát của bounding box. Điều này cho thấy mô hình có thể phát hiện đúng đối tượng ở ngưỡng IoU 0.5 nhưng box chưa đủ chính xác khi đánh giá ở các ngưỡng IoU cao hơn.

3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?

Các lớp cần chú ý là truck, bus và van vì kết quả nhận diện còn thấp. Cụ thể, truck có Recall 0.200, bus có Recall 0.000 và van có Recall 0.250. Có thể xảy ra nhầm lẫn giữa các lớp có đặc điểm hình dáng tương tự. Tuy nhiên, cần đối chiếu ảnh dự đoán với nhãn gốc và GUIDELINE_MINI.md để xác định lỗi có thực sự liên quan đến guideline hay không.

4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

Không sử dụng 10 ảnh eval để train nhằm tránh mô hình học trực tiếp từ dữ liệu đánh giá, gây sai lệch kết quả. Trong notebook, 10 ảnh eval được sử dụng làm validation để đánh giá mô hình qua các epoch và hỗ trợ lựa chọn checkpoint best.pt. Vì vậy, tập eval trong bài thực hành này đồng thời đóng vai trò validation, không phải một test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

Khó khăn 1: Gán nhãn dữ liệu
- Lỗi quan sát: Khó phân biệt một số đối tượng có hình dáng tương tự như truck, bus và van.
- Cách đã thử: Tham khảo định nghĩa class và xây dựng guideline gán nhãn.
- Kết quả/còn thiếu: Đã hoàn thành gán nhãn 50 ảnh train, nhưng kết quả đối chiếu gold-set chưa đạt ngưỡng ban đầu.
Khó khăn 2: Đánh giá nhãn
- Lỗi quan sát: F1 đạt 0.6106, thấp hơn ngưỡng 0.8 ban đầu.
- Cách đã thử: Kiểm tra Precision, Recall, IoU và thử điều chỉnh nhãn.
- Kết quả/còn thiếu: Kết quả tốt nhất F1 = 0.6106, mean IoU = 0.8580.
Khó khăn 3: Huấn luyện YOLO26
- Lỗi quan sát: Kết quả nhận diện một số class còn thấp, đặc biệt là bus và van.
- Cách đã thử: Sử dụng YOLO26n, huấn luyện 50 epochs trên Google Colab với GPU Tesla T4.
- Kết quả/còn thiếu: Train hoàn tất nhưng mAP50 chỉ đạt 0.274, cần tiếp tục kiểm tra ảnh dự đoán để xác định nguyên nhân.
Khó khăn 4: Đánh giá trực quan
- Lỗi quan sát: Chưa xác định chính xác nguyên nhân mô hình không nhận diện đúng bus.
- Cách đã thử: Kiểm tra metrics và đối chiếu ảnh dự đoán với nhãn gốc.
- Kết quả/còn thiếu: Cần bổ sung phân tích cụ thể trên ảnh eval.

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

- Annotation: Đã hoàn thành gán nhãn 50 ảnh train và có 50/50 file nhãn đạt kiểm tra cấu trúc.
- Self-QC: Đã rà soát một số ảnh, phát hiện các trường hợp nhầm class và bounding box trùng lặp. Cần bổ sung danh sách đủ 10 ảnh đã kiểm tra và xác nhận các lỗi đã được xử lý.
- Gold QC: Kết quả tốt nhất đạt Precision 0.5415, Recall 0.6998, F1 0.6106 và mean IoU 0.8580. Chưa đạt ngưỡng F1 0.8 trong kết quả đánh giá gốc.
- YOLO training: Đã hoàn thành huấn luyện YOLO26n trong 50 epochs trên Google Colab với GPU Tesla T4.
- Model evaluation: Đã đánh giá trên 10 ảnh eval. Kết quả mAP50 đạt 0.274 và mAP50-95 đạt 0.201. Cần bổ sung phân tích trực quan một ảnh cụ thể.
- Báo cáo và minh chứng: Đã có notebook, checkpoint và thư mục ảnh dự đoán. Cần kiểm tra lại đường dẫn và bổ sung đầy đủ minh chứng trước khi nộp.

## 8. Công cụ AI và tài liệu tham khảo

- ChatGPT: Hỗ trợ giải thích quy trình gán nhãn YOLO, hướng dẫn chạy Checker, đọc kết quả Gold QC, phân tích metrics và rà soát nội dung báo cáo. Các kết quả và thông tin kỹ thuật được đối chiếu lại với dữ liệu thực tế trong repo và Colab.

- Notebook `notebooks/day2_annotation_to_yolo.ipynb` trong repo bài tập.
- `GUIDELINE_MINI.md` và `RUBRIC.md` trong repo bài tập.
- Tài liệu Ultralytics: https://docs.ultralytics.com/
- Google Colab: https://colab.research.google.com/
