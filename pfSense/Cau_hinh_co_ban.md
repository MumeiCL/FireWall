# Cấu hình ban đầu cho pfSense
Thiết lập mặc định của pfSense chỉ cho truy cập vào trang quản trị Web UI từ mạng LAN nội bộ. Do đó chưa thể truy cập vào địa chỉ 192.168.0.104.
Để có thể truy cập Web UI, cần phải tắt dịch vụ quản lý gói tin (packet filter) bằng lệnh `pfctl -d`
![image](https://github.com/user-attachments/assets/048da9bc-e389-49e2-bfd4-a369856add2c)

**Truy cập vào ip Wan của pfSense**
- Tên đăng nhập và mật khẩu mặc định của pfSense:
  - Tên đăng nhập: admin
  - Mật khẩu: pfsense

![image](https://github.com/user-attachments/assets/2ce3e8cf-cf81-4fd5-919c-58f18a3b3cbb)

## Thông tin chung
![image](https://github.com/user-attachments/assets/96b4bf8f-1f61-4b9d-8c54-a75dcaae83a1)
**Time Server**
![image](https://github.com/user-attachments/assets/18753fc0-4792-4447-8606-4fe55beae0fe)

## Cấu hình WAN interface
- Thiết lập WAN Interface Type là Static cùng thông số IP như sau:

![image](https://github.com/user-attachments/assets/bc791781-b0e0-496c-9469-90775dbcd009)

 **Tùy chọn này sẽ chặn lưu lượng từ các địa chỉ IP private khi đi vào cổng WAN.
Không chọn do pfsense được cài trên máy ảo nên mọi ip truy cập vào WAN interface để cấu hình đều là ip private**
![image](https://github.com/user-attachments/assets/53ca6e41-7f13-4bda-bcfc-c7cf77286112)

## Cấu hình LAN interface
![image](https://github.com/user-attachments/assets/558d7eeb-a6f2-4d78-9ce9-97bf4c48567a)

## Đổi mật khẩu admin
![image](https://github.com/user-attachments/assets/a306d9a9-9652-458b-bb82-78a2a36202fb)
## Cho phép truy cập WebGUI từ WAN
Sau khi cấu hình xong thì reboot và đăng nhập lại theo ip mới. Chú ý: gõ lệnh `pfctl -d` để có thể truy cập
![image](https://github.com/user-attachments/assets/40b26b3e-84b4-4f6a-a89d-ea3b07ec0be5)

**Chọn FireWall -> Rules**
![image](https://github.com/user-attachments/assets/3fab6ecf-8729-4238-a82a-2131c323feb3)

- Chọn add để thêm rule mới. Thiết lập thông số như sau:
  - Action: Pass
  - Interface: WAN
  - Protocol: TCP
  - Source: Any 
  - Destination: WAN Address
  - Destination port range: HTTPS
  - Description: Cho phép truy cập Web UI từ WAN
![image](https://github.com/user-attachments/assets/aa9fd14f-be5f-4114-9cba-1c0136bdbea5)

Bấm Apply Changes để áp dụng Rule mới cho pfSense
Từ giờ có thể truy cập vào mà k cần dùng lệnh `pfctl -d` trước.
