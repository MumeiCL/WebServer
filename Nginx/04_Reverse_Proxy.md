# Reverse Proxy trong Nginx
- Reverse Proxy là một máy chủ trung gian đứng giữa client và server backend. Nó nhận request từ client, chuyển tiếp đến server nội bộ (backend), nhận phản hồi, và gửi lại cho client.
- Nginx thường được sử dụng làm reverse proxy để:
  - Cân bằng tải giữa nhiều backend.
  - Ẩn chi tiết nội bộ của hệ thống backend.
  - Caching để giảm tải cho backend.
  - Hỗ trợ SSL/TLS termination.
## Cấu hình Reverse Proxy đơn giản
- Khi client truy cập `example.com`, Nginx sẽ chuyển tiếp request đến `http://backend_server`.
- Các header như `X-Real-IP` giúp backend nhận biết IP thực của client.
- `X-Forwarded-For`: Gửi danh sách các IP client qua các proxy trung gian đến backend.
- `Host`: Giữ nguyên Host Header từ client khi chuyển tiếp đến backend. Nếu không có dòng này, Nginx sẽ thay Host bằng IP backend, gây lỗi trên một số ứng dụng web.
```
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend_server;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
    }
}
```

## Reverse Proxy với HTTPS (SSL Termination)
- Client kết nối đến Nginx qua HTTPS, còn Nginx chuyển tiếp request đến backend qua HTTP.
```
server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;

    location / {
        proxy_pass http://backend_server;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

server {                     #Chuyển hướng tất cả request HTTP (http://example.com) sang HTTPS (https://example.com).
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri;
}
```
