# Configuration Files
## Directive trong Nginx
**Directive là các lệnh trong file cấu hình Nginx (nginx.conf), dùng để thiết lập hành vi của Nginx.**
Ví dụ:
```
worker_processes 4;
server_name example.com;
listen 80;
```
- `worker_processes 4;` → Định nghĩa số lượng worker process.
- `server_name example.com;` → Xác định tên miền của server.
- `listen 80;` → Lắng nghe trên cổng 80.
## Context
**Context là phạm vi mà directive có thể được sử dụng. Một số directive chỉ hợp lệ trong một context nhất định.**
|Context	|Mô tả	                                            |Chứa directive nào?|
|---------|---------------------------------------------------|-------------------|
|Main	    |Context cấp cao nhất, áp dụng cho toàn bộ Nginx.  	|worker_processes, error_log…|
|Events	  |Quản lý cách Nginx xử lý kết nối.	                |worker_connections, use…|
|HTTP	    |Chứa các cài đặt liên quan đến HTTP server.	      |server, gzip, log_format…|
|Server	  |Xác định từng website hoặc domain.	                |listen, server_name, location…|
|Location	|Xác định cách xử lý từng URL hoặc đường dẫn cụ thể.|	proxy_pass, root, index…|
|Upstream	|Định nghĩa backend server khi dùng reverse proxy.	|server, keepalive…|

Ví dụ:
```
worker_processes auto;  # Main context

events {
    worker_connections 1024;  # Events context
}

http {
    include mime.types;  # HTTP context
    sendfile on;

    server {
        listen 80;  # Server context
        server_name example.com;

        location / {
            root /var/www/html;  # Location context
            index index.html;
        }
    }
}
```
## Variables
### Client Variables
| Biến | Mô tả | Ví dụ |
|------|-------|-------|
| `$remote_addr` | Địa chỉ IP của client | `192.168.1.1` |
| `$remote_port` | Cổng kết nối của client | `54321` |
| `$remote_user` | Người dùng đã xác thực (nếu có) | `admin` |
| `$http_user_agent` | User-Agent của client | `"Mozilla/5.0 (Windows NT 10.0)"` |
| `$http_referer` | URL referrer của request | `https://google.com` |

### Request Variables
| Biến | Mô tả | Ví dụ |
|------|-------|-------|
| `$request` | Request đầy đủ | `GET /index.html HTTP/1.1` |
| `$request_method` | Phương thức request | `GET`, `POST` |
| `$request_uri` | URI đầy đủ với query string | `/page.php?id=10` |
| `$uri` | URI không có query string | `/page.php` |
| `$args` | Query string của request | `id=10&name=test` |
| `$query_string` | Tương tự `$args` | `id=10&name=test` |
| `$request_body` | Nội dung request (dùng cho POST) | `username=admin&password=123` |
| `$request_length` | Độ dài toàn bộ request (header + body) | `512` |

### Server Variables
| Biến | Mô tả | Ví dụ |
|------|-------|-------|
| `$server_name` | Tên server xử lý request | `example.com` |
| `$server_addr` | Địa chỉ IP của server | `192.168.1.100` |
| `$server_port` | Cổng server đang lắng nghe | `80` |
| `$server_protocol` | Phiên bản giao thức HTTP | `HTTP/1.1` |

### Path & File Variables
| Biến | Mô tả | Ví dụ |
|------|-------|-------|
| `$document_root` | Thư mục gốc của website | `/var/www/html` |
| `$request_filename` | Đường dẫn thực tế của file được yêu cầu | `/var/www/html/index.html` |
| `$realpath_root` | Đường dẫn tuyệt đối thực tế của thư mục gốc | `/var/www/html` |
| `$path_info` | Phần path sau script trong request | `/extra/path` |

### Connection Variables
| Biến | Mô tả | Ví dụ |
|------|-------|-------|
| `$connection` | ID của kết nối hiện tại | `12345` |
| `$connection_requests` | Số request trên kết nối hiện tại | `10` |
| `$time_local` | Thời gian hiện tại trên server (local time) | `10/Mar/2025:15:30:00 +0700` |
| `$msec` | Timestamp chính xác đến mili-giây | `1647890123.456` |

### Proxy Variables
| Biến | Mô tả | Ví dụ |
|------|-------|-------|
| `$proxy_host` | Tên host của proxy đang kết nối | `backend.example.com` |
| `$proxy_port` | Cổng proxy đang kết nối | `8080` |
| `$proxy_add_x_forwarded_for` | Địa chỉ IP của client thực (X-Forwarded-For) | `192.168.1.1` |
| `$http_x_forwarded_for` | Địa chỉ IP client nếu qua proxy | `192.168.1.1, 10.0.0.1` |

### SSL Variables
| Biến | Mô tả | Ví dụ |
|------|-------|-------|
| `$ssl_protocol` | Giao thức SSL sử dụng | `TLSv1.3` |
| `$ssl_cipher` | Thuật toán mã hóa SSL | `ECDHE-RSA-AES256-GCM-SHA384` |
| `$ssl_session_id` | ID của phiên SSL | `3ABCD456EFG` |
| `$ssl_client_cert` | Chứng chỉ client gửi lên | `-----BEGIN CERTIFICATE-----` |

### Upstream Variables
| Biến | Mô tả | Ví dụ |
|------|-------|-------|
| `$upstream_addr` | Địa chỉ backend server đã kết nối | `192.168.1.2:8080` |
| `$upstream_status` | HTTP status trả về từ backend | `200` |
| `$upstream_response_time` | Thời gian phản hồi từ backend | `0.250` |
| `$upstream_connect_time` | Thời gian kết nối đến backend | `0.100` |
