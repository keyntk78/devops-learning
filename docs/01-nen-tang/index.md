# Module 1: Nền tảng DevOps

## Mục tiêu

- Hiểu DevOps không chỉ là công cụ, mà là cách rút ngắn vòng phản hồi giữa phát triển và vận hành.
- Nắm các khái niệm SDLC, CI, CD, automation, feedback, reliability và platform thinking.
- Biết đọc một delivery flow từ commit đến production.

## Chủ đề chính

- DevOps culture: ownership, collaboration, continuous improvement
- Software delivery lifecycle
- CI/CD và deployment strategy
- Environment: dev, test, staging, production
- Manual work, toil và automation
- Lead time, deployment frequency, change failure rate, MTTR

## Bài học

- [DevOps là gì?](devops-la-gi.md)
- [Tư duy triển khai](tu-duy-trien-khai.md)
- [Tư duy triển khai mọi dự án](tu-duy-trien-khai-moi-du-an.md)

## Lab gợi ý

Vẽ flow triển khai của một ứng dụng đơn giản:

```text
Developer -> Git -> CI -> Artifact -> Deploy -> Monitor -> Feedback
```

Sau đó ghi lại:

- Bước nào có thể tự động hóa?
- Bước nào cần approval?
- Bước nào dễ gây lỗi nhất?

## Câu hỏi ôn tập

- CI khác CD ở điểm nào?
- Vì sao DevOps quan tâm đến feedback loop?
- Toil là gì và khi nào nên tự động hóa?
- Một deployment tốt cần khả năng rollback như thế nào?
