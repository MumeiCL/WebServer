# Loadbalancing
**Load Balancing là kỹ thuật phân phối lưu lượng truy cập giữa nhiều backend server để tăng hiệu suất, độ sẵn sàng và khả năng chịu lỗi.
Khi có nhiều request từ client, Nginx sẽ phân tán request đến nhiều server backend thay vì dồn vào một server duy nhất.**
## Các thuật toán Load Balancing trong Nginx
### Round Robin (Mặc định)
- Phân phối request theo vòng tuần hoàn (server 1 → server 2 → server 3 → ...).
- Không xét đến tải của server, chỉ đơn giản là luân phiên gửi request.
- Phù hợp với các server có tài nguyên tương đương.
```
upstream backend {
    server 192.168.1.101;
    server 192.168.1.102;
    server 192.168.1.103;
}
```

### Least Connections
- Gửi request đến server có ít kết nối hoạt động nhất.
- Phù hợp với hệ thống có request nặng hoặc thời gian xử lý không đều.
```
upstream backend {
    least_conn;
    server 192.168.1.101;
    server 192.168.1.102;
}
```

### IP Hash
- Dùng địa chỉ IP client để quyết định server backend.
- Cùng một IP sẽ luôn được chuyển đến cùng một server, giúp duy trì session.
- Phù hợp với ứng dụng có session-based authentication (ví dụ: đăng nhập web).
- Ví dụ:
  - Client A (IP: 203.0.113.50) luôn đến 192.168.1.101.
  - Client B (IP: 192.168.10.5) luôn đến 192.168.1.102.
```
upstream backend {
    ip_hash;
    server 192.168.1.101;
    server 192.168.1.102;
}
```

### Weighted Load Balancing
- Gán trọng số (weight) để phân bổ nhiều request hơn cho server mạnh hơn.
- Ví dụ:
  - 192.168.1.101 nhận 75% request.
  - 192.168.1.102 nhận 25% request.
```
upstream backend {
    server 192.168.1.101 weight=3;
    server 192.168.1.102 weight=1;
}
```

### Failover (Tự động chuyển đổi khi lỗi)
- Nếu một backend server down, request sẽ chuyển sang server còn hoạt động.
- Nếu 192.168.1.101 bị lỗi 3 lần trong 30 giây, Nginx sẽ bỏ qua và gửi toàn bộ request đến 192.168.1.102.
```
upstream backend {
    server 192.168.1.101 max_fails=3 fail_timeout=30s;
    server 192.168.1.102;
}
```

