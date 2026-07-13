# Module 8: Infrastructure as Code và Configuration

## Mục tiêu

- Quản lý hạ tầng bằng code thay vì thao tác tay.
- Hiểu Terraform state, plan, apply, module và drift.
- Dùng configuration management để chuẩn hóa máy chủ.

## Chủ đề chính

- IaC principles
- Terraform provider, resource, variable, output
- State file và remote state
- Module và workspace
- Drift detection
- Ansible inventory, playbook, role, idempotency
- Policy as code cơ bản

## Lab gợi ý

Viết Terraform tạo một hạ tầng giả lập hoặc cloud sandbox:

- Network
- Compute
- Security rule
- Output endpoint

Sau đó viết Ansible playbook cài một service đơn giản lên máy chủ.

## Câu hỏi ôn tập

- `terraform plan` giúp tránh lỗi gì?
- State file nhạy cảm ở điểm nào?
- Idempotency nghĩa là gì?
- Terraform và Ansible khác vai trò thế nào?

