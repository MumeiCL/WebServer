# Tổng quan về Nginx
NGINX là một phần mềm mã nguồn mở được dùng như là một web server, reverse proxy, caching, load balancing, media streaming, ... Ban đầu, nó được thiết kế để làm web server có hiệu suất và độ ổn định cao.
Ngoài khả năng giao tiếp bằng HTTP, NGINX còn có thể hoạt động như là một email server (IMAP, POP3, SMTP), reverse proxy, load balancer cho các server dùng HTTP, TCP và UDP.
## Lịch sử và phát triển
- Được phát triển bởi Igor Sysoev vào năm 2004 với mục tiêu xử lý hàng nghìn kết nối đồng thời một cách hiệu quả.
- Ban đầu, nó được tạo ra để giải quyết vấn đề C10k (xử lý 10.000 kết nối cùng lúc).
- Hiện nay, Nginx là một trong những web server phổ biến nhất trên thế giới, cạnh tranh với Apache và sử dụng rộng rãi trong các hệ thống lớn như Netflix, Airbnb, và Cloudflare.
## Đặc điểm nổi bật của Nginx
- Hiệu suất cao: Sử dụng kiến trúc sự kiện bất đồng bộ (asynchronous event-driven), giúp Nginx xử lý nhiều kết nối đồng thời mà không tiêu tốn quá nhiều tài nguyên.
- Nhẹ và nhanh: So với Apache, Nginx tiêu thụ ít RAM hơn và có tốc độ phản hồi nhanh hơn trong nhiều tình huống.
- Hỗ trợ proxy ngược: Dùng làm reverse proxy cho các ứng dụng backend như PHP, Node.js, Python, Go, v.v.
- Cân bằng tải: Phân phối lưu lượng đến nhiều backend server để tối ưu tài nguyên.
- Hỗ trợ HTTP/2 và TLS: Tăng tốc độ tải trang và bảo mật tốt hơn.
- Dễ cấu hình và mở rộng: Sử dụng file cấu hình đơn giản, dễ dàng tùy chỉnh theo nhu cầu.
## Các lệnh cơ bản 
### Cài đặt Nginx 
**Trên Ubuntu/Debian**
```
sudo apt update
sudo apt install nginx -y
```
**Trên CentOS/RHEL**
```
sudo yum install epel-release -y
sudo yum install nginx -y
```
**Kiểm tra phiên bản Nginx**
`nginx -v`

### Quản lý Nginx
```
sudo systemctl start nginx   # Khởi động Nginx
sudo systemctl stop nginx    # Dừng Nginx
sudo systemctl restart nginx # Khởi động lại Nginx
sudo systemctl reload nginx  # Tải lại cấu hình mà không dừng dịch vụ
sudo systemctl status nginx  # Kiểm tra trạng thái Nginx
```

## Các file/thư mục quan trọng trong Nginx
- `/etc/nginx/nginx.conf` File cấu hình chính của Nginx. Chứa các thiết lập chung như worker processes, log, gzip, và các block cấu hình khác.
- `/etc/nginx/sites-available/` Chứa các file cấu hình website (virtual host). File trong thư mục này chưa được Nginx sử dụng trực tiếp.
- `/etc/nginx/sites-enabled/` Chứa các site đã được kích hoạt bằng cách tạo symlink từ sites-available.
  - Kích hoạt site bằng lệnh: `sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/`
  - Gỡ kích hoạt site: `sudo rm /etc/nginx/sites-enabled/example.com`
- `/etc/nginx/conf.d/` Trên CentOS/RHEL, Nginx đọc file .conf trong thư mục này để load cấu hình site.
- `/etc/nginx/snippets/` Dùng để lưu các đoạn cấu hình dùng chung như cài đặt SSL, bảo mật…
- `/var/log/nginx/access.log` Ghi lại các request từ client.
  - Kiểm tra bằng lệnh: `sudo tail -f /var/log/nginx/access.log`
- `/var/log/nginx/error.log` Ghi lại lỗi của Nginx.
  - Kiểm tra bằng lệnh: `sudo tail -f /var/log/nginx/error.log`
- Thư mục chứa file web (document root)
  - /var/www/html/ (Ubuntu/Debian)
  - /usr/share/nginx/html/ (CentOS/RHEL)
