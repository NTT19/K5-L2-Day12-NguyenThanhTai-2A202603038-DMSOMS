# TEAM — Ngày 12

Mã bộ ảnh: **G07** · Hình thức: **cá nhân**

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Nguyễn Thành Tài | 2A202603038 | có | PHẠM HỮU HƯNG 2A202603087 | 66 |

Ca hai bạn vẫn không thống nhất và đã hỏi coach (ghi tên ảnh + đối tượng):

- `D02.png`: trạng thái mắt và cờ `occluded` của `face13`.
- `D04.png`: `hand.on_wheel`, `seatbelt` và cách đồng bộ cờ điểm mắt.
- `S02_rgb.png` / `S02_ir.png`: phân biệt `child_seat` với `infant_seat`.

## Cá nhân

### Nguyễn Thành Tài-2A202603038

- Phần mình làm: gán đủ 22 ảnh G07, thực hiện review chéo, điền và xử lý 66 dòng trong `disagreement_log.csv`, sau đó kiểm tra bản export v2.
- Điều mình học được hôm nay: seat OMS phải gọi theo xe chứ không theo phía ảnh; với DMS, `unknown` và cờ `occluded/outside` quan trọng hơn việc đoán trạng thái mắt.
