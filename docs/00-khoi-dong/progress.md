# Theo dõi tiến độ

## Checklist tổng

| Module               | Trạng thái   | Ghi chú           |
| -------------------- | ------------ | ----------------- |
| Khởi động            | Đang học     | Khung wiki đã tạo |
| Nền tảng DevOps      | Đang học     | Đã thêm DevOps là gì, tư duy triển khai và framework triển khai mọi dự án |
| Linux và Shell       | Đang học     | Đã thêm WSL, hệ thống file, lệnh cơ bản và quyền truy cập |
| Networking           | Đang học     | Đã hoàn thiện bộ bài networking cơ bản và lab tổng hợp |
| Git Collaboration    | Chưa bắt đầu |                   |
| Containers           | Chưa bắt đầu |                   |
| CI/CD                | Chưa bắt đầu |                   |
| Cloud                | Chưa bắt đầu |                   |
| IaC và Configuration | Chưa bắt đầu |                   |
| Kubernetes           | Chưa bắt đầu |                   |
| Observability        | Chưa bắt đầu |                   |
| Security             | Chưa bắt đầu |                   |
| SRE và Operations    | Chưa bắt đầu |                   |
| Projects             | Đang học     | Đã thêm 2 lab deploy Next.js và NestJS không Docker |

## Nhật ký học

| Ngày | Chủ đề | Đã làm | Cần ôn lại |
| --- | --- | --- | --- |
| 2026-07-13 | DevOps là gì? | Tạo ghi chú bài đầu tiên, flow DevOps cơ bản, khái niệm CI/CD/automation/feedback/rollback | Tự trả lời câu hỏi ôn tập và vẽ flow cho app ví dụ |
| 2026-07-14 | Tư duy triển khai | Tạo bài học về artifact, environment, config/secret, migration, rollback, health check, monitoring, deployment strategy và checklist deploy | Làm lab viết `deploy-plan.md` cho app `todo-api` |
| 2026-07-14 | Tư duy triển khai mọi dự án | Viết lại ghi chú thô thành framework phân tích dự án: loại app, công nghệ/version, file dự án, dependency, config/secret, build, run, user riêng, thư mục riêng, log, network, data và rollback | Làm template `project-profile.md` cho một repo bất kỳ |
| 2026-07-13 | Tạo Linux server bằng WSL | Ghi hướng dẫn WSL là gì, vì sao dùng WSL thay VMware, cách setup Ubuntu WSL để thực hành | Chạy lab `hello-server.sh` và thử quản lý SSH service |
| 2026-07-13 | Hệ thống file trong Linux | Tách thành bài riêng về cây thư mục, đường dẫn tuyệt đối/tương đối và WSL mount | Làm lab khám phá `/`, `/etc`, `/var/log`, `~` |
| 2026-07-13 | Các lệnh cơ bản trong Linux | Tách thành bài riêng có công dụng từng lệnh, trường hợp dùng, ví dụ, output mẫu và giải thích | Thực hành lab đi một vòng các lệnh cơ bản trong WSL |
| 2026-07-14 | Quyền truy cập trong Linux | Viết lại ghi chú thô thành bài đầy đủ về user, group, `/etc/passwd`, `/etc/shadow`, `/etc/group`, chmod, chown, chgrp, umask, ACL và lab | Làm lab tạo user/group demo và debug `Permission denied` |
| 2026-07-14 | Lab deploy không Docker | Tạo 2 lab: deploy frontend Next.js và backend NestJS trực tiếp trên Linux bằng user riêng, thư mục riêng, systemd và Nginx | Thực hành trên WSL/server demo, ghi lỗi gặp phải và cách rollback |
| 2026-07-14 | Nginx cơ bản | Tạo bài học về Nginx, reverse proxy, server block, location, proxy_pass, log, lỗi 502/404/403 và lab proxy app local | Làm lab Nginx reverse proxy trước khi quay lại lab deploy Next/Nest |
| 2026-07-14 | Networking cơ bản | Tạo bộ bài request flow, IP/port/subnet/NAT, DNS, HTTP/HTTPS/TLS, proxy/load balancer, công cụ debug và lab tổng hợp request flow | Làm lab tổng hợp, viết `report.md`, sau đó quay lại lab deploy Next/Nest |

## Kỹ năng cần chứng minh

- [ ] Giải thích được DevOps lifecycle.
- [ ] Debug được service Linux cơ bản.
- [ ] Viết được Bash script nhỏ.
- [x] Phân tích được request HTTP qua DNS, TCP, TLS.
- [ ] Tạo Dockerfile tốt cho một app.
- [ ] Thiết kế CI pipeline có test và artifact.
- [ ] Viết Terraform module nhỏ.
- [ ] Deploy workload lên Kubernetes.
- [ ] Tạo dashboard và alert cơ bản.
- [ ] Viết runbook xử lý incident.
