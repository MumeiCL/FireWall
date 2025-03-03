# Cấu hình DHCP Server, SSH Server
**MÔ HÌNH**
![image](https://github.com/user-attachments/assets/687cb22f-ad9d-494e-ad75-d60eb18e8d92)

## Cấu hình DHCP Server
**Services -> DHCP Server**

![image](https://github.com/user-attachments/assets/bc91fc7a-3d1a-4af1-890a-1ba7c258a2e3)

- Điều chỉnh lại thông số cho phù hợp với nhu cầu:
  - Nhớ bấm Tick vào mục Enable DHCP Server on LAN interface để kích hoạt DHCP Server
  - Range: Mình chỉnh lại From 10.0.0.100 – To 10.0.0.199, nghĩa là DHCP Server sẽ sử dụng IP Range từ 10.0.0.100 đến 10.0.0.199 cho các máy tính trong mạng LAN
  - DNS Servers: 8.8.8.8 và 8.8.4.4
**Sau đó bấm Save -> Apply change**

![image](https://github.com/user-attachments/assets/ec08ab1f-748e-43be-b89e-28a2fda73bfd)

**Kiểm tra trên máy client. Máy client đã nhận ip từ DHCP Server và kết nối đc internet**

![image](https://github.com/user-attachments/assets/55c8c9e7-534b-436d-9340-c27ee1765252)

**Status -> DHCP Leases Để xem tình trạnh hoạt động của DHCP Server**

![image](https://github.com/user-attachments/assets/e6ae751e-cc03-49b0-82ff-5087cbfb0d9c)

## Cấu hình SSH Server
**System -> Advanced -> Admin Access. Kéo xuống đến mục Secure Shell tích chọn**

![image](https://github.com/user-attachments/assets/ee839a19-2cbd-45bc-8cfa-29ff65014c89)

**Cần phải tạo thêm 1 Firewall để mở cổng 22 cho truy cập từ WAN vào pfSense**

![image](https://github.com/user-attachments/assets/bfa1e99c-4f79-4a20-9c0b-fbceb462e18e)


