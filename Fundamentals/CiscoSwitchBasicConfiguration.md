# Bảng Cấu Hình Cơ Bản Của Switch Cisco

## Tổng Quan
Tài liệu này cung cấp các lệnh cấu hình cơ bản thường được sử dụng trên các switch Cisco. Đây là tài liệu tham khảo nhanh cho phần "Cisco Core Technologies" trong lộ trình học networking.

## Truy Cập Switch

| Mục Đích | Lệnh | Mô Tả |
|----------|------|-------|
| Truy cập privileged mode | `Switch> enable` | Chuyển đến chế độ privileged EXEC (được đánh dấu bằng dấu #) |
| Truy cập cấu hình | `Switch# configure terminal` | Chuyển đến chế độ global configuration |
| Thoát chế độ hiện tại | `Switch(config)# exit` | Quay lại chế độ trước đó |
| Thoát hoàn toàn | `Switch# end` hoặc `Ctrl+Z` | Quay trở lại privileged mode từ bất kỳ chế độ nào |

## Cấu Hình Cơ Bản

| Mục Đích | Lệnh | Mô Tả |
|----------|------|-------|
| Đặt tên cho switch | `Switch(config)# hostname [Tên_Switch]` | Định danh cho thiết bị trong mạng |
| Đặt mật khẩu enable | `Switch(config)# enable secret [mật_khẩu]` | Đặt mật khẩu bảo vệ privileged mode |
| Cấu hình console | `Switch(config)# line console 0`<br>`Switch(config-line)# password [mật_khẩu]`<br>`Switch(config-line)# login` | Đặt mật khẩu cho kết nối console |
| Cấu hình telnet/SSH | `Switch(config)# line vty 0 15`<br>`Switch(config-line)# password [mật_khẩu]`<br>`Switch(config-line)# login` | Đặt mật khẩu cho kết nối từ xa |
| Mã hóa mật khẩu | `Switch(config)# service password-encryption` | Mã hóa tất cả mật khẩu văn bản thường trong cấu hình |
| Thông báo đăng nhập | `Switch(config)# banner motd #[Nội_dung]#` | Hiển thị thông báo khi người dùng đăng nhập |

## Cấu Hình Quản Lý IP

| Mục Đích | Lệnh | Mô Tả |
|----------|------|-------|
| Cấu hình IP quản lý | `Switch(config)# interface vlan 1`<br>`Switch(config-if)# ip address [IP] [subnet_mask]`<br>`Switch(config-if)# no shutdown` | Đặt địa chỉ IP quản lý cho switch |
| Đặt default gateway | `Switch(config)# ip default-gateway [IP_gateway]` | Đặt gateway mặc định cho quản lý từ xa |

## Cấu Hình Cổng (Port)

| Mục Đích | Lệnh | Mô Tả |
|----------|------|-------|
| Cấu hình một cổng | `Switch(config)# interface [loại] [số]`<br>Ví dụ: `interface gigabitethernet 0/1` | Truy cập cấu hình một cổng cụ thể |
| Cấu hình nhiều cổng | `Switch(config)# interface range [loại] [số-bắt_đầu - số_kết_thúc]`<br>Ví dụ: `interface range fa0/1-24` | Cấu hình nhiều cổng cùng lúc |
| Mô tả cổng | `Switch(config-if)# description [mô_tả]` | Thêm ghi chú cho cổng |
| Thiết lập tốc độ | `Switch(config-if)# speed [10 \| 100 \| 1000 \| auto]` | Đặt tốc độ cho cổng |
| Thiết lập duplex | `Switch(config-if)# duplex [full \| half \| auto]` | Đặt chế độ duplex cho cổng |
| Bật/tắt cổng | `Switch(config-if)# [no] shutdown` | Bật hoặc tắt cổng |

## Cấu Hình VLAN Cơ Bản

| Mục Đích | Lệnh | Mô Tả |
|----------|------|-------|
| Tạo VLAN | `Switch(config)# vlan [id]`<br>`Switch(config-vlan)# name [tên_VLAN]` | Tạo VLAN mới với ID và tên |
| Gán cổng vào VLAN | `Switch(config)# interface [loại] [số]`<br>`Switch(config-if)# switchport mode access`<br>`Switch(config-if)# switchport access vlan [id]` | Gán cổng vào VLAN cụ thể |
| Cấu hình cổng trunk | `Switch(config)# interface [loại] [số]`<br>`Switch(config-if)# switchport mode trunk`<br>`Switch(config-if)# switchport trunk allowed vlan [danh_sách_id]` | Thiết lập cổng trunk để kết nối giữa các switch |

## Xem Trạng Thái và Cấu Hình

| Mục Đích | Lệnh | Mô Tả |
|----------|------|-------|
| Xem cấu hình đang chạy | `Switch# show running-config` | Hiển thị cấu hình hiện tại trong RAM |
| Xem cấu hình khởi động | `Switch# show startup-config` | Hiển thị cấu hình được lưu trong NVRAM |
| Xem thông tin VLAN | `Switch# show vlan [brief]` | Hiển thị danh sách VLAN và các cổng |
| Xem thông tin cổng | `Switch# show interfaces status` | Hiển thị trạng thái tất cả các cổng |
| Xem thông tin cổng cụ thể | `Switch# show interface [loại] [số]` | Hiển thị thông tin chi tiết về một cổng |
| Xem bảng MAC | `Switch# show mac address-table` | Hiển thị bảng địa chỉ MAC |
| Xem thông tin hệ thống | `Switch# show version` | Hiển thị thông tin phiên bản IOS và phần cứng |

## Lưu Cấu Hình

| Mục Đích | Lệnh | Mô Tả |
|----------|------|-------|
| Lưu cấu hình | `Switch# write memory` hoặc<br>`Switch# copy running-config startup-config` | Lưu cấu hình hiện tại vào NVRAM |
| Xóa cấu hình | `Switch# write erase` hoặc<br>`Switch# erase startup-config` | Xóa tệp cấu hình khởi động |

## Quy Trình Cấu Hình Cơ Bản Đề Xuất

1. Kết nối với switch qua cổng console
2. Đặt tên cho switch
3. Cấu hình mật khẩu bảo mật
4. Mã hóa tất cả mật khẩu
5. Đặt banner đăng nhập
6. Cấu hình IP quản lý và default gateway
7. Cấu hình cơ bản cho các cổng
8. Cấu hình VLAN nếu cần thiết
9. Lưu cấu hình
10. Xác minh cấu hình

## Ví Dụ Cấu Hình Hoàn Chỉnh

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW-Building1
SW-Building1(config)# enable secret Cisco123
SW-Building1(config)# line console 0
SW-Building1(config-line)# password Cisco123
SW-Building1(config-line)# login
SW-Building1(config-line)# exit
SW-Building1(config)# line vty 0 15
SW-Building1(config-line)# password Cisco123
SW-Building1(config-line)# login
SW-Building1(config-line)# exit
SW-Building1(config)# service password-encryption
SW-Building1(config)# banner motd #WARNING: Authorized Personnel Only!#
SW-Building1(config)# interface vlan 1
SW-Building1(config-if)# ip address 192.168.10.10 255.255.255.0
SW-Building1(config-if)# no shutdown
SW-Building1(config-if)# exit
SW-Building1(config)# ip default-gateway 192.168.10.1
SW-Building1(config)# vlan 10
SW-Building1(config-vlan)# name MANAGEMENT
SW-Building1(config-vlan)# exit
SW-Building1(config)# vlan 20
SW-Building1(config-vlan)# name PRODUCTION
SW-Building1(config-vlan)# exit
SW-Building1(config)# interface range fastEthernet 0/1-12
SW-Building1(config-if-range)# switchport mode access
SW-Building1(config-if-range)# switchport access vlan 10
SW-Building1(config-if-range)# exit
SW-Building1(config)# interface range fastEthernet 0/13-24
SW-Building1(config-if-range)# switchport mode access
SW-Building1(config-if-range)# switchport access vlan 20
SW-Building1(config-if-range)# exit
SW-Building1(config)# interface gigabitEthernet 0/1
SW-Building1(config-if)# switchport mode trunk
SW-Building1(config-if)# exit
SW-Building1(config)# exit
SW-Building1# write memory
```

## Lưu ý quan trọng
- Luôn lưu cấu hình sau khi thực hiện thay đổi
- Ghi chú lại các thay đổi cấu hình quan trọng
- Xác minh cấu hình để đảm bảo hoạt động đúng
- Tạo bản sao lưu cấu hình trước khi thực hiện thay đổi lớn