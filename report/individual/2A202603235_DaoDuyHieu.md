# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Đào Duy Hiếu
- MSSV:2A202603235
- Email: 26ai.hieudd2@vinuni.edu.vn
- Lớp: E403
- Link repo bài làm: https://github.com/HIEU2460/K5-L2-DAY02-DaoDuyHieu-2A202603235-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/github/HIEU2460/K5-L2-DAY02-DaoDuyHieu-2A202603235-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb#scrollTo=8teZRm2y00hQ

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). 

## 3. Annotation và self-QC

- Số file nhãn train: 50 file .txt tương ứng với 50 ảnh train (drive_001.jpg đến drive_050.jpg).

- Kết quả checker và lỗi đã xử lý: 
    + Chạy lệnh python tools/check_yolo_labels.py.
    + Kết quả: ĐẠT cấu trúc: 50/50 file nhãn hợp lệ. Chờ ground truth để chấm chất lượng.
    + Đã thực hiện Self-QC và chỉnh sửa các lỗi gán nhãn phát hiện tại ảnh drive_005.jpg, drive_035.jpg và drive_050.jpg.

- Danh sách ít nhất 10 ảnh đã rà bằng mắt: 
    drive_001.jpg
    drive_005.jpg
    drive_010.jpg
    drive_015.jpg
    drive_020.jpg
    drive_025.jpg
    drive_030.jpg
    drive_035.jpg
    drive_040.jpg
    drive_050.jpg
Tổng kết: Bổ sung 3 bounding box bị bỏ sót và sửa class cho 2 đối tượng.

- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
    + drive_005.jpg: Phát hiện 1 đối tượng gán nhầm class bus → đã sửa thành van. Đồng thời phát hiện bỏ sót 2 xe van → đã bổ sung 2 bounding box.
    + drive_035.jpg: Phát hiện bỏ sót 1 xe car (ID 930) → đã bổ sung bounding box cho xe này.
    + drive_050.jpg: Phát hiện đối tượng ID 162 gán nhầm class car → đã sửa thành van.
- Commit khóa nhãn ban đầu: 6e6ed4d (add ảnh đã labeling).

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |445 |433 |
| FP |485 |366 |
| FN |208|220|
| Precision |0.4785 |0.5419 |
| Recall |0.6815 | 0.6631|
| F1 | 0.5622|0.5964|
| mean_iou_of_matches |0.8474 |0.8495 |
| passed |false |false |

File đánh giá: [annotation_evaluation.json](../../outputs/annotation_evaluation.json)

Nhận xét: Qua hai lần rework, F1 tăng từ 0.5622 lên 0.5964, Precision tăng từ 0.4785 lên 0.5419, FP giảm từ 485 xuống 366. Mean IoU vẫn ở mức khoảng 0.85, vượt ngưỡng 0.70. Tuy nhiên, Recall giảm từ 0.6815 xuống 0.6631, TP giảm từ 445 xuống 433 và FN tăng từ 208 lên 220.

Quá trình rework: Sau Gold QC lần đầu, đã rà soát và cập nhật annotation trong CVAT. Một số thay đổi số lượng box được ghi nhận qua các lần đánh giá:

    + drive_001.jpg: Số box giảm từ 16 xuống 6, số  match giữ nguyên 5.

    + drive_002.jpg: Số box giảm từ 39 xuống 21, số match giữ nguyên 11.

    + drive_003.jpg: Số box giảm từ 41 xuống 24, số match giảm từ 17 xuống 15.

    + drive_006.jpg: Số box giảm từ 40 xuống 24, số match giảm từ 18 xuống 16.

    + drive_030.jpg: Số box giảm từ 42 xuống 19, số match giảm từ 14 xuống 10.

    + drive_041.jpg: Số box tăng từ 14 lên 24, số match tăng từ 6 lên 10.

    + drive_050.jpg: Số box giảm từ 33 xuống 22, số match giảm từ 16 xuống 10.
Kết quả cuối: F1 = 0.5964 và mean IoU = 0.8495

Quyết định sửa/giữ và lý do: Đã rà soát và chỉnh sửa các bounding box theo `GUIDELINE_MINI.md`:
- `drive_005.jpg`: Sửa class `bus` => `van`, bổ sung 2 xe `van` bị bỏ sót.
- `drive_035.jpg`: Bổ sung 1 xe `car` bị bỏ sót.
- `drive_050.jpg`: Sửa class `car` → `van`.
- Sau Gold QC, tiếp tục rà soát và điều chỉnh số lượng box ở một số ảnh, 1 số box có hình ảnh bị mờ => bỏ không gán nhãn cho box đó. Giữ các box hợp lệ, kiểm tra lại các box chưa phù hợp với guideline.

**Kết quả:** FP giảm từ 485 xuống 366, F1 tăng từ 0.5622 lên 0.5964, đáp ứng ngưỡng 0.50 theo thông báo của Lab Coach.

Lưu ý về tiêu chí đạt: Script vẫn trả về passed: false vì sử dụng ngưỡng F1 mặc định 0.80. Theo thông báo của Lab Coach, do vấn đề class ID trong Gold Labels, bài được chấp nhận với F1 từ 0.50. Kết quả hiện tại đáp ứng ngưỡng F1 điều chỉnh và ngưỡng mean IoU 0.70. Giữ nguyên JSON đánh giá, không chỉnh sửa Gold Labels.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
    Runtime: Google Colab, Python 3.13.16.
    GPU: Tesla T4 (CUDA:0).
    Ultralytics: 8.4.208.
    PyTorch: 2.14.1+cu130.

- Các tham số thay đổi so với notebook nếu có và lý do: Không thay đổi so với notebook gốc

- Đường dẫn best checkpoint trong phiên chạy:  /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.3156|
| Recall |0.3066|
| mAP50 |0.2740|
| mAP50–95 |0.2006|

Nhận xét: Mô hình nhận diện lớp car tốt nhất với Recall = 0.777 và mAP50 = 0.738. Trong ảnh drive_053.jpg, mô hình dự đoán 5 xe car, 3 xe bus và 1 xe van. Tuy nhiên, cần đối chiếu ảnh với nhãn thực tế để xác định dự đoán nào chính xác

**Minh chứng kết quả YOLO:**
- [Thư mục ảnh dự đoán eval](assets/eval_predictions/visuals/)
- [Thư mục kết quả YOLO26n](assets/yolo26_runs/)
- [Ảnh dự đoán drive_053.jpg](assets/eval_predictions/visuals/drive_053.jpg)
- [Biểu đồ kết quả huấn luyện](assets/yolo26_runs/vehicle_annotation/results.png)
- [Ma trận nhầm lẫn](assets/yolo26_runs/vehicle_annotation/confusion_matrix.png)

Nhận xét ảnh cụ thể: Trong ảnh drive_053.jpg, mô hình dự đoán xe bus màu xanh lá lớn ở phía dưới với confidence 0.85 và xe bus ở góc trên bên phải với confidence 0.84. Tuy nhiên, Recall của lớp bus trên tập eval bằng 0.000, cho thấy các dự đoán này chưa được tính là khớp đúng với nhãn ground truth theo tiêu chí đánh giá. Cần kiểm tra sự khác biệt về class ID hoặc bounding box giữa dự đoán và nhãn thực tế.

Trả lời bốn câu hỏi cuối notebook:
1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Trả lời: Lớp bus có Recall thấp nhất, bằng 0.000. Ảnh drive_053.jpg có các dự đoán bus, là ảnh cần kiểm tra để minh họa sự nhầm lẫn hoặc sai lệch trong nhận diện lớp này.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Trả lời: Cần kiểm tra vị trí, kích thước và độ bám sát vật thể của bounding box. Box có thể đạt IoU 0.50 nhưng không đủ chính xác ở các ngưỡng IoU cao hơn.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Trả lời: Các lỗi nhầm lẫn giữa car, van và bus có thể liên quan đến quy tắc phân lớp. Trong Self-QC đã phát hiện và sửa lỗi bus → van ở drive_005.jpg và car → van ở drive_050.jpg. Kết quả eval cho thấy Recall của bus = 0.000 và van = 0.250, cần tiếp tục kiểm tra tính nhất quán của nhãn.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Trả lời: Không dùng 10 ảnh eval để train nhằm tránh rò rỉ dữ liệu và bảo đảm đánh giá khách quan. Trong notebook, tập eval được dùng để kiểm tra khả năng nhận diện trên dữ liệu độc lập, tính Precision, Recall, mAP50 và mAP50–95 sau khi huấn luyện.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Annotation|Nhầm class car, van, bus|Rà soát và sửa nhãn trong CVAT |Đã sửa các lỗi phát hiện|
| Self-QC|Bỏ sót bounding box|Kiểm tra lại 10 ảnh|Đã bổ sung 3 box|

## 7. Tự đánh giá theo rubric
| Tiêu chí | Tự đánh giá |
|---|---|
| Hoàn thành và định dạng YOLO (10đ) | Đã tạo đủ 50 file nhãn, checker đạt 50/50. |
| Đầy đủ vật thể (20đ) | Đã gán nhãn 50 ảnh và bổ sung các vật thể bỏ sót khi Self-QC. Chưa xác nhận độ đầy đủ tuyệt đối. |
| Độ bám sát và hình học box (20đ) | Mean IoU đạt 0.8495 trên các box khớp Gold. |
| Phân lớp nhất quán (15đ) | Đã rà soát và sửa lỗi nhầm class `car`, `van`, `bus`. |
| Mini guideline (10đ) | Đã xây dựng `GUIDELINE_MINI.md`; cần bảo đảm đủ 3 trường hợp mơ hồ có lý do. |
| Gold-set annotation QC (15đ) | Đã thực hiện 2 vòng rework, F1 tăng từ 0.5622 lên 0.5964, Mean IoU = 0.8495. Script vẫn báo `passed: false` theo ngưỡng 0.80; Lab Coach thông báo chấp nhận F1 ≥ 0.50 do lỗi class ID của Gold Labels. |
| Pipeline YOLO và phân tích (10đ) |Đã chạy thành công YOLO26n trên Google Colab (GPU Tesla T4), đánh giá 10 ảnh eval. Đã xuất ảnh dự đoán, lưu kết quả huấn luyện và phân tích ảnh `drive_053.jpg`.|

**Tổng kết:** Đã hoàn thành annotation, Self-QC và Gold QC theo ngưỡng điều chỉnh của Lab Coach.

## 8. Công cụ AI và tài liệu tham khảo
- **ChatGPT:** Hỗ trợ giải thích YOLO, CVAT và trình bày báo cáo.
- **CVAT:** Sử dụng để gán nhãn và chỉnh sửa bounding box.
- **Python:** Chạy công cụ kiểm tra nhãn và đánh giá Gold QC.
- **Tự kiểm tra:** Trực tiếp rà soát ảnh, sửa nhãn và chạy lại các lệnh kiểm tra.
**Tài liệu tham khảo:** `GUIDELINE_MINI.md`, `SUBMISSION.md`, `RUBRIC.md` và Read me thực hành của bài Lab.
