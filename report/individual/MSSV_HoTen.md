# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Huỳnh Ngọc Gia Hân 
- MSSV: 2A202603128
- Email: huynh.han.ctc@gmail.com
- Lớp: L2A
- Link repo bài làm: https://github.com/suadaunanh/K5-L2-Day02-HuynhNgocGiaHan-2A202603128-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1IfwMQO2B7hbVH4cbla99VheNTB-PmKnP#scrollTo=pNpBtqZcJqFr

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

- Cách áp dụng schema: 4 lớp theo data.yaml (0 car, 1 truck, 2 bus, 3 van). Phân biệt chính: van là thân hộp liền khối, không có cabin tách khỏi thùng; truck có cabin tách riêng với thùng hàng; bus thân dài, nhiều hàng cửa sổ. Box ôm sát phần nhìn thấy, xe bị che/cắt khung vẫn gán nếu nhận ra lớp, không gán mô tô/xe máy, phản chiếu, bảng quảng cáo.
- Các ca mơ hồ đã quyết định:
drive_027 (x≈0.80, y≈0.49): xe nhỏ thân hộp, cửa lùa → van.
drive_009 (x≈0.29, y≈0.38): xe tải bưu chính thùng kín → truck (cabin tách khỏi thùng nên không phải van).
drive_009 (x≈0.67, y≈0.57): xe buýt thành phố đỏ trắng → bus.
drive_029 (x≈0.02, y≈0.79): vật thể tối màu sát mép trái, không nhận ra là xe → không gán.
- Phần cần giảng viên làm rõ: thứ tự lớp trong gold train và dataset/labels/eval có vẻ là car/van/truck/bus, khác với data.yaml (xem mục 4). Đã báo thầy và được thầy xác nhận nhãn chuẩn có sai sót.

## 3. Annotation và self-QC

- Số file nhãn train: 50/50 (dataset/labels/train/, gán bằng CVAT, export YOLO 1.1).
- Kết quả checker và lỗi đã xử lý: python3 tools/check_yolo_labels.py → ĐẠT cấu trúc: 50/50 file nhãn hợp lệ (cả bản khóa ban đầu và bản sau sửa). Lỗi đã xử lý: bản export CVAT nằm trong thư mục con obj_train_data/ kèm obj.data, obj.names, train.txt; chỉ chép 50 file .txt vào dataset/labels/train/. Lần chấm đầu ra F1 = 0 do dataset/labels/train/ còn trống, đã chép nhãn vào rồi chạy lại.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_008, drive_009, drive_010, drive_026, drive_027, drive_028, drive_029, drive_030, drive_041, drive_045.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Bỏ sót xe ở ảnh đông xe: vẽ thêm box (số box so với bản export CVAT trước khi sửa) ở drive_041 (11 → 19 box), drive_045 (12 → 19), drive_027 (22 → 28), drive_009 (19 → 22), drive_028 (10 → 13), drive_026 (34 → 36), drive_030 (24 → 26).
drive_029: sửa lại lớp các box, sau sửa khớp 17/19 xe trong gold, không còn box sai lớp.
Box của xe nhỏ ở xa còn rộng hơn xe (IoU 0.2–0.48 ở drive_045, drive_041) → bổ sung quy tắc "phóng to ảnh khi vẽ" vào guideline.
- Commit khóa nhãn ban đầu: 96b7326 — "Add YOLO labels and guideline" (09/11/2026 22:54).

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 376 | 413 |
| FP | 247 | 368 |
| FN | 277 | 240 |
| Precision | 0.6035 | 0.5288 |
| Recall | 0.5758 | 0.6325|
| F1 | 0.5893 | 0.5760 |
| mean_iou_of_matches | 0.8550 | 	0.8376 |
| passed | | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

Kết quả: outputs/annotation_evaluation_before.json (trước sửa) và outputs/annotation_evaluation.json (sau sửa).

Rework: sửa 10 ảnh liệt kê ở mục 3, chủ yếu vẽ thêm xe bị sót. Recall tăng 0.576 → 0.633 (FN giảm 277 → 240), nhưng FP tăng vì nhiều box mới vẽ cho xe nhỏ ở xa bị rộng hoặc lệch (IoU < 0.5), nên F1 gần như không đổi. Mean IoU vẫn đạt (> 0.70).

Lý do F1 thấp — nhãn gold lệch thứ tự lớp so với data.yaml: Khi đối chiếu các box trùng vị trí (IoU ≥ 0.5) nhưng khác lớp, lỗi lặp lại theo quy luật cố định: bus của tôi ↔ gold id 3 (42 box), van ↔ gold id 1 (18 box), truck ↔ gold id 2 (15 box). Kiểm tra bằng mắt cho thấy gold train và dataset/labels/eval có vẻ dùng thứ tự car/van/truck/bus, khác với data.yaml (car/truck/bus/van):

drive_027 (x≈0.80, y≈0.49): xe thân hộp nhỏ, cửa lùa (van) → gold ghi id 1 (truck).
drive_009 (x≈0.67, y≈0.57): xe buýt thành phố đỏ trắng → gold ghi id 3 (van).
Eval drive_052: xe buýt 2 tầng → labels/eval ghi id 3 (van).
Đã báo với 

Đối chiếu: nếu đọc gold theo thứ tự car/van/truck/bus, F1 bản trước sửa là 0.660 và bản sau sửa là 0.667 (thay vì 0.589 / 0.576). Như vậy một phần điểm thấp đến từ gold; phần còn lại là lỗi của tôi: bỏ sót xe và box thừa ở xe nhỏ phía xa (drive_041, drive_027, drive_024, drive_012, drive_050), box xe nhỏ còn rộng.

Quyết định: giữ nguyên lớp theo schema data.yaml, không đổi lớp để khớp gold và không sửa gold.
## 5. Kết quả YOLO

- Runtime: Google Colab, GPU Tesla T4 (15 GB), Ultralytics 8.4.208, Torch 2.14.1+cu130, Python 3.13.16.
- Các tham số thay đổi so với notebook nếu có và lý do: giữ nguyên notebook (yolo26n.pt, epochs=50, imgsz=640, batch=-1, seed=20260909). Colab clone repo ở commit ebb040b, tức train bằng bộ nhãn khóa ban đầu 96b7326 (539 car, 26 truck, 58 bus, 0 van).
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt 

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.888 |
| Recall | 0.306 |
| mAP50 | 0.318 |
| mAP50–95 | 0.211 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
