# Tự đánh giá — Nguyễn Thành Tài · G07

## 1. Tự rà soát 4 cảnh

Đã chọn 2 ảnh OMS và 2 ảnh DMS, đối chiếu với ảnh cùng cặp và các dòng trong `disagreement_log.csv`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| `S02_rgb.png` | `child_seat` / `infant_seat` | `wrong_class` | Hình dạng ghế/nôi ở `rear_right` dễ bị nhầm; đối chiếu thêm `S02_ir.png` và gọi ghế theo loại xe, không theo phía ảnh. | Có, kiểm lại trong bản v2 |
| `S03_ir.png` | `object` | `missing` | Review chéo nhắc kiểm tra vật ở `rear_left`; chỉ giữ box khi hình dạng đủ rõ và chọn `object.type` theo hình dạng. | Đã rà lại |
| `D04.png` | `face13` và mắt trái | `flag` | `eye_left=unknown` phải nhất quán với cờ `occluded/outside` của bốn điểm mắt trái. | Có, bản v2 không còn cảnh báo |
| `D06.png` | `eye_right` | `guess` | Không chọn `open` chỉ vì thấy một phần mắt; đối chiếu bốn điểm và dùng `unknown` khi vùng mắt không đủ rõ. | Có, bản v2 không còn cảnh báo |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: `guess` — gán trạng thái mắt hoặc cờ điểm mặt khi vùng mắt bị che chưa đủ rõ.

Vì sao mình mắc: Mình nhìn nhanh một vài điểm sáng trong ảnh NIR rồi suy ra trạng thái mắt, thay vì kiểm tra cả bốn điểm và đối chiếu với thuộc tính `eye_left`/`eye_right`.

Lần sau tránh bằng cách: Dùng `unknown` khi không chắc; đặt `occluded=1` nếu biết vị trí nhưng bị che, `outside=1` nếu điểm không nằm trong vùng quan sát; sau đó chạy `cabin_check` trước khi export.

## 3. Ghi chú về disagreement_log

Ở dòng `S03_ir.png` về `object`, bạn review chéo đã đúng khi nhắc mình kiểm tra lại khả năng bỏ sót vật thể. Mình học được rằng phải rà từng vật nhìn thấy trong cabin và kiểm tra cả cặp RGB/IR, không được mặc định ảnh IR không có vật chỉ vì vật có tương phản thấp.
