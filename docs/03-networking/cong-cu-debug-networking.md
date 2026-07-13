# Công cụ debug networking

## Mục tiêu

- Biết dùng công cụ debug theo từng tầng: DNS, IP, port, TLS, HTTP, proxy.
- Không debug ngẫu nhiên; biết chọn lệnh theo câu hỏi đang cần trả lời.

## Bảng chọn công cụ

| Câu hỏi | Công cụ |
| --- | --- |
| Domain ra IP nào? | `nslookup`, `dig`, `Resolve-DnsName` |
| Máy có route ra ngoài không? | `ping`, `traceroute`, `ip route` |
| Port có mở không? | `nc`, `telnet`, `Test-NetConnection`, `ss` |
| Server trả HTTP gì? | `curl -i`, `curl -I` |
| TLS certificate có hợp lệ không? | `curl -vI`, `openssl s_client` |
| App có listen không? | `ss -lntp`, `netstat -lntp` |
| Nginx lỗi gì? | `nginx -t`, `error.log`, `access.log` |

## `nslookup` và `dig`

```bash
nslookup example.com
dig example.com A
```

Kết quả mẫu:

```text
example.com. 300 IN A 93.184.216.34
```

Giải thích:

- Domain resolve ra IP `93.184.216.34`.
- TTL là `300`.

## `ping`

```bash
ping -c 4 example.com
```

Kết quả mẫu:

```text
4 packets transmitted, 4 received, 0% packet loss
```

Giải thích:

- Có phản hồi ICMP.
- Nếu ping fail, chưa chắc web fail vì nhiều server chặn ICMP.

## `traceroute` / `tracert`

Linux:

```bash
traceroute example.com
```

Windows:

```powershell
tracert example.com
```

Dùng khi:

- Muốn biết request đi qua những hop nào.
- Debug mất mạng/route bất thường.

## `ss`

```bash
sudo ss -lntp
```

Kết quả mẫu:

```text
LISTEN 0 511 127.0.0.1:3000 users:(("node",pid=1234,fd=22))
```

Giải thích:

- App Node đang listen local port `3000`.
- Nếu không thấy port, app chưa chạy hoặc bind port khác.

## `curl`

Header only:

```bash
curl -I https://example.com
```

Header + body:

```bash
curl -i https://example.com
```

Verbose:

```bash
curl -vI https://example.com
```

Gửi header:

```bash
curl -i https://api.example.com/me \
  -H "Authorization: Bearer TOKEN"
```

Gửi JSON:

```bash
curl -i -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Ada"}'
```

## `nc` và `telnet`

Kiểm tra port TCP:

```bash
nc -vz api.example.com 443
```

Hoặc:

```bash
telnet api.example.com 443
```

Kết quả tốt:

```text
Connection succeeded
```

Nếu fail:

- Port không mở.
- Firewall chặn.
- Server không listen.
- Sai IP/domain.

## `openssl s_client`

Kiểm tra TLS:

```bash
openssl s_client -connect example.com:443 -servername example.com
```

Dùng khi:

- Cert sai domain.
- Cert hết hạn.
- Chain lỗi.
- Debug SNI.

## PowerShell trên Windows

```powershell
Resolve-DnsName example.com
Test-NetConnection example.com -Port 443
Invoke-WebRequest https://example.com -Method Head
```

`Test-NetConnection` rất tiện để kiểm tra port từ Windows.

## Quy trình debug chuẩn

```text
1. DNS: domain có ra IP đúng không?
2. Port: IP/domain có mở port không?
3. TLS: HTTPS cert có hợp lệ không?
4. HTTP: status code/header/body là gì?
5. Proxy: Nginx/LB có log lỗi không?
6. App: service có chạy và listen port không?
7. Data: DB/cache/queue có lỗi không?
```

## Lab nhỏ

Chọn một domain:

```bash
DOMAIN=example.com
nslookup $DOMAIN
curl -I https://$DOMAIN
curl -vI https://$DOMAIN
```

Nếu dùng Linux local có Nginx/app:

```bash
sudo ss -lntp
sudo nginx -t
sudo tail -n 50 /var/log/nginx/error.log
```

Ghi lại:

- Domain ra IP nào?
- Port 443 có kết nối được không?
- Status code là gì?
- Server header là gì?
- Có lỗi TLS không?

## Câu hỏi ôn tập

- Khi nào dùng `nslookup`, khi nào dùng `curl`?
- `ss -lntp` trả lời câu hỏi gì?
- `curl -I` khác `curl -i` thế nào?
- `ping` fail có chắc website chết không?
- Khi gặp `502`, nên dùng những lệnh nào?

## Liên kết nội bộ

- [Request flow tổng quan](request-flow-tong-quan.md)
- [DNS cơ bản](dns-co-ban.md)
- [Nginx cơ bản](nginx-co-ban.md)

