# Tư duy triển khai

## Mục tiêu

- Hiểu triển khai không chỉ là "đẩy code lên server".
- Biết nghĩ theo flow từ code, artifact, config, môi trường, database, release, rollback đến monitoring.
- Có checklist trước, trong và sau khi deploy.
- Biết đặt câu hỏi để giảm rủi ro trước khi đưa thay đổi ra production.

## Triển khai là gì?

Triển khai là quá trình đưa một phiên bản phần mềm từ nơi phát triển sang môi trường chạy thật hoặc gần thật.

Ví dụ flow đơn giản:

```text
Code -> Test -> Build artifact -> Configure -> Deploy -> Verify -> Monitor -> Feedback
```

Trong DevOps, deploy tốt không chỉ cần "chạy được", mà còn cần:

- Biết version nào đang chạy.
- Biết thay đổi gì được đưa lên.
- Có cách kiểm tra sau deploy.
- Có rollback nếu lỗi.
- Có log/metric/alert để phát hiện vấn đề.
- Có người chịu trách nhiệm rõ ràng.

## Deploy khác release thế nào?

Hai từ này hay bị dùng lẫn nhau.

| Khái niệm | Ý nghĩa | Ví dụ |
| --- | --- | --- |
| Deploy | Đưa code/artifact lên môi trường chạy | Đưa image `api:1.4.0` lên production |
| Release | Cho người dùng sử dụng tính năng | Bật feature flag `new_checkout=true` |

Có thể deploy trước nhưng chưa release ngay.

Ví dụ:

```text
Deploy code mới lên production -> Tính năng vẫn tắt -> Bật feature flag cho 5% user -> Theo dõi -> Bật cho 100%
```

Cách này giúp giảm rủi ro vì có thể tắt tính năng mà không cần deploy lại.

## Artifact là gì?

Artifact là kết quả build có thể lưu lại, kiểm tra và triển khai.

Ví dụ artifact:

- File `.jar` của Java app.
- Folder build của frontend.
- Docker image.
- Package `.deb`, `.rpm`.
- File binary đã compile.

Tư duy đúng:

```text
Một commit -> một artifact rõ ràng -> deploy artifact đó qua các môi trường
```

Không nên:

```text
SSH vào server -> git pull -> sửa file trực tiếp -> restart app
```

Vì cách này khó biết chính xác version nào đang chạy và khó rollback.

## Môi trường triển khai

Một hệ thống thường có nhiều môi trường.

| Môi trường | Mục đích | Ai dùng |
| --- | --- | --- |
| Local | Developer chạy trên máy cá nhân | Developer |
| Dev | Tích hợp thay đổi sớm | Developer/team |
| Test/QA | Kiểm thử chức năng | QA/team |
| Staging | Gần giống production | Team/reviewer |
| Production | Người dùng thật | End user |

Tư duy quan trọng:

- Môi trường càng gần production thì càng cần kiểm soát chặt.
- Staging nên giống production nhất có thể.
- Không sửa tay production nếu có thể tự động hóa.
- Config khác nhau theo môi trường, nhưng artifact nên giống nhau.

## Config và secret

Code không nên chứa config nhạy cảm hoặc config thay đổi theo môi trường.

Ví dụ không nên hard-code:

```text
DATABASE_PASSWORD=abc123
API_KEY=secret-key
PRODUCTION_DB_HOST=10.0.0.5
```

Nên tách:

| Loại | Ví dụ | Cách quản lý |
| --- | --- | --- |
| Config thường | `PORT=8080`, `LOG_LEVEL=info` | Environment variables, config file |
| Secret | DB password, API token, private key | Secret manager, CI/CD secrets, Kubernetes Secret |

Câu hỏi cần hỏi trước khi deploy:

- Secret có bị commit vào Git không?
- Config production đã đúng chưa?
- App có fail rõ ràng nếu thiếu config không?
- Ai có quyền xem/sửa secret?

## Database migration

Deploy app thường đi kèm thay đổi database.

Ví dụ:

```text
Thêm column users.phone_number
Đổi tên column orders.status
Tạo index mới cho bảng transactions
```

Rủi ro:

- App mới cần schema mới nhưng migration chưa chạy.
- App cũ không chạy được với schema mới.
- Migration lâu làm lock bảng.
- Rollback app dễ nhưng rollback database khó.

Tư duy an toàn:

1. Thay đổi database theo hướng tương thích ngược nếu có thể.
2. Tách migration nguy hiểm khỏi deploy app.
3. Backup trước thay đổi lớn.
4. Test migration trên dữ liệu gần giống production.
5. Có kế hoạch rollback hoặc forward fix.

Ví dụ an toàn hơn:

```text
Deploy 1: thêm column mới, app cũ vẫn chạy
Deploy 2: app mới ghi vào column mới
Deploy 3: sau khi ổn định, bỏ column cũ nếu cần
```

## Rollback

Rollback là quay lại phiên bản ổn định trước đó.

Trước khi deploy phải trả lời được:

- Phiên bản trước là gì?
- Artifact cũ còn lưu không?
- Rollback mất bao lâu?
- Database có rollback được không?
- Nếu không rollback được, kế hoạch forward fix là gì?

Ví dụ rollback container:

```text
api:1.4.1 bị lỗi -> quay về api:1.4.0
```

Nhưng nếu deploy `1.4.1` đã chạy migration phá vỡ schema cũ, quay app về `1.4.0` có thể vẫn lỗi. Vì vậy rollback không chỉ là đổi version app.

## Health check

Health check giúp biết app có còn sống và sẵn sàng nhận request không.

Hai loại hay gặp:

| Loại | Ý nghĩa |
| --- | --- |
| Liveness | App còn sống không? Nếu chết thì restart |
| Readiness | App đã sẵn sàng nhận traffic chưa? |

Ví dụ endpoint:

```text
GET /healthz
GET /readyz
```

Response mẫu:

```json
{
  "status": "ok",
  "database": "ok",
  "version": "1.4.0"
}
```

Giải thích:

- `status=ok`: app đang chạy.
- `database=ok`: app kết nối được database.
- `version=1.4.0`: biết version đang chạy.

## Monitoring sau deploy

Sau deploy, không nên đóng laptop đi ngủ ngay. Cần quan sát hệ thống.

Các chỉ số cần xem:

| Nhóm | Cần xem |
| --- | --- |
| Traffic | Request rate |
| Error | Error rate, HTTP 5xx |
| Latency | p95, p99 latency |
| Resource | CPU, memory, disk |
| Dependency | Database, cache, queue |
| Business | Login, checkout, payment, order |

Ví dụ dấu hiệu deploy lỗi:

```text
Error rate tăng từ 0.2% lên 8%
p95 latency tăng từ 200ms lên 2s
Log xuất hiện nhiều database timeout
CPU tăng 95%
```

Tư duy:

- Không chỉ hỏi "deploy thành công chưa?"
- Phải hỏi "người dùng có còn dùng ổn không?"

## Chiến lược triển khai

| Chiến lược | Cách hoạt động | Ưu điểm | Rủi ro |
| --- | --- | --- | --- |
| Recreate | Tắt bản cũ, bật bản mới | Đơn giản | Có downtime |
| Rolling | Thay từng instance | Ít downtime | Cần app tương thích nhiều version |
| Blue-Green | Chạy song song blue và green, chuyển traffic | Rollback nhanh | Tốn tài nguyên |
| Canary | Mở dần cho một phần nhỏ traffic | Giảm rủi ro | Cần monitoring tốt |
| Feature Flag | Deploy code nhưng bật/tắt tính năng bằng config | Release linh hoạt | Cần quản lý flag sạch |

Người mới không cần thuộc hết ngay. Quan trọng là hiểu câu hỏi:

```text
Nếu bản mới lỗi, người dùng bị ảnh hưởng bao nhiêu và mình quay lại bằng cách nào?
```

## Checklist trước khi deploy

- [ ] Code đã được review.
- [ ] Test quan trọng đã chạy.
- [ ] Artifact đã build và có version rõ ràng.
- [ ] Config đúng môi trường.
- [ ] Secret không nằm trong Git.
- [ ] Database migration đã được kiểm tra.
- [ ] Có backup nếu thay đổi dữ liệu quan trọng.
- [ ] Có kế hoạch rollback.
- [ ] Dashboard/log/alert đã sẵn sàng.
- [ ] Người liên quan biết thời điểm deploy.

## Checklist trong khi deploy

- [ ] Deploy đúng version.
- [ ] Theo dõi log realtime.
- [ ] Kiểm tra health check.
- [ ] Kiểm tra metric chính.
- [ ] Kiểm tra chức năng quan trọng.
- [ ] Không thực hiện nhiều thay đổi lớn cùng lúc nếu không cần.

## Checklist sau khi deploy

- [ ] Xác nhận version mới đang chạy.
- [ ] Error rate ổn định.
- [ ] Latency không tăng bất thường.
- [ ] Không có alert nghiêm trọng.
- [ ] Chức năng chính chạy được.
- [ ] Ghi lại kết quả deploy.
- [ ] Nếu có lỗi, quyết định rollback hoặc forward fix.

## Ví dụ tư duy triển khai cho `todo-api`

Giả sử có app `todo-api` cần deploy version `1.2.0`.

Thông tin release:

```text
App: todo-api
Version: 1.2.0
Change: thêm API cập nhật trạng thái task
Artifact: todo-api:1.2.0
Environment: staging -> production
```

Câu hỏi trước deploy:

| Câu hỏi | Trả lời mẫu |
| --- | --- |
| Artifact là gì? | Docker image `todo-api:1.2.0` |
| Có migration không? | Có, thêm column `tasks.status` |
| Có tương thích app cũ không? | Có, column mới nullable |
| Config mới là gì? | `TASK_STATUS_ENABLED=true` |
| Secret mới không? | Không |
| Health check là gì? | `GET /healthz` |
| Rollback thế nào? | Quay về image `todo-api:1.1.3`, tắt feature flag |
| Theo dõi gì? | 5xx, latency, database query error |

Flow deploy:

```text
Deploy migration tương thích -> Deploy app 1.2.0 staging -> Test smoke -> Deploy production canary -> Monitor -> Mở 100%
```

## Smoke test

Smoke test là kiểm tra nhanh các chức năng sống còn sau deploy.

Ví dụ với API:

```bash
curl -i https://api.example.com/healthz
curl -i https://api.example.com/todos
curl -i -X POST https://api.example.com/todos \
  -H "Content-Type: application/json" \
  -d '{"title":"test deploy"}'
```

Kết quả mong muốn:

```text
HTTP/2 200
```

hoặc khi tạo mới:

```text
HTTP/2 201
```

Giải thích:

- `200` nghĩa là request đọc thành công.
- `201` nghĩa là tạo resource thành công.
- Nếu nhận `500`, cần kiểm tra log server.
- Nếu nhận `401/403`, cần kiểm tra auth/config.
- Nếu timeout, cần kiểm tra network, load balancer, app readiness.

## Những lỗi tư duy thường gặp

| Lỗi | Hậu quả | Cách nghĩ đúng hơn |
| --- | --- | --- |
| Deploy là copy code lên server | Khó kiểm soát version | Deploy artifact có version |
| Chỉ test local | Lên staging/production dễ lỗi config | Test theo môi trường gần production |
| Không nghĩ rollback | Lỗi rồi mới hoảng | Luôn có rollback plan trước deploy |
| Migration làm cùng lúc quá lớn | Rollback khó, downtime | Chia migration nhỏ, tương thích ngược |
| Không monitor sau deploy | Lỗi âm thầm ảnh hưởng user | Theo dõi metric/log ngay sau deploy |
| Sửa tay production | Mất dấu thay đổi, khó lặp lại | Tự động hóa bằng pipeline/IaC |
| Deploy nhiều thay đổi cùng lúc | Khó biết lỗi do đâu | Release nhỏ, dễ quan sát |

## Lab: viết kế hoạch deploy

Chọn một app tưởng tượng, ví dụ `todo-api`, rồi tạo file:

```bash
mkdir -p ~/devops-lab/deployment-thinking
nano ~/devops-lab/deployment-thinking/deploy-plan.md
```

Nội dung mẫu:

```markdown
# Deploy Plan: todo-api 1.2.0

## Thay đổi

- 

## Artifact

- Tên artifact:
- Version:
- Commit:

## Môi trường

- Staging:
- Production:

## Config và secret

- Config mới:
- Secret mới:

## Database migration

- Có/không:
- Có tương thích ngược không:
- Backup cần thiết không:

## Cách deploy

1. 
2. 
3. 

## Smoke test

- 

## Monitoring

- Error rate:
- Latency:
- Log cần xem:

## Rollback

- Version quay lại:
- Cách rollback:
- Khi nào quyết định rollback:
```

Hoàn thành lab khi bạn trả lời được:

- App deploy version nào?
- Deploy lên môi trường nào?
- Cần config/secret gì?
- Có migration không?
- Kiểm tra sau deploy bằng gì?
- Nếu lỗi thì rollback thế nào?

## Câu hỏi ôn tập

- Deploy khác release như thế nào?
- Vì sao nên deploy artifact thay vì `git pull` trực tiếp trên server?
- Vì sao artifact nên giống nhau qua các môi trường?
- Secret nên được quản lý thế nào?
- Database migration nguy hiểm ở điểm nào?
- Rollback app khác rollback database như thế nào?
- Health check giúp gì khi deploy?
- Sau deploy nên theo dõi những metric nào?
- Canary deployment giảm rủi ro ra sao?
- Smoke test là gì?

## Liên kết nội bộ

- [DevOps là gì?](devops-la-gi.md)
- [CI/CD](../06-ci-cd/index.md)
- [Containers](../05-containers/index.md)
- [Observability](../10-observability/index.md)
- [SRE và Operations](../12-sre-operations/index.md)
