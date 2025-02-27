
# Mô hình
![image](https://github.com/user-attachments/assets/2ea72132-f762-4e14-a4a0-5e2375dc43e9)
## Bạn muốn thiết lập Port Forwarding trên máy firewalld để máy client có thể truy cập Internet.
### B1: Bật IP Forwarding
Sửa tệp tin /etc/sysctl.conf
```
vi /etc/sysctl.conf
```
Tìm và sửa dòng:
```
net.ipv4.ip_forward=1
```
![image](https://github.com/user-attachments/assets/66dcbcc7-0e2e-4b6a-b40d-06575ecbb3ea)

### B2: Thêm card mạng vào đúng zone
Thêm `ens33` vào zone `external`:
```
sudo firewall-cmd --permanent --zone=external --add-interface=ens33
```
Thêm `ens38` vào zone `internal`:
```
sudo firewall-cmd --permanent --zone=internal --add-interface=ens38
```
![image](https://github.com/user-attachments/assets/306d8c1a-2a09-4a1b-891c-b00ce7ec3839)
### B3: Bật Masquerade cho NAT
Bật NAT trên `ens33` để client truy cập Internet:
```
sudo firewall-cmd --permanent --zone=external --add-masquerade
```
![image](https://github.com/user-attachments/assets/dc51e612-60b6-4d2b-89e0-6c89caabbe62)
### B4: Cho phép Forwarding giữa hai zone
Thêm luật NAT để cho phép máy Client truy cập Internet:
```
sudo firewall-cmd --permanent --direct --add-rule ipv4 nat POSTROUTING 0 -o ens33 -j MASQUERADE
```
Cho phép chuyển tiếp gói tin từ ens38 đến ens33:
```
sudo firewall-cmd --permanent --direct --add-rule ipv4 filter FORWARD 0 -i ens38 -o ens33 -j ACCEPT
```
### B5: Kiểm tra lại cấu hình
```
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
sudo firewall-cmd --get-active-zones
```
### B6: Kiểm tra trên máy client
Ping thử 8.8.8.8 thành công

![image](https://github.com/user-attachments/assets/1a9887da-886a-422e-a0bf-f023380d9f73)

## Cấu hình nâng cao
### Tạo Zone riêng
Mặc dù, các zone có sẵn là quá đủ với nhu cầu sử dụng , vẫn có thể tạo lập zone của riêng mình để mô tả rõ ràng hơn về các chức năng của chúng .
VD : Có thể tạo riêng một zone cho webserver publicweb hay một zone cấu hình riêng cho DNS trong mạng nội bộ privateDNS . Cần thiết lập permanent khi thêm một zone :
```
firewall-cmd --permanent --new-zone=publicweb
firewall-cmd --permanent --new-zone=privateDNS
firewall-cmd --reload
```
Kiểm tra lại :
```
firewall-cmd --get-zones
```
Khi đã có zone thiết lập riêng , có thể cấu hình như các zone thông thường : thiết lập mặc định , thêm quy tắc : 
```
firewall-cmd --zone=publicweb --add-service=ssh --permanent
firewall-cmd --zone=publicweb --add-service=http --permanent
firewall-cmd --zone=publicweb --add-service=https --permanent
```
###  Định nghĩa services riêng trên FirewallD
- Khi có một service mới thêm vào hệ thống , có 2 phương án :
  - Mở port của service đó trên FirewallD
  - Tự định nghĩa service đó trên FirewallD
Tạo tệp XML cho dịch vụ
```
sudo vi /etc/firewalld/services/adminservice.xml
```
Thêm nội dung sau:
```
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>AdminService</short>
  <description>Admin port 5000 TCP và 6000 UDP</description>
  <port protocol="tcp" port="5000"/>
  <port protocol="udp" port="6000"/>
</service>

![image](https://github.com/user-attachments/assets/e0b7c663-42ce-4dfe-984f-e9ce475c4d10)

```
Cập nhật quyền
```
sudo chmod 640 /etc/firewalld/services/adminservice.xml
```
Tải lại Firewalld để nhận dịch vụ mới
```
sudo firewall-cmd --reload
```
Kiểm tra dịch vụ mới
```
sudo firewall-cmd --get-services | grep adminservice
```
![image](https://github.com/user-attachments/assets/1c5d1c31-0401-4ebd-b9f2-90ed701d5db3)
