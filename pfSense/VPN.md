# VPN trên tường lửa PfSense sử dụng OpenVPN 
**MÔ HÌNH**

![image](https://github.com/user-attachments/assets/97875d1d-f8d0-433f-b9a4-30bbf3734ef9)

**Cấu hình hệ thống từ máy trạm Windows 10 hoặc máy tính vật lý truy cập được dữ liệu chia sẽ từ máy chủ Windows Server.**

## Cấu hình trên pfSense
### B1: Tạo trung tâm cấp phát chứng thư số CA local
System → Cert Manager → Certificate Authorities → Add

![image](https://github.com/user-attachments/assets/cceb4a85-9733-49c4-a296-0af13af424d0)
Chọn Add để cài đặt CA local

Đặt tên CA. Chọn Internal Certificate Authority
![image](https://github.com/user-attachments/assets/9ad2da8a-a292-48a2-a56a-3f149fed9ba5)

Các tùy chọn mật mã để mặc định:
![image](https://github.com/user-attachments/assets/b6cb4075-6294-42d4-bce6-d21971420eab)

Khai báo các tham số về vị trí thiết lập:
![image](https://github.com/user-attachments/assets/229d54c8-76a3-4e21-b7d6-c0782e6b1f0a)

Kết quả:
![image](https://github.com/user-attachments/assets/cb729510-b136-4d1d-99d9-803dc3f3eede)

### B2: Tạo chứng thư số cho máy chủ VPN
Certificates → Add
![image](https://github.com/user-attachments/assets/db7b853f-6306-4833-95ba-f25e0b33b557)

Lựa chọn chứng thư số dạng local (internal cert), đặt tên cho chứng thư số:
![image](https://github.com/user-attachments/assets/4905f2e3-c8cf-482d-96d8-34f2ce73f96f)

Các tùy chọn về mật mã để mặc định:
![image](https://github.com/user-attachments/assets/9e8bc6d5-f974-4a88-8229-a0f4019eeac7)

Tùy chọn về loại chứng thư số: Server Certificate
![image](https://github.com/user-attachments/assets/ee9ac787-d51c-43a9-ad0d-c0ce31b4d28a)

### B3: Tạo tài khoản người dùng VPN
System → User Manager → Add để thêm tài khoản người dùng hệ thống
![image](https://github.com/user-attachments/assets/cc3f10ba-279d-447b-aa03-44bb831d4d60)

Đặt tên cho tài khoản người dùng:
![image](https://github.com/user-attachments/assets/c9c3dd8d-8312-4d29-848f-719ef9b26f39)

Tạo chứng thư số cho người dùng:
![image](https://github.com/user-attachments/assets/82e52ff5-e98f-4cae-9e4f-1b263492276d)

### B4: Cấu hình OpenVPN
VPN → OpenVPN → Wizards
Tiếp theo lựa chọn phương thức xác thực: Local user access: tài khoản được tạo trực tiếp trên máy chủ VPN.
![image](https://github.com/user-attachments/assets/2c2e461e-f43d-44c6-a17d-8511891a8b68)

Tiếp theo chọn CA
![image](https://github.com/user-attachments/assets/7719212f-71ab-4392-93e9-51e15153f354)

Tiếp theo chọn chứng thư số Server:
![image](https://github.com/user-attachments/assets/7a51e219-6a78-46f3-9531-382f19a7a700)

Thông tin chung về máy chủ VPN:
![image](https://github.com/user-attachments/assets/572c13f9-4036-4841-9b3f-5a38e90227e0)

Các tùy chọn mật mã để mặc định:
![image](https://github.com/user-attachments/assets/b49c6b6e-2ac1-42b8-82d8-d1eef16a9976)

Trong phần Tunnel Setting: khai báo dải địa chỉ IP sử dụng trong đường hầm và dải IP sử dụng trong mạng nội bộ.
![image](https://github.com/user-attachments/assets/a8fe4337-4027-4a6b-aa64-121b1252a5bb)

Lựa chọn sử dụng địa chỉ IP cho máy trạm:
![image](https://github.com/user-attachments/assets/e488e4f7-7667-44af-bf0a-0c0b57c4c698)

Tiếp theo cần thiết lập luật cho tường lửa để kết nối VPN có thể đi qua:
![image](https://github.com/user-attachments/assets/b7d994ea-c974-462b-aaf2-fa3ba65a4613)

Firewall → Rules → WAN. Bỏ dấu tích ở các luật mặc định:
![image](https://github.com/user-attachments/assets/a2e52ab9-8dd9-41d0-90ad-ca5e5cc488f9)

### B5: Export cấu hình VPN
System → Package Manager → Available Packages
![image](https://github.com/user-attachments/assets/c3cdbbf1-c48f-4c60-872f-2279c900d5f0)

Kết quả:
![image](https://github.com/user-attachments/assets/950b85f9-9357-4912-ac2a-7b6fa6c60605)

Trính xuất gói cấu hình và cài đặt VPN cho máy trạm.
VPN → OpenVPN → Client Export
Lựa chọn phiên bản cài đặt và tải về cho Windows 10
![image](https://github.com/user-attachments/assets/4b5aad08-4da8-4fc0-a4fd-c196f5e3026d)

### B6: Cài đặt và kết nối OpenVPN trên máy trạm
Cài đặt và đăng nhập tài khoản vpn user:
![image](https://github.com/user-attachments/assets/6d83fd13-65a5-4805-bd7d-572283aecdf0)

Kết nối thành công tới máy chủ VPN:
![image](https://github.com/user-attachments/assets/121b4e73-5998-4fe0-8f18-769180057700)
### B7: Kiểm tra kết nối
Kiểm tra địa chỉ IP trên Windows 10.
![image](https://github.com/user-attachments/assets/ba661d64-e1d9-418e-8ac5-ec99531ac2ed)

Kiểm tra kết nối VPN trên tường lửa: Status → OpenVPN
![image](https://github.com/user-attachments/assets/aab09cfb-763f-4370-a178-b9efc48608a4)

Từ máy Windows 10 ping tới máy chủ DC.
![image](https://github.com/user-attachments/assets/01e90add-7ce2-43e3-b2d0-f56624af17c3)

# Xác thực Radius
Thay đổi phương thức xác thực
![image](https://github.com/user-attachments/assets/0d97638e-48ba-4241-80ff-c0cd99363aaa)
## Cấu hình trên máy Windows Server
### B1: Nâng domain
![image](https://github.com/user-attachments/assets/56ddf7bb-32e5-4de9-88df-192dab38b376)

### B2: Tạo tài khoản và nhóm VPN
![image](https://github.com/user-attachments/assets/3dce78db-f0fa-44d2-a082-bb352a949552)

### B3: Cài đặt Network Policy and Access Services
![image](https://github.com/user-attachments/assets/3a29868f-0e0f-4d95-bfb8-63088b390cf0)

### B4: Cấu hình RADIUS client (pfsense) trong Network Policy Server
**Tạo mới và khai báo thông tin về Radisu client:**
![image](https://github.com/user-attachments/assets/82b0abf9-6e6b-4ebb-961c-383e8e321532)

**Tạo mới và thiết lập chính sách cho nhóm người dùng truy cập từ xa:**
![image](https://github.com/user-attachments/assets/94839675-3e5e-453c-84e1-f0f9b6b38cd7)

## Cấu hình pfSense
### B1: Cấu hình xác thực Radius trên pfSense
System → User Manager → Authentication Servers
![image](https://github.com/user-attachments/assets/65882218-ef34-40f5-9935-1cf84aee9040)
Lưu ý: k cần tạo người dùng tại B3 ở trên tại sẽ dùng người dùng trong DC.
### B2: Tải file cấu hình mới và import
Tải lại file cấu hình tại Inline Configurations:
![image](https://github.com/user-attachments/assets/d353f874-ed23-408f-9b34-759b95c38ceb)

import file cấu hình mới
![image](https://github.com/user-attachments/assets/7365e8a5-afef-4683-892e-cfc4f1dc0011)

Tài khoản sẽ là tài khoản đã tạo trong DC
![image](https://github.com/user-attachments/assets/f9849bd7-48b8-41c5-9bc7-a97c96438e3d)

Trên máy winserver2016 dùng wireshark để bắt gói tin. tiến hành reconnect vpn sẽ thấy gói tin xác thực RADIUS
![image](https://github.com/user-attachments/assets/808f2d87-2b2f-4093-adb0-68a0621bf91c)
