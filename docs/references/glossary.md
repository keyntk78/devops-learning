# Glossary

| Thuật ngữ | Giải thích ngắn | Liên kết |
| --- | --- | --- |
| CI | Continuous Integration, tự động kiểm tra thay đổi code sớm và thường xuyên | [CI/CD](../06-ci-cd/index.md) |
| CD | Continuous Delivery hoặc Continuous Deployment, tự động chuẩn bị hoặc triển khai release | [CI/CD](../06-ci-cd/index.md) |
| Artifact | Kết quả build có thể lưu, kiểm tra và triển khai | [CI/CD](../06-ci-cd/index.md) |
| Deploy | Đưa artifact/code lên môi trường chạy | [Tư duy triển khai](../01-nen-tang/tu-duy-trien-khai.md) |
| Release | Cho người dùng sử dụng một thay đổi hoặc tính năng | [Tư duy triển khai](../01-nen-tang/tu-duy-trien-khai.md) |
| Rollback | Quay lại phiên bản hoặc trạng thái ổn định trước đó | [Tư duy triển khai](../01-nen-tang/tu-duy-trien-khai.md) |
| Smoke test | Kiểm tra nhanh các chức năng sống còn sau deploy | [Tư duy triển khai](../01-nen-tang/tu-duy-trien-khai.md) |
| Runtime | Môi trường/chương trình cần để app chạy, ví dụ Node.js, Python, Java | [Tư duy triển khai mọi dự án](../01-nen-tang/tu-duy-trien-khai-moi-du-an.md) |
| Project deploy profile | Hồ sơ tóm tắt cách build, run, config, deploy và rollback của một dự án | [Tư duy triển khai mọi dự án](../01-nen-tang/tu-duy-trien-khai-moi-du-an.md) |
| Nginx | Web server/reverse proxy phổ biến để nhận request và chuyển vào app phía sau | [Nginx cơ bản](../03-networking/nginx-co-ban.md) |
| Reverse proxy | Thành phần đứng trước app, nhận request từ client rồi chuyển tiếp vào upstream | [Nginx cơ bản](../03-networking/nginx-co-ban.md) |
| Upstream | App/server phía sau mà reverse proxy chuyển request tới | [Nginx cơ bản](../03-networking/nginx-co-ban.md) |
| DNS | Hệ thống phân giải domain thành IP | [DNS cơ bản](../03-networking/dns-co-ban.md) |
| IP address | Địa chỉ của máy/interface trong mạng | [IP, port, subnet và NAT](../03-networking/ip-port-subnet-nat.md) |
| Port | Số định danh service/app trên một máy | [IP, port, subnet và NAT](../03-networking/ip-port-subnet-nat.md) |
| NAT | Cơ chế chuyển đổi địa chỉ mạng, thường giúp private network đi ra Internet | [IP, port, subnet và NAT](../03-networking/ip-port-subnet-nat.md) |
| TLS | Lớp bảo mật mã hóa kết nối, dùng trong HTTPS | [HTTP, HTTPS và TLS](../03-networking/http-https-tls.md) |
| Load balancer | Thành phần chia traffic tới nhiều backend instance | [Proxy, reverse proxy và load balancer](../03-networking/proxy-load-balancer.md) |
| Container | Môi trường chạy ứng dụng được đóng gói cùng dependency cần thiết | [Containers](../05-containers/index.md) |
| IaC | Infrastructure as Code, mô tả hạ tầng bằng code | [IaC](../08-iac-configuration/index.md) |
| SLI | Chỉ số đo chất lượng dịch vụ từ góc nhìn người dùng | [SRE](../12-sre-operations/index.md) |
| SLO | Mục tiêu chất lượng dịch vụ dựa trên SLI | [SRE](../12-sre-operations/index.md) |
| MTTR | Mean Time To Recovery, thời gian trung bình để khôi phục sau sự cố | [SRE](../12-sre-operations/index.md) |
