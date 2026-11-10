# Mini guideline — bài làm cá nhân

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
|---|---|---|
| `car` | Xe ô tô chở người, thường có tối đa 9 chỗ ngồi. | Sedan, hatchback, SUV. |
| `truck` | Xe tải được thiết kế chủ yếu để vận chuyển hàng hóa. | Xe tải thùng, xe tải ben. |
| `bus` | Phương tiện chở nhiều hành khách, thường có khoang hành khách lớn. | Xe buýt nội đô, xe khách. |
| `van` | Xe có thân kín, khoang chở người hoặc hàng hóa, kích thước nhỏ hơn xe tải lớn. | Minivan, xe van chở hàng. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Bao quanh toàn bộ phần vật thể nhìn thấy được, không lấy quá nhiều nền xung quanh.
- Vật thể bị che khuất: Ước lượng bounding box toàn bộ vật thể nếu phần còn nhìn thấy đủ để nhận diện. Nếu bị che khuất quá nhiều và không thể xác định class thì không gán nhãn.
- Vật thể bị cắt bởi rìa ảnh: Vẽ box đến sát rìa ảnh, bao quanh phần vật thể xuất hiện trong ảnh.
- Vật thể rất nhỏ/mơ hồ: Chỉ gán nhãn khi có đủ đặc điểm để xác định đó là phương tiện thuộc một trong bốn lớp.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
|---|---|---|---|
| `drive_...` | Xe con bị che khuất một phần | Gán nhãn theo phần nhìn thấy nếu xác định rõ là car. | Không suy đoán phần bị che, box chỉ bao quanh vùng nhìn thấy. |
| `drive_...` | Khó phân biệt van và car | Chọn lớp dựa trên hình dáng thân xe và đặc điểm khoang xe. | Phân biệt theo định nghĩa lớp đã thống nhất. |
| `drive_...` | Xe bị cắt một phần ở mép ảnh | Vẫn gán nhãn nếu nhận diện được loại xe. | Box giới hạn trong phần ảnh chứa vật thể. |

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
|---|---|---|---|
| `drive_038.jpg` | Xe con bị che khuất một phần bởi phương tiện khác. | Gán nhãn car, bounding box ước lượng phạm vi toàn bộ phương tiện, bao gồm cả phần bị che khuất. | Dựa vào hình dạng và cấu trúc phần xe còn nhìn thấy để ước lượng kích thước, vị trí của toàn bộ phương tiện. |
| `drive_027.jpg` | Hai phương tiện ở gần nhau, bounding box của car và van chồng lấn một phần. | Giữ cả hai bounding box và gán nhãn riêng cho từng phương tiện. | Mỗi phương tiện là một đối tượng độc lập, cần có bounding box và class riêng. Việc hai box chồng lấn một phần không phải lý do để loại bỏ một box. |
| `drive_026.jpg` | Xe car bị che khuất phần lớn, không đủ đặc điểm để nhận diện chắc chắn. | Không gán nhãn đối tượng này. | Tránh gán nhãn khi không đủ thông tin để xác định class theo schema, hạn chế nhãn không chắc chắn. |