# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Xe con chở người, gồm sedan, hatchback, SUV, taxi và xe bán tải theo schema của bài |Xe bán tải thuộc `car`, không thuộc `truck`; xe đang đỗ vẫn được gán nhãn. 
| `truck` |Xe tải chở hàng hoặc xe đầu kéo.  |Phân biệt với xe bán tải có cabin nhỏ (car) và xe buýt chở khách (bus).  |
| `bus` |Xe buýt hoặc minibus dùng để chở khách. |Có thân xe và bố trí phục vụ chở nhiều hành khách, không gộp với van chỉ vì kích thước tương tự.
| `van` |Xe van hoặc minivan chở hàng/chở người. |Phân biệt với xe con thông thường (car) và xe buýt/minibus (bus) theo chức năng, hình dáng quan sát được. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: vẽ hình chữ nhật ôm sát phần thân xe nhìn thấy được; không lấy thừa mặt đường, bầu trời, bóng đổ hoặc phần của xe bên cạnh. Mỗi xe hợp lệ có một box riêng, kể cả khi nhiều xe cùng lớp hoặc nằm sát nhau.
- Vật thể bị che khuất: vẫn gán nhãn nếu phần còn thấy đủ để nhận diện lớp; chỉ vẽ quanh phần nhìn thấy, không đoán phần bị che.
- Vật thể bị cắt bởi rìa ảnh: box chạm rìa ảnh và chỉ bao phần xe còn nằm trong ảnh.
- Vật thể rất nhỏ/mơ hồ: chỉ gán khi vẫn xác định được đó là xe thuộc một trong bốn lớp, nếu không đủ căn cứ xác định lớp thì không đoán và ghi lại để hỏi giảng viên.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_005.jpg` |	Xe màu trắng thân dài gần dải phân cách bên trái, dễ phân vân giữa van và car.|Gán van. |Xe có thân dạng hộp kín, ba cửa sổ đặc trưng và hình dáng giống xe dùng để chở đồ hoặc hàng; phù hợp định nghĩa van hơn xe con thông thường.
| `drive_026.jpg` |	Nhiều xe lớn xếp sát, che khuất nhau ở nửa trái ảnh. | Vẽ một box riêng cho từng xe còn nhận diện được, bám theo phần thân nhìn thấy; xác định lớp của từng xe theo schema.|Mỗi xe hợp lệ cần một box riêng. Phần bị che không được đoán thêm vào box; xe chở khách có đặc điểm của xe buýt được phân vào bus. |
| `drive_...` |	Xe bạc bị cắt ở mép dưới và nhiều xe rất nhỏ ở xa. |Gán xe bạc là car, với box dừng ở rìa ảnh. Chỉ gán xe ở xa khi đủ rõ để xác định lớp. |Xe bị cắt khung vẫn cần nhãn nếu còn nhận diện được, nhưng box không bao phần nằm ngoài ảnh; không đoán lớp cho vật thể quá mơ hồ. |
