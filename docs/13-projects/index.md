# Module 13: Projects

## Mục tiêu

- Ghép toàn bộ kiến thức thành dự án có thể đưa vào portfolio.
- Chứng minh được năng lực build, deploy, observe và operate.

## Labs triển khai không Docker

- [Deploy frontend Next.js không Docker](2.%20lab-deploy-nextjs-khong-docker.md)
- [Deploy backend NestJS không Docker](1.%20lab-deploy-nestjs-khong-docker.md)

## Project 1: Dockerized App

Yêu cầu:

- App chạy được bằng Dockerfile
- Có Compose cho app và database
- Có healthcheck
- Có README hướng dẫn chạy local

## Project 2: CI/CD Pipeline

Yêu cầu:

- Pipeline chạy test
- Build artifact hoặc image
- Scan dependency hoặc image
- Deploy vào môi trường test
- Có rollback note

## Project 3: Kubernetes Deployment

Yêu cầu:

- Manifest hoặc Helm chart
- Namespace, Deployment, Service, Ingress
- ConfigMap, Secret, probe
- Dashboard basic hoặc logs/metrics hướng dẫn

## Project 4: Production Runbook

Yêu cầu:

- SLO cho service
- Dashboard cần xem
- Alert rule
- Runbook incident
- Postmortem mẫu

## Portfolio checklist

- [ ] Repo có README rõ ràng.
- [ ] Có sơ đồ kiến trúc.
- [ ] Có pipeline chạy được.
- [ ] Có script hoặc IaC tái tạo môi trường.
- [ ] Có tài liệu vận hành.
- [ ] Có ghi chú các trade-off kỹ thuật.
