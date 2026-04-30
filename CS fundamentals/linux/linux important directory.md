Linux có các tập tin quan trọng sau:
- `etc`: nơi lưu trữ các system configuration file quan trong như `etc/passwd`, `etc/fstab` và `etc/host`
- `home`: tập hợp các thư mục riêng cho từng user.
- `temp` : thư mục chứa các file tạm sẽ bị xóa khi reboot
- `var`: cũng là file tạm nhưng không bị xóa khi reboot như `var/log`. User chủ động xóa khi cần thiết.
- `dev`: device file (`dev/sda`  hard drives)
- `bin`:  gồm nhiều exec  cơ bản cho regular user như `cd`, `ls`
- `sbin`: chứa các exec cho root user để quản lý hệ thống.