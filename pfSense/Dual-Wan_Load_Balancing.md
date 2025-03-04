# Dual-Wan Load Balancing và Failover
**MÔ HÌNH**

![image](https://github.com/user-attachments/assets/f585039e-0a51-412e-beb2-6357464b5c5d)

**2 máy ảo pfSense đóng vai trò là nhà cung cấp mạng ISP ảo kết nối vào WAN1 và WAN2 của máy ảo pfSense còn lại đóng vai trò là tường lửa chính cấu hình Dual-Wan Load Balancing và Failover**
**Giới hạn băng thông 70Mbps cho WAN1 và 30Mbps cho WAN2**
## Cấu hình Load Balancing
### Tạo Gateway Groups
**System -> Routing -> Gateway Groups. Chọn add để thêm mới**
- Group Name: Load_Balancer
- Gateway Priority: Chọn Tier 1 cho cả hai WAN. 
- Trigger Level: Member Down
- Description: Dual Wan Load Balancer.
**Save -> Apply Change**
  
**Nếu cấu hình Failover WAN 1 – Tier 1 & WAN 2 – Tier 2, chỉ WAN 1 sẽ được sử dụng cho đường truyền chính. Khi WAN 1 mất kết nối, WAN 2 sẽ được sử dụng.**
![image](https://github.com/user-attachments/assets/47cd0c32-1beb-4425-acf6-40e12bb35c26)

### Thiết lập Monitor IP
Để pfSense nhận biết được WAN có mất kết nối Internet không, cần phải thiết lập Monitor IP cho từng Gateway.
WAN 1: 8.8.8.8 (DNS của Google)
WAN 2: 208.67.222.222 (DNS của OpenDNS)
**System -> Routing -> Gateway. Kéo xuống mục Monitor IP**

![image](https://github.com/user-attachments/assets/121772b3-5d57-42cb-ab53-659541eacfc2)

**Làm tương tự với WAN2**

### 
**Để sử dụng Gateway Group cho mạng LAN, chúng ta cần phải điều chỉnh lại Firewall Rule của pfSense.**

![image](https://github.com/user-attachments/assets/49287b80-0ab1-4333-a859-be94eb1dc4c9)

**Vào phần Advanced Option. Mục Gateway: chọn Load_BLancer – Dual WAN Load Balancer. Sau đó bấm Save -> Apply Change**

![image](https://github.com/user-attachments/assets/4156e7f9-b071-4283-948f-d65cba9cd930)

## Kiểm tra
**Trước khi cấu hình Load Balancing**

![image](https://github.com/user-attachments/assets/6c242c7a-dea3-4b6b-9100-735b8663b970)

**Sau khi cấu hình Load Balancing**

![image](https://github.com/user-attachments/assets/1a91fc28-0e09-405b-ba56-082dc1ad6851)

**Load Balancing mặc định sẽ hoạt động như một giải pháp Failover khi 1 trong 2 cổng WAN mất kết nối. Giả sử WAN2 mất kết nối**

![image](https://github.com/user-attachments/assets/1cc50e22-5a0c-4c08-a02f-97b0d3a9766c)

