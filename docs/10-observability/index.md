# Module 10: Observability

## Mục tiêu

- Biết hệ thống đang khỏe hay không dựa trên dữ liệu.
- Phân biệt logs, metrics và traces.
- Tạo dashboard, alert và quy trình phản hồi sự cố.

## Chủ đề chính

- Logs: structured logs, correlation id
- Metrics: counter, gauge, histogram
- Traces: span, trace id, latency breakdown
- RED metrics: rate, errors, duration
- USE metrics: utilization, saturation, errors
- Alert fatigue và actionable alerts
- Dashboard cho service

## Lab gợi ý

Thiết kế dashboard cho API:

- Request rate
- Error rate
- Latency p95/p99
- CPU/memory
- Database latency
- Alert khi error rate vượt ngưỡng

## Câu hỏi ôn tập

- Log khác metric ở điểm nào?
- Vì sao alert cần action rõ ràng?
- p95 latency nói điều gì?
- Correlation id giúp debug ra sao?

