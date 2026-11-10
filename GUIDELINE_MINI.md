# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con chở người, thường 4 đến 7 chỗ, thân thấp, không có thùng hàng | Sedan, hatchback, SUV, MPV (Vios, Camry, CX-5, Xpander). Phân biệt: thấp hơn van, không có thùng sau như truck |
| `truck` | Xe tải chuyên chở hàng, có thùng hàng hoặc thùng trần phía sau, cabin tách riêng với thùng | Xe tải nhỏ, xe ben, container, bán tải (Ford Ranger). Phân biệt: nhìn thấy rõ thùng hàng, dáng dài và cao hơn car |
| `bus` | Xe chở khách cỡ lớn, từ khoảng 17 chỗ trở lên, thân dài, nhiều cửa sổ hai bên | Xe buýt thành phố, xe khách đường dài. Phân biệt: dài và cao nhất, nhiều hàng cửa sổ, thường có cửa lên xuống riêng |
| `van` | Xe thân hộp liền khối, cỡ giữa car và bus, chở người (khoảng 9 đến 16 chỗ) hoặc hàng nhẹ | Ford Transit, Hyundai Solati, Toyota Hiace, xe giao hàng thùng kín. Phân biệt: dáng hộp vuông, cao hơn car, ngắn hơn bus, không có cabin tách khỏi thùng như truck |

## Quy tắc chung 

- Box bám sát vật thể nghĩa là: bốn cạnh box chạm sát mép ngoài cùng của phần thân xe nhìn thấy được (kể cả gương, cản trước/sau); không lấy thêm mặt đường, bóng xe, trời hay xe bên cạnh. Mỗi xe một box riêng, không gộp nhiều xe vào một box. Với xe nhỏ ở xa, phóng to ảnh khi vẽ để box không bị rộng hơn xe.
- Vật thể bị che khuất: vẫn gán nếu phần nhìn thấy đủ để nhận ra loại xe; box chỉ bao phần nhìn thấy được, không đoán phần bị che. Hai xe chồng lên nhau thì vẽ hai box riêng trên phần nhìn thấy của từng xe.
- Vật thể bị cắt bởi rìa ảnh: vẫn gán nếu nhận diện được lớp; cạnh box chạm đúng rìa ảnh, không kéo ra ngoài và không đoán phần nằm ngoài khung.
- Vật thể rất nhỏ/mơ hồ: chỉ gán khi nhìn rõ đó là xe và xác định chắc được lớp; nếu chỉ là vài pixel hoặc một mảnh xe không rõ loại thì không gán và không tự đoán lớp, ghi case ID vào bảng bên dưới để hỏi giảng viên. Không gán phản chiếu trong kính/gương, hình xe trên bảng quảng cáo, mô tô/xe máy, xe đạp.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_027` | Xe nhỏ thân hộp liền khối, cửa lùa (x≈0.80, y≈0.49) | van | Dáng hộp, không có thùng hàng tách rời như truck |
| `drive_009` | Xe tải bưu chính thùng kín màu xanh (x≈0.29, y≈0.38) | truck | Cabin tách riêng với thùng, nên không phải van |
| `drive_009` | Xe buýt thành phố đỏ trắng (x≈0.67, y≈0.57) | bus | Thân dài, nhiều hàng cửa sổ |
| `drive_029` | Vật thể tối màu sát mép trái, cạnh hàng rào (x≈0.02, y≈0.79) | Không gán |Bị cắt ở mép ảnh, không nhận ra được là xe |
