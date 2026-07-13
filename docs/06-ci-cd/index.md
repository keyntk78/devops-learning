# Module 6: CI/CD

## Mục tiêu

- Thiết kế pipeline tự động từ commit đến artifact.
- Phân biệt build, test, scan, package và deploy.
- Biết thêm approval, environment và rollback vào quy trình.

## Chủ đề chính

- CI trigger: push, pull request, tag
- Job, step, runner, artifact, cache
- Test pyramid và quality gate
- Deployment strategy: rolling, blue-green, canary
- Environment variables và secrets
- Rollback và release notes

## Đọc trước

- [Tư duy triển khai](../01-nen-tang/tu-duy-trien-khai.md)

## Lab gợi ý

Tạo pipeline cho app demo:

- Checkout code
- Install dependencies
- Run tests
- Build Docker image
- Scan image
- Push artifact
- Deploy vào môi trường test

## Câu hỏi ôn tập

- Artifact là gì?
- Pipeline fail ở bước test thì nên làm gì?
- Vì sao secret không được hard-code trong repo?
- Canary deployment giảm rủi ro như thế nào?
