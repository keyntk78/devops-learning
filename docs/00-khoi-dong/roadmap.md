# Lộ trình học DevOps

## Cấp độ 1: Nền móng

Mục tiêu là dùng được terminal, hiểu đường đi của một request và biết cộng tác bằng Git.

- DevOps mindset, SDLC, CI/CD, automation, feedback loop
- Linux filesystem, permissions, process, service, logs
- Shell scripting cơ bản
- Networking căn bản: IP, port, DNS, HTTP, TLS
- Git branch, merge, rebase, PR, tag, release

## Cấp độ 2: Build và deploy

Mục tiêu là đóng gói ứng dụng, kiểm thử tự động và triển khai có quy trình.

- Dockerfile, image layer, container runtime
- Docker Compose cho môi trường local
- CI pipeline: build, test, lint, artifact
- CD pipeline: environment, approval, rollback
- Secret và biến môi trường

## Cấp độ 3: Hạ tầng hiện đại

Mục tiêu là quản lý hạ tầng bằng code và chạy workload trên cloud/Kubernetes.

- Cloud primitives: IAM, compute, storage, network, database
- Terraform: provider, resource, state, module
- Ansible: inventory, playbook, role
- Kubernetes: pod, deployment, service, ingress, configmap, secret, volume
- Helm và GitOps cơ bản

## Cấp độ 4: Vận hành production

Mục tiêu là quan sát, bảo mật và cải thiện độ tin cậy của hệ thống.

- Logs, metrics, traces
- Alert, dashboard, runbook
- SLI, SLO, error budget
- Incident response và postmortem
- Container security, dependency scanning, least privilege

## Dự án cuối khóa

Triển khai một web app hoàn chỉnh:

- App có Dockerfile và Compose
- Pipeline CI/CD tự động
- Infrastructure as Code
- Deploy lên cloud hoặc Kubernetes local
- Monitoring dashboard và alert cơ bản
- Runbook và postmortem mẫu

