# Tổng quan
## Khái niệm
**Firewalld là một dịch vụ quản lý tường lửa động trên hệ điều hành Linux, thay thế iptables, giúp quản lý quy tắc tường lửa một cách linh hoạt mà không cần khởi động lại dịch vụ.**
- **Hỗ trợ vùng (zones):** Phân vùng mạng với chính sách bảo mật khác nhau.
- **Quản lý dịch vụ thay vì cổng:** Cho phép dịch vụ cụ thể mà không cần nhớ số cổng.
- **Cấu hình động:** Thay đổi quy tắc tường lửa mà không cần khởi động lại.
- **Hỗ trợ IPv4, IPv6, NAT và chuyển tiếp gói tin (masquerade, port forwarding).**
### Zone
- Trong FirewallD , zone là một nhóm các quy tắc nhằm chỉ ra những luồng dữ liệu được cho phép , dựa trên mức độ tin tưởng của điểm xuất phát luồng dữ liệu đó trong hệ thống mạng . Để sử dụng , bạn có thể lựa chọn zone mặc định , thiết lập các quy tắc trong zone hay chỉ định network interface để quy định hành vi được cho phép.
  
|Tên vùng |	Mức độ bảo mật|	Mô tả|
|---------|---------------|-------------------------------------------------------------|
|drop	    |Cao nhất      	|Chặn tất cả, chỉ cho phép phản hồi từ kết nối đã có.         |
|block    |	Cao	          |Giống drop, nhưng gửi phản hồi ICMP "host unreachable".      |
|public	  |Trung bình	    |Sử dụng cho mạng công cộng, chỉ cho phép một số dịch vụ.     |
|external |Trung bình     |	Dùng cho kết nối ra Internet, thường bật masquerading (NAT).|
|dmz      |	Trung bình    |	Dùng cho máy chủ trong DMZ, hạn chế truy cập từ Internet. |
|work     |	Thấp	        |Cho phép nhiều dịch vụ hơn trong môi trường làm việc.|
|home     |	Thấp	        |Dành cho mạng gia đình, tin cậy hơn work.|
|internal |	Thấp          |	Dùng cho mạng nội bộ, an toàn hơn home.|
|trusted  |	Thấp nhất    	|Cho phép tất cả kết nối, không chặn gì cả.|
- Hiệu lực của các quy tắc
  - `Runtime` ( mặc định ) : có tác dụng ngay lập tức , mất hiệu lực khi reboot hệ thống .
  - `Permanent` : không áp dụng cho hệ thống đang chạy, cần reload mới có hiệu lực, tác dụng vĩnh viễn cả khi reboot hệ thống.
### Cài đặt 
- **FirewallD** được cài đặt mặc định trên **CentOS 7**.Cài đặt nếu chưa có :
    ```
    yum install firewalld
    ```
    cài đặt trên ubuntu
    ```
    sudo apt install firewalld -y
    ```
- Sau khi cài đặt xong, kiểm tra trạng thái:
  ```
  sudo systemctl status firewalld
  ```
  Nếu chưa chạy, bật và kích hoạt Firewalld:
  ```
  sudo systemctl enable --now firewalld
  ```
### Cấu hình Firewalld
#### Thiết lập các Zone
- Liệt kê tất cả các **zone** trong hệ thống :
    ```
    firewall-cmd --get-zones
    ```
- Kiểm tra **zone** mặc định :
    ```
    firewall-cmd --get-default-zone
    ```

- Kiểm tra **zone active** ( được sử dụng bởi card mạng )
    - Vì **FirewallD** chưa được thiết lập bất kỳ quy tắc nào nên **zone** mặc định cũng đồng thời là zone duy nhất được kích hoạt , điều khiển mọi luồng dữ liệu .
    ```
    firewall-cmd --get-active-zones
    ```

- Thay đổi **default zone** (  vd thành `home` ) :
    ```
    firewall-cmd --set-default-zone=home
    ```

#### Thiết lập các rule
- Liệt kê toàn bộ các **rule** của các **zones** :
    ```
    firewall-cmd --list-all-zones
    ```
- Liệt kê toàn bộ các **rule** trong **default zone** và **active zone** :
    ```
    firewall-cmd --list-all
    ```

- Liệt kê toàn bộ các quy tắc trong một **zone** cụ thể , ví dụ `home` :
    ```
    firewall-cmd --zone=home --list-all
    ```
- Liệt kê danh sách **services/port** được cho phép trong **zone** cụ thể :
    ```
    # firewall-cmd --zone=puclic --list-services
    # firewall-cmd --zone=public --list-ports
    ```

#### Thiết lập cho services
- Xác định các **services** trên hệ thống :
    ```
    firewall-cmd --get-services
    ```
    
    - Thông tin về **services** được lưu trữ tại `/usr/lib/firewalld/services`
- Thiết lập cho phép **services** trên **firewalld** , sử dụng `--add-service` :
    ```
    firewall-cmd --zone=public --add-service=http
    success
    firewall-cmd --zone=public --add-service=http --permanent
    success
    ```
- Thực hiện kiểm tra xem **service** đã được cho phép chưa :
    ```
    firewall-cmd --zone=public --list-services
    ```
- Vô hiệu hóa **services** trên **firewalld** , sử dụng `--remove-service` :
    ```
    firewall-cmd --zone=public --remove-service=http
    firewall-cmd --zone=public --remove-service=http --permanent
    ```
#### Thiết lập cho port
- Mở **port** với tham số `--add-port` :
    ```
    # firewall-cmd --zone=public --add-port=9999/tcp
    # firewall-cmd --zone=public --add-port=9999/tcp --permanent
    ```
- Mở 1 **dải port** :
    ```
    # firewall-cmd --zone=public --add-port=4990-5000/tcp
    # firewall-cmd --zone=public --add-port=4990-5000/tcp --permanent
    ```
- Kiểm tra lại các **port** đã mở :
    ```
    # firewall-cmd --zone=public --list-ports
    ```
- Đóng **port** với tham số `--remove-port` :
    ```
    # firewall-cmd --zone=public --remove-port=9999/tcp
    # firewall-cmd --zone=public --remove-port=9999/tcp --permanent
    ```
