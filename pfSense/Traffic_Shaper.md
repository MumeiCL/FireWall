# Cấu hình giới hạn băng thông
## Tạo Limiter Download
**Firewall → Traffic Shaper → Limiters → New Limiter**

![image](https://github.com/user-attachments/assets/022c9ae0-2a54-4f75-b94e-db75d3e6fb98)

## Tạo Limiter Upload

![image](https://github.com/user-attachments/assets/f90ad1bd-46e5-43b8-8e2c-29f58d4c401d)

## Áp dụng Limiters vào Firewall Rule
**Firewall → Rules → LAN. Tìm rule "Default allow LAN to any" → Nhấn Edit**

**Kéo xuống dưới, bấm vào nút Display Advanced để hiện ra các thông số cấu hình nâng cao.**

![image](https://github.com/user-attachments/assets/b41a898c-3236-47cf-90b9-cd9c8ec9d2c1)

- **Tìm In / Out Pipe**
  - **In Pipe: Upload**
  - **Out Pipe: Download**

![image](https://github.com/user-attachments/assets/646b4c63-5f98-4f13-ab67-87250f5be885)
