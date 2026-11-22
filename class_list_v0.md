# Danh sách lớp v0 — Nguyễn Thành Tài · G07

Danh sách nháp cho model DMS/OMS. Các giá trị phải phản ánh điều có thể quan sát; phần không rõ dùng `unknown`, không suy đoán từ màu bị mất trên IR.

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face.eye_left`, `face.eye_right` | `open` / `closed` / `unknown` | Phát hiện dấu hiệu buồn ngủ hoặc mắt bị che; theo dõi mức chắc chắn của trạng thái mắt | Thường có, nhưng phản xạ kính và vùng tối có thể làm phải dùng `unknown` |
| 2 | `face.glasses` | `none` / `clear` / `sun` / `unknown` | Nhận biết kính có thể che mắt hoặc tạo phản xạ cho các bước phân tích sau | Có thể nhận ra hình dạng kính; loại kính không phải lúc nào cũng chắc chắn |
| 3 | `face.gaze_zone` | `road` / `mirror_l` / `mirror_r` / `mirror_c` / `cluster` / `center` / `lap` / `other` | Ước lượng tài xế đang nhìn đường, gương, bảng điều khiển hay xuống thấp | Có thể gán theo hướng mặt/mắt; phản xạ NIR làm giảm độ tin cậy |
| 4 | `face13` | 13 điểm mặt; mỗi điểm có `occluded` / `outside` | Cung cấp keypoint để ước lượng hướng đầu, mắt và mức che khuất | Có nếu mặt đủ lớn; không vẽ khi mặt rộng dưới 60 px hoặc điểm không quan sát được |
| 5 | `hand.on_wheel` | `yes` / `no` / `unknown` | Phân tích tay tài xế có đang đặt trên vô lăng hay không | Có thể thấy tay và vô lăng, nhưng vùng tối cần giữ `unknown` |
| 6 | `phone.state` | `held` / `mounted` / `lap` | Phát hiện sử dụng điện thoại và phân biệt điện thoại cầm tay, gắn trên xe hoặc đặt trên đùi | Có thể thấy hình dạng; không gán `phone` nếu vật không đủ rõ |
| 7 | `person.seatbelt` | `not_worn` / `unknown` / `worn_correct` / `worn_incorrect` | Phát hiện việc thắt dây an toàn của tài xế | Thường khó trên áo tối hoặc ảnh NIR; không đoán khi không thấy dây |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person.seat` | `driver` / `front_passenger` / `rear_left` / `rear_middle` / `rear_right` | Xác định người đang ngồi ở vị trí nào để suy luận bố trí cabin và cảnh báo theo ghế | Có thể gán theo vị trí ghế và hướng camera; ghế trái ảnh là `rear_right` của xe |
| 2 | `person.seatbelt` | `not_worn` / `unknown` / `worn_correct` / `worn_incorrect` | Kiểm tra an toàn của người ngồi trong xe | Có thể bị mất trên áo tối; với SVIRO không suy đoán trạng thái `worn_*` |
| 3 | `child_seat.occupied` | `no` / `unknown` | Phát hiện ghế trẻ em trống hay có vật/người bên trong | Hình dạng ghế thường thấy; trạng thái bên trong có thể không rõ trên IR |
| 4 | `infant_seat.facing` | `rear` / `front` | Xác định hướng của nôi để hỗ trợ cảnh báo an toàn và túi khí | Hình dạng nôi thường thấy, nhưng phần dây/chi tiết nhỏ có thể mất |
| 5 | `object.type` | `other` / `bag` / `box` / `cup` | Nhận ra vật thể trong cabin để quản lý vật bỏ quên hoặc vật cản | Có thể nhận ra hình dạng; không dựa vào màu vì IR-sim là đơn sắc |
| 6 | `object.seat` | `driver` / `front_passenger` / `rear_left` / `rear_middle` / `rear_right` | Gắn vật thể với đúng vị trí ghế để cảnh báo theo hành khách | Thường suy ra được từ vị trí tương đối trong cabin |
| 7 | `child_seat` / `infant_seat` | box theo đúng loại ghế | Phân biệt ghế trẻ em với nôi xách, tránh gộp hai loại thành một nhãn | Có thể thấy silhouette; khi không đủ bằng chứng cần đối chiếu RGB |

## 2 câu hỏi leo thang

1. Câu hỏi: Khi ảnh IR-sim làm nôi xách và ghế trẻ em ở `S02_ir` khó phân biệt, ngưỡng hình dạng nào đủ để chọn `infant_seat` thay vì `child_seat`?
   Khung ví dụ: `S02_rgb` và `S02_ir`.
   Bạn tạm gán thế nào và vì sao: Đối chiếu cặp RGB-IR; chọn loại có silhouette rõ nhất, còn chi tiết không nhìn thấy thì không suy đoán thêm thuộc tính.

2. Câu hỏi: Với vùng mắt ở `D04` hoặc `D06`, khi nào dùng `occluded=1` và khi nào dùng `outside=1` nếu thuộc tính mắt là `unknown`?
   Khung ví dụ: `D04.png`.
   Bạn tạm gán thế nào và vì sao: Dùng `occluded=1` khi biết vị trí điểm nhưng bị vật/reflection che; dùng `outside=1` khi điểm nằm ngoài vùng quan sát; giữ `unknown` nếu không đủ bằng chứng về trạng thái mắt.
