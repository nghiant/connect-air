# Ghép nối
Theo mặc định, tiện ích bổ sung có `permit_join` được đặt thành `false`. Để cho phép các thiết bị tham gia, bạn cần kích hoạt tùy chọn này sau khi tiện ích bổ sung đã bắt đầu. Bây giờ, bạn có thể sử dụng **giao diện người dùng tích hợp** để thực hiện điều này. Để biết chi tiết về cách bật giao diện người dùng tích hợp, hãy xem phần tiếp theo.

# Bật giao diện người dùng tích hợp
Bật `ingress` để giao diện người dùng khả dụng trong Giao diện người dùng của bạn: **Cài đặt → Tiện ích bổ sung → ConnectAir → Hiển thị trong thanh bên**.

# Cấu hình
Cấu hình cần thiết để khởi động ConnectAir có sẵn trong cấu hình tiện ích bổ sung. Các tùy chọn còn lại có thể được cấu hình thông qua giao diện ConnectAir.

# Sao lưu cấu hình
Phần bổ trợ sẽ tạo bản sao lưu configuration.yml của bạn trong đường dẫn dữ liệu của bạn: `$DATA_PATH/configuration.yaml.bk`. Khi nâng cấp, bạn nên sử dụng phần này để điền các giá trị có liên quan vào cấu hình mới của mình, đặc biệt là khóa mạng, để tránh làm hỏng mạng và phải sửa chữa tất cả các thiết bị của bạn.
Bản sao lưu cấu hình của bạn được tạo khi khởi động phần bổ trợ nếu không tìm thấy bản sao lưu trước đó.

# Bật watchdog
Để tự động khởi động lại ConnectAir trong trường hợp lỗi mềm (như "bộ điều hợp bị ngắt kết nối"), có thể sử dụng watchdog. Có thể bật watchdog bằng cách thêm nội dung sau vào cấu hình phần bổ trợ:

```yaml
watchdog: default
```

Phần bổ trợ này sẽ sử dụng độ trễ thử lại watchdog mặc định là 1 phút, 5 phút, 15 phút, 30 phút, 60 phút. Độ trễ tùy chỉnh cũng được hỗ trợ, ví dụ: `watchdog: 5,10,30` sẽ khởi động ConnectAir với độ trễ thử lại của watchdog là 5 phút, 10 phút, 30 phút.

# Lưu ý
- Tùy thuộc vào cấu hình của bạn, cấu hình máy chủ AirMQTT có thể cần bao gồm cổng, thường là `1883` hoặc `8883` cho giao tiếp SSL. Ví dụ: `mqtt://core-mosquitto:1883` cho tiện ích bổ sung Mosquitto của Omaika Assistant.
- Để tìm ra cổng nối tiếp nào bạn đã mở, hãy vào **Giám sát viên → Hệ thống → Hệ thống máy chủ → ⋮ → Phần cứng**

# Socat
Trong một số trường hợp, không thể chuyển tiếp thiết bị nối tiếp đến vùng chứa mà zigbee2mqtt chạy trong đó. Nguyên nhân có thể là do thiết bị không được kết nối vật lý với máy.

Có thể sử dụng Socat để chuyển tiếp thiết bị nối tiếp qua TCP đến ConnectAir.

Bạn có thể cấu hình mô-đun socat trong phần socat bằng các tùy chọn sau:

- `enabled` true/false để bật socat (mặc định: false)
- `master` master hoặc địa chỉ đầu tiên được sử dụng trong dòng lệnh socat (bắt buộc)
- `slave` slave hoặc địa chỉ thứ hai được sử dụng trong dòng lệnh socat (bắt buộc)
- `options` tùy chọn bổ sung được thêm vào dòng lệnh socat (tùy chọn)
- `log` true/false nếu muốn ghi lại stdout/stderr của socat vào data_path/socat.log (mặc định: false)

**LƯU Ý:** Bạn sẽ phải thay đổi cả tùy chọn `master` và `slave` theo nhu cầu của mình. Các giá trị mặc định sẽ đảm bảo rằng socat lắng nghe trên cổng `8485` và chuyển hướng đầu ra của nó đến `/dev/ttyZ2M`. Cài đặt cổng nối tiếp của zigbee2mqtt KHÔNG được tự động đặt và phải được thay đổi cho phù hợp.