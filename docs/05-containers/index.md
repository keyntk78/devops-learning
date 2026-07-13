# Module 5: Containers

## Mục tiêu

- Hiểu container giải quyết vấn đề "chạy được trên máy tôi" như thế nào.
- Viết Dockerfile cho ứng dụng đơn giản.
- Dùng Docker Compose để chạy nhiều service local.

## Chủ đề chính

- Image, container, registry
- Dockerfile: `FROM`, `WORKDIR`, `COPY`, `RUN`, `CMD`, `ENTRYPOINT`
- Layer caching và image size
- Volume, network, environment variables
- Docker Compose
- Healthcheck và logs

## Lab gợi ý

Đóng gói một web app đơn giản:

- Viết Dockerfile
- Build image
- Run container
- Expose port
- Xem logs
- Thêm Compose với database

## Câu hỏi ôn tập

- Image khác container thế nào?
- Vì sao không nên chạy container bằng user root nếu không cần?
- Multi-stage build dùng để làm gì?
- Volume giải quyết vấn đề gì?

