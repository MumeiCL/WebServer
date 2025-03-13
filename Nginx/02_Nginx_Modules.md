# Các module trong Nginx
## Tổng quan
- Nginx sử dụng kiến trúc mô-đun để mở rộng chức năng mà không cần thay đổi lõi phần mềm. Mỗi mô-đun có thể cung cấp tính năng riêng như xử lý HTTP, proxy, bảo mật, hoặc nén dữ liệu.
  - Mô-đun tích hợp (Built-in Modules): Được biên dịch sẵn trong Nginx.
  - Mô-đun động (Dynamic Modules): Có thể tải và gỡ bỏ khi cần mà không cần biên dịch lại Nginx.
## Các loại mô-đun chính trong Nginx
### Core Modules
**Các mô-đun cốt lõi xử lý hoạt động chính của Nginx.**
|Mô-đun|	Mô tả|
|-------|-------
|ngx_core_module|	Quản lý cấu hình chung, tiến trình worker, log|
|ngx_events_module	|Quản lý sự kiện và kết nối|
|ngx_http_module	|Kích hoạt HTTP server trong Nginx|

### HTTP Modules
**Các mô-đun xử lý request HTTP, caching, logging, bảo mật, proxy...**
|Mô-đun|Mô tả|
|------|-----|
|ngx_http_proxy_module	|Cho phép Nginx hoạt động như một reverse proxy|
|ngx_http_upstream_module|	Cấu hình load balancing giữa các backend|
|ngx_http_fastcgi_module|	Hỗ trợ giao thức FastCGI (thường dùng với PHP)|
|ngx_http_uwsgi_module|	Hỗ trợ giao thức uWSGI (thường dùng với Python)|
|ngx_http_scgi_module	|Hỗ trợ giao thức SCGI|
|ngx_http_gzip_module	|Bật nén Gzip để giảm băng thông|
|ngx_http_brotli_module	|Hỗ trợ nén Brotli (cần cài đặt thêm)|
|ngx_http_headers_module	|Kiểm soát header HTTP|
|ngx_http_cache_purge_module	|Xóa cache Nginx dễ dàng|
|ngx_http_rewrite_module	|Cho phép viết lại URL bằng rewrite hoặc return|
|ngx_http_map_module|	Tạo mapping giữa các giá trị|
|ngx_http_geo_module	|Kiểm tra vị trí địa lý của client|
|ngx_http_ssl_module|	Hỗ trợ SSL/TLS|
|ngx_http_auth_basic_module	|Xác thực bằng HTTP Basic Auth|
|ngx_http_limit_conn_module	|Giới hạn số kết nối đồng thời|
|ngx_http_limit_req_module	|Giới hạn tốc độ request|

### Stream Modules
**Dùng để xử lý TCP/UDP proxy.**
|Mô-đun|	Mô tả|
|------|-------|
|ngx_stream_core_module	|Hỗ trợ proxy TCP và UDP|
|ngx_stream_proxy_module|	Cho phép Nginx làm reverse proxy TCP/UDP|
|ngx_stream_ssl_module|	Hỗ trợ SSL/TLS trên kết nối TCP|

### Mail Modules
**Hỗ trợ proxy cho các giao thức mail như SMTP, IMAP, POP3.**
|Mô-đun|	Mô tả|
|------|-------|
|ngx_mail_core_module|	Hỗ trợ proxy email|
|ngx_mail_ssl_module	|Bật SSL/TLS cho email proxy|
|ngx_mail_auth_http_module	|Xác thực người dùng qua HTTP|

### Third-party Modules
**Ngoài các mô-đun mặc định, Nginx hỗ trợ thêm mô-đun của bên thứ ba để mở rộng tính năng.**
|Mô-đun	|Chức năng|
|-------|---------|
|ngx_pagespeed	|Tăng tốc website bằng Google PageSpeed|
|ngx_brotli|	Hỗ trợ nén Brotli|
|nginx-rtmp-module	|Hỗ trợ phát trực tuyến RTMP|

## Kiểm tra và cài đặt mô-đun trong Nginx
### Kiểm tra mô-đun đã bật
`nginx -V 2>&1 | grep --color=auto module`
### Biên dịch lại Nginx để thêm mô-đun
**Một số mô-đun không có sẵn và cần biên dịch lại Nginx.**
```
./configure --with-http_ssl_module --with-stream
make
sudo make install
```
### Cài đặt mô-đun bên thứ ba
** Cài đặt Brotli trên Ubuntu**
```
sudo apt install brotli
nginx -V 2>&1 | grep brotli
```
