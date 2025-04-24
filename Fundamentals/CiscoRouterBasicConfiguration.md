# Cấu Hình Cơ Bản Router Cisco

## Giới Thiệu
Tài liệu này hướng dẫn các bước cấu hình cơ bản cho Router Cisco, bao gồm các thiết lập ban đầu, cấu hình giao diện, và các tính năng thiết yếu cho hoạt động mạng.

## Kết Nối với Router
1. **Kết nối Console**:
   - Sử dụng cáp console (thường là cáp RJ-45-to-DB9 hoặc USB-to-Serial)
   - Cấu hình terminal emulator (như PuTTY) với các thông số:
     - Speed: 9600 baud
     - Data bits: 8
     - Stop bits: 1
     - Parity: None
     - Flow Control: None

2. **Kết nối SSH/Telnet** (sau khi đã cấu hình):
   - Đảm bảo có kết nối IP đến router
   - Sử dụng client SSH hoặc Telnet để kết nối

## Chế Độ Hoạt Động của IOS
Router Cisco có các chế độ hoạt động chính:

1. **User EXEC Mode** (`Router>`): Chỉ cho phép xem thông tin cơ bản
2. **Privileged EXEC Mode** (`Router#`): Cho phép xem cấu hình và debug
3. **Global Configuration Mode** (`Router(config)#`): Cho phép thay đổi cấu hình toàn cục
4. **Interface Configuration Mode** (`Router(config-if)#`): Cấu hình cho một giao diện cụ thể
5. **Line Configuration Mode** (`Router(config-line)#`): Cấu hình cho các line kết nối
6. **Router Configuration Mode** (`Router(config-router)#`): Cấu hình giao thức định tuyến

## Các Bước Cấu Hình Cơ Bản

### 1. Thiết lập ban đầu
```
Router> enable                      # Vào chế độ privileged EXEC
Router# configure terminal          # Vào chế độ global configuration
Router(config)# hostname R1         # Đặt tên cho router
R1(config)# enable secret password  # Đặt mật khẩu bảo vệ chế độ privileged EXEC
```

### 2. Cấu hình bảo mật cơ bản
```
R1(config)# service password-encryption      # Mã hóa mật khẩu
R1(config)# banner motd #Unauthorized access prohibited!#  # Thiết lập banner
R1(config)# no ip domain-lookup              # Tắt DNS lookup để tránh trễ khi gõ sai lệnh
```

### 3. Cấu hình đường console và VTY
```
R1(config)# line console 0
R1(config-line)# password console-password
R1(config-line)# login
R1(config-line)# exit

R1(config)# line vty 0 4
R1(config-line)# password vty-password
R1(config-line)# login
R1(config-line)# transport input ssh telnet  # Cho phép kết nối SSH và Telnet
R1(config-line)# exit
```

### 4. Cấu hình giao diện
```
# Cấu hình giao diện LAN
R1(config)# interface GigabitEthernet0/0
R1(config-if)# description LAN Connection
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

# Cấu hình giao diện WAN
R1(config)# interface GigabitEthernet0/1
R1(config-if)# description WAN Connection
R1(config-if)# ip address 203.0.113.2 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# exit
```

### 5. Cấu hình định tuyến tĩnh cơ bản
```
R1(config)# ip route 192.168.2.0 255.255.255.0 203.0.113.1  # Thêm route tĩnh
```

### 6. Cấu hình DHCP
```
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10  # Loại trừ dải IP
R1(config)# ip dhcp pool LAN-POOL
R1(dhcp-config)# network 192.168.1.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.1.1
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# lease 7                                       # Lease time 7 ngày
R1(dhcp-config)# exit
```

### 7. Cấu hình NAT cơ bản
```
# Định nghĩa ACL để xác định traffic cần NAT
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

# Cấu hình NAT
R1(config)# ip nat inside source list 1 interface GigabitEthernet0/1 overload

# Áp dụng NAT cho các interface
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip nat inside
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip nat outside
R1(config-if)# exit
```

### 8. Lưu cấu hình
```
R1# copy running-config startup-config  # Lưu cấu hình vào NVRAM
```

## Xác minh Cấu hình

Lệnh kiểm tra cấu hình:
```
show running-config               # Hiển thị cấu hình đang chạy
show ip interface brief           # Hiển thị tóm tắt các giao diện IP
show ip route                     # Hiển thị bảng định tuyến
show ip dhcp binding              # Hiển thị các binding DHCP
show ip nat translations          # Hiển thị bảng NAT
show version                      # Hiển thị phiên bản IOS và thông tin phần cứng
```

## Xử Lý Sự Cố Cơ Bản
1. **Ping** - Kiểm tra kết nối layer 3: `ping ip-address`
2. **Traceroute** - Kiểm tra đường đi của gói tin: `traceroute ip-address`
3. **Debug** - Bật chế độ debug: `debug ip routing`

## Thực Hành
Hãy cố gắng thực hiện các bước cấu hình này trong môi trường lab như Cisco Packet Tracer hoặc GNS3, và thử nghiệm với các mô hình mạng khác nhau để hiểu rõ hơn về cách hoạt động của router.

## Lưu Ý Quan Trọng
- Luôn sao lưu cấu hình trước khi thực hiện thay đổi lớn
- Cẩn thận khi cấu hình từ xa để tránh mất kết nối
- Sử dụng các biện pháp bảo mật mạnh (mật khẩu phức tạp, SSH thay vì Telnet)
- Luôn cập nhật IOS để khắc phục lỗ hổng bảo mật