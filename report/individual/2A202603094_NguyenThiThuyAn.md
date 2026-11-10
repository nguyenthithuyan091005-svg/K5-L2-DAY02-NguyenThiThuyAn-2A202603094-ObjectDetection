# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Thị Thúy An
- MSSV: 2A202603094
- Email: nguyenthithuyan091005@gmail.com
- Lớp: L2A
- Link repo bài làm: https://github.com/nguyenthithuyan091005-svg/K5-L2-DAY02-NguyenThiThuyAn-2A202603094-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/drive/10ASSqk7MFv6kKK_ULhb2D50Zru7DJhPl?usp=sharing
## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: ĐẠT cấu trúc: 50/50 file nhãn hợp lệ
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:  `drive_001`, `drive_005`, `drive_009`, `drive_015`, `drive_026`, `drive_028`, `drive_031`, `drive_041`, `drive_044`, `drive_046`.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Bổ sung ba xe còn nhận diện được ở xa trong `drive_001`; bỏ bốn box của các xe quá nhỏ, khó xác định lớp trong `drive_046`. Xe trắng trong `drive_005` được tôi quyết định gán `van` theo mini guideline; trường hợp này còn khác gold và cần hỏi giảng viên.
- Commit khóa nhãn ban đầu: "Update dataset"

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |435|440|
| FP | 265|248|
| FN | 218|213|
| Precision | 0.6214|0.6395|
| Recall | 0.6662|0.6738|
| F1 | 0.6430|0.6562|
| mean_iou_of_matches | 0.8547|0.852|
| passed | pass|pass|

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.  
Sau khi xem lại ảnh và nhãn, tôi bổ sung ba xe còn nhận diện được ở xa bên phải drive_001 và giữ mỗi xe một box riêng. Với drive_046, tôi bỏ bốn box ở vùng xa vì các xe quá nhỏ để xác định lớp một cách chắc chắn; tôi giữ ba box của những xe nhìn rõ. Kết quả gold QC của toàn bộ bộ nhãn cho thấy F1 tăng từ 0,6430 lên 0,6562 nhưng vẫn chưa đạt ngưỡng 0,80. Tôi sẽ rà tiếp từng ảnh trước khi quyết định sửa hoặc giữ các nhãn còn khác gold.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, NVIDIA Tesla T4, 

Runtime: Google Colab.

GPU: NVIDIA Tesla T4, dung lượng VRAM 14.913 MiB (~14.9 GB).

Device: CUDA:0 (GPU).

Python: 3.13.16.

Ultralytics: 8.4.208.

PyTorch (Torch): 2.14.1+cu130.

- Các tham số thay đổi so với notebook nếu có và lý do: không có
- Đường dẫn best checkpoint trong phiên chạy: outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.4203 |
| Recall | 0.3028|
| mAP50 | 0.3669|
| mAP50–95 |0.2407 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
- Hai lớp có recall thấp nhất là Truck và Van, cùng đạt 0%. Điều này cho thấy mô hình chưa nhận diện chính xác được các đối tượng thuộc hai lớp này.
- Ảnh minh họa cho van: drive_057.jpg hoặc drive_059.jpg. Cả hai ảnh chỉ dự đoán “10 cars, 1 van” và “6 cars, 1 van”, tức mô hình rất ít khi gọi đúng “van”. Phần lớn van thật bị bỏ sót hoặc bị gọi thành car.
- Ảnh minh họa cho truck: drive_058.jpg. Mô hình dự đoán “4 trucks, 2 buses”, nhưng điểm P/R của truck vẫn bằng 0. Nghĩa là các box truck dự đoán không khớp box ground-truth (sai vị trí hoặc sai lớp, ví dụ gọi nhầm bus/truck).
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
- Nếu mAP50 cao nhưng mAP50-95 thấp, điều đó cho thấy mô hình có thể nhận diện đúng đối tượng ở ngưỡng IoU dễ hơn, nhưng vị trí và kích thước bounding box chưa đủ chính xác khi yêu cầu khắt khe hơn. 
- Các vấn đề thường gặp: 
    - Box quá rộng, chứa nhiều vùng nền
    - Box quá hẹp, không bao phủ hết đối tượng.
    - Box bị lệch khỏi vị trí thực tế.
    - Box không ôm sát phần thân xe.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?

Có khả năng lỗi nằm ở cách quy định và phân biệt các lớp phương tiện trong guideline, đặc biệt là van, truck và bus.
Dữ liệu cho thấy:
    - Lớp van có 12 đối tượng nhưng Recall = 0, nghĩa là mô hình không phát hiện đúng đối tượng nào thuộc lớp này.
    - Lớp truck có 5 đối tượng nhưng Recall = 0, cho thấy mô hình cũng gặp khó khăn tương tự.
    - Ở ảnh drive_053 và drive_058, mô hình dự đoán nhiều xe là truck hoặc bus, cho thấy có sự nhầm lẫn giữa các lớp này
       
-> Nguyên nhân có thể là: Một số loại xe có hình dáng khá giống nhau nên khó phân biệt. Ví dụ: Xe bán tải có thể bị nhầm giữa car và truck, Xe minivan có thể bị nhầm giữa car và van, xe tải nhỏ và xe buýt mini có thể bị nhầm giữa truck và bus. Nếu guideline không hướng dẫn rõ cách phân loại những trường hợp này, người gán nhãn có thể đưa cùng một loại xe vào những lớp khác nhau. Điều này khiến dữ liệu huấn luyện thiếu nhất quán và mô hình khó học cách phân biệt chính xác.

4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

Không sử dụng 10 ảnh eval để train nhằm tránh Data Leakage và đảm bảo tính khách quan của quá trình đánh giá. Tập eval đóng vai trò dữ liệu chưa được sử dụng để huấn luyện, giúp kiểm tra khả năng tổng quát hóa của mô hình trên dữ liệu mới. Nếu sử dụng chúng để train, kết quả đánh giá có thể bị sai lệch và không phản ánh chính xác hiệu suất thực tế của mô hình.
## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
Gold QC|F1 hiện tại 0,6562, thấp hơn ngưỡng 0,80 |So sánh và rà lại các ảnh có nhiều box chưa ghép hoặc gán thừa |F1 có cải thiện nhưng chưa đạt; cần tiếp tục kiểm tra trực quan và rework |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

Đối chiếu [RUBRIC.md](../../RUBRIC.md), tôi đã có 50 file nhãn train đúng cấu trúc YOLO; công cụ kiểm tra báo đạt 50/50 file. `GUIDELINE_MINI.md` đã nêu định nghĩa bốn lớp, quy tắc vẽ box và ba ca mơ hồ kèm quyết định, lý do. Repo cũng có JSON đánh giá annotation, notebook đã chạy, 10 ảnh dự đoán trên tập eval và các file kết quả huấn luyện trong `report/individual/assets/`.

Phần gold QC chưa đạt yêu cầu: F1 sau rework là 0,6562, thấp hơn ngưỡng 0,80; mean IoU của các box ghép được là 0,8520. Tôi cần tiếp tục rà các ảnh có nhiều FP/FN và báo giảng viên về những trường hợp phân lớp còn mơ hồ. 

## 8. Công cụ AI và tài liệu tham khảo

Tôi sử dụng Codex (ChatGPT) để góp ý cách diễn đạt báo cáo và rà soát sự nhất quán giữa báo cáo, `outputs/annotation_evaluation.json`, notebook đã chạy và các file trong repo. Tôi tự quyết định nhãn và các ca mơ hồ trong `GUIDELINE_MINI.md`; tự kiểm tra lại số liệu, ảnh minh họa và nội dung báo cáo trước khi nộp. Tôi không sử dụng AI để tạo nhãn thay cho mình.

Tài liệu tham khảo: `README.md`, `GUIDE.md`, `RULES.md`, `RUBRIC.md` và `SUBMISSION.md` của repo bài học. Không sử dụng nguồn ngoài repo.

