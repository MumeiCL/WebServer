# Keepalived
- Keepalived là một công cụ giúp thiết lập High Availability (HA) và Failover trên hệ thống có nhiều Nginx server.
- Nó sử dụng Virtual Router Redundancy Protocol (VRRP) để tạo một IP ảo (VIP - Virtual IP), giúp tự động chuyển đổi khi server chính bị lỗi.
- Khi server chính (Master) bị down, Keepalived sẽ tự động chuyển VIP sang server dự phòng (Backup).

**Mô hình**
  
|Server	     |Interface	|IP           |
|------------|----------|-------------|
|Nginx Master|eth0      |192.168.1.101|
|Nginx Backup|eth0      |192.168.1.102|

## Cấu hình keepalived
**Cài đặt**
```
sudo apt update && sudo apt install -y nginx keepalived
```

**Cho phép gắn IP ảo lên card mạng và IP Forward**
```
echo "net.ipv4.ip_nonlocal_bind = 1" >> /etc/sysctl.conf
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
sysctl -p
```

**Cấu hình Keepalived trên Master**
```
vrrp_instance VI_1 {
    state MASTER
    interface eth0       # Interface mạng (thay đổi nếu cần)
    virtual_router_id 51 # ID nhóm VRRP (cả 2 server phải giống nhau)
    priority 100         # Giá trị cao hơn Backup
    advert_int 1         # Thời gian gửi gói tin VRRP

    authentication {
        auth_type PASS
        auth_pass 123456
    }

    virtual_ipaddress {
        192.168.1.100    # Địa chỉ IP ảo (VIP)
    }
}
```

**Cấu hình trên node backup (NGINX 2)**
```
vrrp_instance VI_1 {
    state BACKUP
    interface eth0
    virtual_router_id 51
    priority 90         # Giá trị thấp hơn Master
    advert_int 1

    authentication {
        auth_type PASS
        auth_pass 123456
    }

    virtual_ipaddress {
        192.168.1.100
    }
}
```

**Thực hiện trên cả 2 server**
```
sudo systemctl restart keepalived
sudo systemctl enable keepalived
```
