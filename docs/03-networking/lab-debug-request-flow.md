# Lab tổng hợp: debug request flow

## Mục tiêu

- Thực hành toàn bộ Networking cơ bản theo một flow giống khi debug production.
- Kiểm tra DNS, port, HTTP, TLS, Nginx, app local và lỗi `502`.
- Ghi lại kết quả như một runbook nhỏ.

## Phần A: điều tra một website thật

Chọn một domain, ví dụ:

```bash
DOMAIN=example.com
```

### 1. Kiểm tra DNS

```bash
nslookup $DOMAIN
dig $DOMAIN A
```

Ghi lại:

- IP trả về là gì?
- TTL là bao nhiêu?
- Có nhiều IP không?

### 2. Kiểm tra HTTP/HTTPS

```bash
curl -I http://$DOMAIN
curl -I https://$DOMAIN
curl -vI https://$DOMAIN
```

Ghi lại:

- HTTP có redirect sang HTTPS không?
- HTTPS trả status code gì?
- TLS verify OK không?
- Header `server`, `content-type`, `cache-control` là gì?

### 3. Kiểm tra route

```bash
ping -c 4 $DOMAIN
traceroute $DOMAIN
```

Ghi lại:

- Ping có packet loss không?
- Traceroute đi qua bao nhiêu hop?
- Nếu ping fail nhưng curl vẫn OK, giải thích vì sao.

## Phần B: dựng app local và proxy bằng Nginx

### 1. Chạy app local

```bash
mkdir -p ~/devops-lab/networking-final-lab
cd ~/devops-lab/networking-final-lab
echo "Hello networking lab" > index.html
python3 -m http.server 3000 --bind 127.0.0.1
```

Mở terminal khác:

```bash
curl http://127.0.0.1:3000
```

Kết quả:

```text
Hello networking lab
```

### 2. Tạo Nginx reverse proxy

```bash
sudo nano /etc/nginx/sites-available/networking-final-lab
```

Nội dung:

```nginx
server {
    listen 8080;
    server_name _;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Enable:

```bash
sudo ln -sfn /etc/nginx/sites-available/networking-final-lab /etc/nginx/sites-enabled/networking-final-lab
sudo nginx -t
sudo systemctl reload nginx
```

Test:

```bash
curl -i http://127.0.0.1:8080
```

Kết quả mong muốn:

```text
HTTP/1.1 200 OK
Hello networking lab
```

### 3. Quan sát port và log

```bash
sudo ss -lntp | grep -E '3000|8080'
sudo tail -n 20 /var/log/nginx/access.log
sudo tail -n 20 /var/log/nginx/error.log
```

Giải thích:

- Port `3000`: app Python.
- Port `8080`: Nginx.
- Access log có request bạn vừa gọi.
- Error log nên không có lỗi mới.

## Phần C: tạo lỗi 502 để hiểu cách debug

Dừng app Python bằng `Ctrl+C`.

Gọi lại:

```bash
curl -i http://127.0.0.1:8080
```

Kết quả mẫu:

```text
HTTP/1.1 502 Bad Gateway
```

Debug:

```bash
sudo ss -lntp | grep 3000
sudo tail -n 50 /var/log/nginx/error.log
```

Giải thích:

- Port `3000` không còn listen.
- Nginx vẫn chạy nhưng upstream app chết.
- Vì vậy Nginx trả `502`.

Chạy lại app Python, rồi test lại:

```bash
python3 -m http.server 3000 --bind 127.0.0.1
curl -i http://127.0.0.1:8080
```

## Phần D: viết báo cáo lab

Tạo file:

```bash
nano ~/devops-lab/networking-final-lab/report.md
```

Mẫu:

```markdown
# Networking Final Lab Report

## Domain thật đã kiểm tra

- Domain:
- IP:
- DNS TTL:
- HTTP status:
- HTTPS status:
- TLS verify:
- Header đáng chú ý:

## App local

- App port:
- Nginx port:
- Nginx config:
- Kết quả curl qua app:
- Kết quả curl qua Nginx:

## Lỗi 502

- Cách tạo lỗi:
- Dấu hiệu:
- Lệnh debug đã dùng:
- Nguyên nhân:
- Cách khôi phục:

## Bài học rút ra

- 
```

## Checklist hoàn thành

- [ ] Dùng được `nslookup` hoặc `dig`.
- [ ] Dùng được `curl -I`, `curl -i`, `curl -vI`.
- [ ] Hiểu status code `200`, `301/302`, `502`.
- [ ] Dựng được app local port `3000`.
- [ ] Dựng được Nginx reverse proxy port `8080`.
- [ ] Xem được port bằng `ss -lntp`.
- [ ] Xem được Nginx access/error log.
- [ ] Tạo và giải thích được lỗi `502`.
- [ ] Viết được report lab.

## Câu hỏi ôn tập

- Request đi qua những bước nào từ domain tới app?
- DNS trả lời câu hỏi gì?
- Port listen nghĩa là gì?
- Nginx reverse proxy làm gì trong lab?
- Vì sao app chết nhưng Nginx vẫn chạy lại trả `502`?
- Khi deploy Next/Nest gặp `502`, bạn sẽ kiểm tra theo thứ tự nào?

## Liên kết nội bộ

- [Request flow tổng quan](request-flow-tong-quan.md)
- [Nginx cơ bản](nginx-co-ban.md)
- [Công cụ debug networking](cong-cu-debug-networking.md)
- [Deploy backend NestJS không Docker](../13-projects/1.%20lab-deploy-nestjs-khong-docker.md)
- [Deploy frontend Next.js không Docker](../13-projects/2.%20lab-deploy-nextjs-khong-docker.md)

