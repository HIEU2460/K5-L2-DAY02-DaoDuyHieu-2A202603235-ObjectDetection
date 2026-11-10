# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Xe con dùng để chở người, bao gồm sedan, SUV, taxi và xe bán tải.|Sedan, SUV, pickup; không phải xe tải chở hàng lớn.|
| `truck` |Xe tải chuyên chở hàng hóa hoặc xe đầu kéo.|Xe tải thùng, xe tải lớn, xe container.|
| `bus` |Xe buýt hoặc minibus phục vụ chở hành khách|Xe buýt đô thị, minibus chở khách; phân biệt với van|
| `van` |Xe van hoặc minivan dùng để chở người hoặc hàng hóa|Xe van giao hàng, minivan; không phải xe buýt lớn hay xe con thông thường|

## Quy tắc chung

- Box bám sát vật thể nghĩa là: là hình chữ nhật bao quanh phần thân xe nhìn thấy được, với các cạnh càng sát xe càng tốt. Không lấy thêm nhiều nền đường, xe bên cạnh hoặc các vật thể khác.
- Vật thể bị che khuất: Nếu xe bị che một phần nhưng vẫn xác định được lớp, vẽ box quanh phần nhìn thấy của xe. Không tự suy đoán phần thân xe bị che.
- Vật thể bị cắt bởi rìa ảnh: Nếu xe nằm một phần ngoài khung hình, chỉ vẽ box cho phần nhìn thấy. Cạnh box chạm rìa ảnh và không vượt ra ngoài.
- Vật thể rất nhỏ/mơ hồ: Chỉ gán nhãn khi có đủ đặc điểm nhận dạng lớp. Nếu xe quá nhỏ, mờ hoặc không thể phân biệt chắc chắn giữa các lớp, ghi lại trường hợp đó để hỏi lab coach.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_003.jpg` |Xe con bị che một phần bởi xe khác. |Gán car, box quanh phần nhìn thấy. |Schema cho phép gán xe bị che nếu vẫn nhận diện được lớp. |
| `drive_005.jpg` |	Xe nằm sát mép ảnh, một phần thân xe nằm ngoài khung.|Gán lớp phù hợp, box dừng tại rìa ảnh|Không dự đoán phần nằm ngoài ảnh.|
| `drive_007.jpg` |Xe nhỏ ở xa, khó phân biệt bus với van.|Chưa tự quyết định lớp; ghi lại để hỏi giảng viên.|Không đủ thông tin nhận dạng chắc chắn.|
