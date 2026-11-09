# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Xe ô tô cá nhân, gia đình và phương tiện hạng nhẹ chở người/hàng nhẹ. | Xe sedan, SUV, hatchback, taxi, xe bán tải|
| `truck` |Phương tiện chuyên dụng dùng để vận chuyển hàng hóa nặng hoặc kéo rơ-móoc. |Xe tải chở hàng các loại, xe đầu kéo. |
| `bus` |Phương tiện giao thông cỡ lớn chuyên chở nhiều hành khách. |Xe buýt đô thị, xe khách liên tỉnh cỡ lớn, minibus chở khách.    |
| `van` |Phương tiện thân liền khối dạng hình hộp, chuyên chở hàng hóa dung tích trung bình hoặc chở khách gia đình. |Xe van, minivan |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Khung chữ nhật phải ôm khít phần viền ngoài cùng của xe (từ mép gương chiếu hậu, lốp xe tiếp đất cho đến đỉnh mui xe) không để dư thừa khoảng trống quá lớn.
- Vật thể bị che khuất:
  + Che một phần: Vẫn tiến hành vẽ box bao trọn phần hình dáng của xe (ước lượng phần bị khuất dựa trên phần hiện diện nếu có thể xác định rõ loại xe)
  + Che khuất quá nhiều: Nếu xe bị che khuất quá nặng (thường trên 70-80%) dẫn đến việc không thể xác định chính xác thuộc lớp nào, có thể cân nhắc bỏ qua không gán nhãn.
- Vật thể bị cắt bởi rìa ảnh: Vẫn tiến hành gán nhãn; cạnh của bounding box sẽ sát đúng vào viền biên của bức ảnh tại phần bị cắt, các phần còn lại vẽ ôm sát thân xe hiển thị.
- Vật thể rất nhỏ/mơ hồ: ghi lại hoặc bỏ qua không gán nhãn để tránh làm nhiễu dữ liệu huấn luyện

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_...` | | | |
| `drive_...` | | | |
| `drive_...` | | | |
