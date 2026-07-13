# Tư duy triển khai mọi dự án

## Mục tiêu

- Có một cách suy nghĩ dùng được khi nhận bất kỳ dự án nào: frontend, backend, fullstack, worker, cron job, static site, API, service nội bộ.
- Biết DevOps cần đọc gì trong source code trước khi deploy.
- Biết xác định công nghệ, version, dependency, file cấu hình, cách build, cách run, log, port, process và dữ liệu.
- Biết vì sao nên có thư mục riêng, user riêng và quy trình riêng cho từng dự án.
- Tạo được checklist triển khai để không deploy kiểu đoán mò.

## Ý chính từ ghi chú thô

DevOps thường không trực tiếp code chức năng như developer, nhưng phải hiểu dự án đủ để triển khai và vận hành.

DevOps cần quan tâm:

- Dự án là frontend, backend hay service loại khác?
- Công nghệ dùng là gì?
- Version runtime tương ứng là gì?
- File nào là code chức năng?
- File nào là cấu hình?
- File nào là tài liệu như `README`, `.env.example`, script?
- Build bằng lệnh gì?
- Run bằng lệnh gì?
- Dự án đặt ở thư mục nào?
- Chạy bằng user Linux nào?
- Log nằm ở đâu?
- Khi lỗi thì rollback thế nào?

Một câu rất quan trọng:

```text
Muốn triển khai được một dự án, trước tiên phải hiểu dự án đó cần gì để chạy ổn định.
```

## Vai trò của DevOps khi nhận một dự án

DevOps không nhất thiết phải viết toàn bộ chức năng nghiệp vụ, nhưng không được triển khai như một hộp đen.

DevOps cần hiểu:

| Cần hiểu | Vì sao quan trọng |
| --- | --- |
| App dùng công nghệ gì | Để cài đúng runtime và build đúng cách |
| App nghe port nào | Để cấu hình firewall, reverse proxy, load balancer |
| App cần biến môi trường gì | Để cấu hình đúng từng môi trường |
| App kết nối service nào | Để chuẩn bị DB, cache, queue, API key |
| App ghi log ở đâu | Để debug và đưa vào hệ thống giám sát |
| App build ra artifact gì | Để deploy đúng thứ cần chạy |
| App chạy bằng command nào | Để viết service, container, pipeline |
| App có migration không | Để tránh lỗi database khi release |
| App rollback thế nào | Để giảm thiệt hại khi bản mới lỗi |

Tư duy đúng:

```text
DevOps không chỉ hỏi "chạy lệnh gì?"
DevOps hỏi "để dự án này chạy an toàn, ổn định, có thể kiểm tra và rollback, cần những gì?"
```

## Mô hình chung của mọi dự án

Bất kỳ dự án nào cũng có thể nhìn theo mô hình:

```text
Source code
  -> Dependencies
  -> Runtime
  -> Configuration
  -> Build
  -> Artifact
  -> Run process
  -> Network
  -> Data
  -> Logs/Metrics
  -> Backup/Rollback
```

Giải thích:

| Thành phần | Câu hỏi cần trả lời |
| --- | --- |
| Source code | Code nằm ở đâu, branch nào, commit nào? |
| Dependencies | Cần package/library nào? Cài bằng gì? |
| Runtime | Cần Node/Python/Java/Go/Nginx version nào? |
| Configuration | Config nằm ở file nào, biến môi trường nào? |
| Build | Lệnh build là gì? Output build ra đâu? |
| Artifact | Thứ đem deploy là gì? Docker image, binary, folder `dist`, file `.jar`? |
| Run process | Chạy bằng command nào? Ai quản lý process? |
| Network | App nghe port nào? Route/domain nào trỏ vào? |
| Data | Có DB, cache, storage, queue không? |
| Logs/Metrics | Xem log và metric ở đâu? |
| Backup/Rollback | Nếu lỗi thì quay lại bằng cách nào? |

## Bước 1: xác định loại dự án

Trước khi cài tool, hãy xác định dự án thuộc loại nào.

| Loại dự án | Dấu hiệu thường gặp | Artifact thường gặp |
| --- | --- | --- |
| Frontend SPA | `package.json`, `vite`, `react`, `vue`, `angular` | Folder `dist` hoặc `build` |
| Backend API Node.js | `package.json`, `server.js`, `src`, `npm start` | Source + `node_modules` hoặc Docker image |
| Backend Python | `requirements.txt`, `pyproject.toml`, `app.py`, `manage.py` | Virtualenv/source hoặc Docker image |
| Java/Spring Boot | `pom.xml`, `build.gradle`, `src/main` | File `.jar` |
| Go service | `go.mod`, `main.go` | Binary |
| Static site | `index.html`, `assets`, `public` | Folder static |
| Worker/consumer | Không có HTTP route chính, xử lý queue/job | Process chạy nền |
| Cron job | Script chạy theo lịch | Script + cron/systemd timer |
| Fullstack | Có cả frontend/backend hoặc monorepo | Nhiều artifact |

Ví dụ khi thấy:

```text
package.json
vite.config.ts
src/App.tsx
```

Bạn có thể đoán đây là frontend Vite/React. Nhưng vẫn phải đọc `package.json` để biết script build/run thật sự.

## Bước 2: đọc các file quan trọng trong dự án

Khi nhận source code, đọc theo thứ tự này:

| File/thư mục | Cần tìm gì |
| --- | --- |
| `README.md` | Cách cài, build, run, test, deploy |
| `.env.example` | Các biến môi trường cần có |
| `package.json` | Script, dependency, Node version nếu có |
| `requirements.txt` | Python dependencies |
| `pyproject.toml` | Python project config |
| `pom.xml` | Java Maven dependency/build |
| `build.gradle` | Java/Gradle build |
| `go.mod` | Go module và version |
| `Dockerfile` | Cách đóng gói app |
| `docker-compose.yml` | Service phụ thuộc như DB/cache |
| `.github/workflows` | CI/CD pipeline hiện có |
| `nginx.conf` | Reverse proxy/static hosting |
| `k8s/`, `helm/` | Kubernetes manifests/chart |
| `terraform/` | Hạ tầng bằng code |
| `migrations/` | Database migration |

Lệnh đọc nhanh:

```bash
ls -la
find . -maxdepth 2 -type f | sort
```

Kết quả mẫu:

```text
./.env.example
./Dockerfile
./README.md
./docker-compose.yml
./package.json
./src/server.js
```

Giải thích:

- Có `package.json`: nhiều khả năng là Node.js project.
- Có `Dockerfile`: dự án có thể build thành Docker image.
- Có `.env.example`: app cần biến môi trường.
- Có `docker-compose.yml`: có thể có service phụ thuộc như database.

## Bước 3: xác định công nghệ và version

Deploy sai version là lỗi rất phổ biến.

Ví dụ:

- App cần Node.js 20 nhưng server chỉ có Node.js 16.
- App cần Python 3.12 nhưng server dùng Python 3.8.
- Java app build bằng JDK 21 nhưng runtime chỉ có JRE 11.

Các nơi cần kiểm tra version:

| Công nghệ | File/lệnh cần xem |
| --- | --- |
| Node.js | `package.json`, `.nvmrc`, `node -v`, `npm -v` |
| Python | `runtime.txt`, `.python-version`, `pyproject.toml`, `python --version` |
| Java | `pom.xml`, `build.gradle`, `java -version` |
| Go | `go.mod`, `go version` |
| Docker | `Dockerfile`, `docker version` |
| Database | Migration, README, compose file |

Ví dụ kiểm tra Node:

```bash
cat package.json
node -v
npm -v
```

Kết quả mẫu:

```text
v20.16.0
10.8.1
```

Giải thích:

- Server đang có Node.js `20.16.0`.
- Nếu dự án yêu cầu Node 20, version này phù hợp.
- Nếu dự án yêu cầu Node 22, cần nâng runtime hoặc dùng Docker.

## Bước 4: phân loại file trong dự án

Một dự án thường có nhiều loại file. DevOps phải biết file nào có vai trò gì.

| Loại file | Ví dụ | Vai trò |
| --- | --- | --- |
| File chức năng | `src/`, `app/`, `controllers/`, `services/` | Code nghiệp vụ |
| File cấu hình app | `.env`, `config.yml`, `settings.py` | Config runtime |
| File dependency | `package.json`, `requirements.txt`, `pom.xml` | Khai báo thư viện |
| File build | `Dockerfile`, `Makefile`, `vite.config.ts` | Cách build |
| File deploy | `docker-compose.yml`, `k8s/*.yaml`, `systemd.service` | Cách chạy trên môi trường |
| File migration | `migrations/`, `prisma/migrations/` | Thay đổi database |
| File tài liệu | `README.md`, `docs/` | Hướng dẫn và quyết định kỹ thuật |
| File ignore | `.gitignore`, `.dockerignore` | Loại trừ file không nên commit/build |

Tư duy:

- Không sửa code chức năng nếu không cần.
- Không commit `.env` chứa secret.
- Không deploy cả thư mục rác như `.git`, `node_modules` nếu build artifact không cần.
- Không bỏ qua README vì nhiều dự án ghi cách chạy ở đó.

## Bước 5: xác định dependency

Dependency là những thứ app cần để build hoặc run.

Ví dụ:

| Loại dependency | Ví dụ |
| --- | --- |
| Library/package | npm packages, pip packages, Maven dependencies |
| Runtime | Node, Python, Java, Go |
| System package | `nginx`, `ffmpeg`, `libpq-dev`, `curl` |
| Service phụ thuộc | PostgreSQL, Redis, RabbitMQ, Elasticsearch |
| External API | Payment gateway, email service, S3 |

Câu hỏi cần trả lời:

- Dependency cài bằng lệnh nào?
- Có lock file không?
- Dependency có version cố định không?
- Có dependency nào cần system package không?
- Dự án có cần database/cache/queue để chạy không?

Ví dụ Node.js:

```bash
npm ci
```

Giải thích:

- `npm ci` cài dependency theo lock file.
- Phù hợp cho CI/CD vì tái lập tốt hơn `npm install`.

Ví dụ Python:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Giải thích:

- Virtualenv cô lập dependency của dự án.
- Tránh cài package lẫn vào Python hệ thống.

## Bước 6: xác định config và secret

Config là giá trị thay đổi theo môi trường. Secret là config nhạy cảm.

Ví dụ config:

```text
PORT=8080
LOG_LEVEL=info
APP_ENV=production
```

Ví dụ secret:

```text
DATABASE_PASSWORD=...
JWT_SECRET=...
AWS_SECRET_ACCESS_KEY=...
```

Checklist:

- Có `.env.example` không?
- Biến nào bắt buộc?
- Biến nào là secret?
- Config dev/staging/prod khác nhau ở đâu?
- Secret được lưu ở đâu?
- App thiếu config thì fail rõ ràng hay lỗi âm thầm?

Không nên:

```text
Commit file .env thật lên Git
Hard-code password trong source code
Dùng chung secret cho dev và production
```

Nên:

```text
Git chỉ lưu .env.example
Secret lưu trong CI/CD secret, secret manager hoặc Kubernetes Secret
Mỗi môi trường có secret riêng
```

## Bước 7: xác định build

Build là bước biến source code thành artifact sẵn sàng deploy.

Câu hỏi cần trả lời:

- Lệnh build là gì?
- Build output nằm ở đâu?
- Build cần biến môi trường không?
- Build có chạy test không?
- Build có tạo artifact version rõ ràng không?

Ví dụ frontend:

```bash
npm ci
npm run build
ls -la dist
```

Kết quả mẫu:

```text
index.html
assets/
```

Giải thích:

- `dist` là artifact static.
- Có thể serve bằng nginx hoặc object storage/CDN.

Ví dụ Java:

```bash
./mvnw clean package
ls -la target/*.jar
```

Kết quả mẫu:

```text
target/demo-api-1.0.0.jar
```

Giải thích:

- File `.jar` là artifact để chạy.

Ví dụ Docker:

```bash
docker build -t demo-api:1.0.0 .
```

Giải thích:

- Artifact là Docker image `demo-api:1.0.0`.
- Image nên gắn tag theo version hoặc commit SHA.

## Bước 8: xác định run

Run là cách app chạy sau khi đã build/cài dependency.

Câu hỏi cần trả lời:

- Command chạy app là gì?
- App chạy foreground hay background?
- App nghe port nào?
- App chạy bằng user nào?
- Process được quản lý bằng gì?
- Khi server reboot, app có tự chạy lại không?

Ví dụ Node.js:

```bash
npm start
```

Ví dụ Java:

```bash
java -jar target/demo-api-1.0.0.jar
```

Ví dụ Docker:

```bash
docker run -d --name demo-api -p 8080:8080 demo-api:1.0.0
```

Ví dụ systemd:

```ini
[Unit]
Description=Demo API
After=network.target

[Service]
User=demo-api
WorkingDirectory=/opt/demo-api/current
EnvironmentFile=/etc/demo-api/demo-api.env
ExecStart=/usr/bin/node /opt/demo-api/current/server.js
Restart=always

[Install]
WantedBy=multi-user.target
```

Giải thích:

- `User=demo-api`: chạy bằng user riêng của dự án.
- `WorkingDirectory`: thư mục chạy app.
- `EnvironmentFile`: file biến môi trường.
- `ExecStart`: command chạy app.
- `Restart=always`: app tự restart nếu crash.

## Bước 9: thư mục riêng cho từng dự án

Không nên rải file lung tung trên server.

Một cấu trúc dễ quản lý:

```text
/opt/demo-api/
├── releases/
│   ├── 2026-07-14-001/
│   └── 2026-07-14-002/
├── current -> /opt/demo-api/releases/2026-07-14-002
├── shared/
│   ├── logs/
│   └── uploads/
└── backups/

/etc/demo-api/
└── demo-api.env

/var/log/demo-api/
└── app.log
```

Ý nghĩa:

| Đường dẫn | Vai trò |
| --- | --- |
| `/opt/demo-api/releases` | Chứa các phiên bản deploy |
| `/opt/demo-api/current` | Symlink trỏ tới bản đang chạy |
| `/opt/demo-api/shared` | Dữ liệu dùng chung giữa các release |
| `/etc/demo-api` | Config của app |
| `/var/log/demo-api` | Log của app |
| `/opt/demo-api/backups` | Backup hoặc artifact cũ |

Lợi ích:

- Dễ biết version nào đang chạy.
- Dễ rollback bằng cách đổi symlink `current`.
- Config tách khỏi code.
- Log có vị trí rõ ràng.
- Không lẫn nhiều dự án trên cùng server.

## Bước 10: user riêng cho từng dự án

Không nên chạy app bằng `root` nếu không cần.

Tạo user riêng:

```bash
sudo useradd --system --home /opt/demo-api --shell /usr/sbin/nologin demo-api
```

Giải thích:

- `--system`: user hệ thống cho service.
- `--home /opt/demo-api`: home/working area của service.
- `--shell /usr/sbin/nologin`: user này không dùng để login tương tác.
- `demo-api`: tên user theo tên dự án.

Phân quyền thư mục:

```bash
sudo mkdir -p /opt/demo-api /etc/demo-api /var/log/demo-api
sudo chown -R demo-api:demo-api /opt/demo-api /var/log/demo-api
sudo chown root:demo-api /etc/demo-api
sudo chmod 750 /etc/demo-api
```

Giải thích:

- App user sở hữu thư mục app và log.
- Config trong `/etc/demo-api` do root quản lý, group `demo-api` đọc được.
- Không cho user khác đọc config nếu không cần.

Tư duy bảo mật:

```text
Mỗi dự án chỉ có quyền đúng với việc nó cần làm.
```

Nếu app bị hack, attacker chỉ có quyền của user dự án đó, không có toàn quyền root.

## Bước 11: network, port và domain

Mỗi app chạy thường cần một hoặc nhiều port.

Câu hỏi cần trả lời:

- App listen port nào?
- Port đó chỉ mở nội bộ hay mở public?
- Có reverse proxy không?
- Domain/subdomain nào trỏ vào app?
- TLS/HTTPS cấu hình ở đâu?
- Health check dùng path nào?

Ví dụ:

```text
App listen: 127.0.0.1:3000
Nginx listen: 443 public
Domain: api.example.com
Health: /healthz
```

Tư duy:

- Backend thường không cần mở trực tiếp ra Internet nếu đã có Nginx/load balancer.
- Public nên đi qua reverse proxy hoặc load balancer.
- Health check cần ổn định, nhanh và không phụ thuộc quá nhiều logic nghiệp vụ.

## Bước 12: log, monitor và debug

Trước khi deploy production, phải biết xem log ở đâu.

Câu hỏi:

- App log ra stdout hay file?
- Nếu dùng systemd, xem log bằng `journalctl` thế nào?
- Nếu dùng Docker, xem log bằng `docker logs` thế nào?
- Log có request id/correlation id không?
- Có metric health/error/latency không?

Ví dụ systemd:

```bash
sudo systemctl status demo-api
sudo journalctl -u demo-api -f
```

Ví dụ Docker:

```bash
docker ps
docker logs -f demo-api
```

Ví dụ kiểm tra port:

```bash
sudo ss -lntp | grep 3000
curl -i http://127.0.0.1:3000/healthz
```

Kết quả mẫu:

```text
HTTP/1.1 200 OK
```

Giải thích:

- App đang phản hồi health check.
- Nếu port không listen, kiểm tra process.
- Nếu port listen nhưng health fail, kiểm tra log app/config/dependency.

## Bước 13: dữ liệu và file upload

Không phải mọi dữ liệu đều nằm trong source code.

Các loại dữ liệu cần chú ý:

| Loại | Ví dụ | Cần làm gì |
| --- | --- | --- |
| Database | PostgreSQL, MySQL, MongoDB | Migration, backup, connection string |
| Cache | Redis | Không lưu dữ liệu quan trọng nếu cache có thể mất |
| Queue | RabbitMQ, Kafka, SQS | Theo dõi backlog/dead letter |
| Upload | Ảnh, tài liệu người dùng | Lưu ngoài thư mục release |
| Local state | File tạm, session file | Biết vị trí và cách dọn |

Nguyên tắc:

```text
Code có thể thay nhanh, dữ liệu người dùng thì không được làm mất.
```

Vì vậy:

- Không để upload nằm trong thư mục release nếu mỗi lần deploy xóa release cũ.
- Backup database trước thay đổi lớn.
- Migration phải được test.
- Rollback app không có nghĩa là rollback dữ liệu.

## Bước 14: checklist triển khai mọi dự án

### A. Nhận diện dự án

- [ ] Dự án tên gì?
- [ ] Là frontend, backend, fullstack, worker, cron hay static site?
- [ ] Repo/branch/commit nào?
- [ ] Ai là owner kỹ thuật?
- [ ] README có hướng dẫn chạy không?

### B. Công nghệ và version

- [ ] Dùng runtime gì?
- [ ] Version runtime yêu cầu là gì?
- [ ] Có lock file dependency không?
- [ ] Có Dockerfile không?
- [ ] Có docker-compose để chạy local không?

### C. File và cấu hình

- [ ] File code chính nằm ở đâu?
- [ ] File config nằm ở đâu?
- [ ] Có `.env.example` không?
- [ ] Biến môi trường bắt buộc là gì?
- [ ] Secret lưu ở đâu?
- [ ] Có file migration không?

### D. Build

- [ ] Lệnh cài dependency là gì?
- [ ] Lệnh test là gì?
- [ ] Lệnh build là gì?
- [ ] Build output nằm ở đâu?
- [ ] Artifact cuối cùng là gì?
- [ ] Artifact có version/tag không?

### E. Run

- [ ] Lệnh run là gì?
- [ ] App nghe port nào?
- [ ] App cần service phụ thuộc nào?
- [ ] Chạy bằng user nào?
- [ ] Quản lý process bằng systemd, Docker, Kubernetes hay tool khác?
- [ ] Khi reboot app có tự chạy lại không?

### F. Server layout

- [ ] Thư mục dự án nằm ở đâu?
- [ ] Config nằm ở đâu?
- [ ] Log nằm ở đâu?
- [ ] Upload/shared data nằm ở đâu?
- [ ] Permission đã đúng chưa?
- [ ] User dự án có quyền tối thiểu chưa?

### G. Network

- [ ] Domain/subdomain là gì?
- [ ] Reverse proxy/load balancer cấu hình ở đâu?
- [ ] TLS/HTTPS đã có chưa?
- [ ] Health check path là gì?
- [ ] Port public/private đã đúng chưa?

### H. Verify và rollback

- [ ] Smoke test sau deploy là gì?
- [ ] Metric/log nào cần xem?
- [ ] Version trước là gì?
- [ ] Rollback bằng cách nào?
- [ ] Database migration có rollback được không?
- [ ] Khi nào quyết định rollback?

## Ví dụ 1: triển khai frontend

Dấu hiệu:

```text
package.json
vite.config.ts
src/
public/
```

Cần xác định:

| Câu hỏi | Ví dụ trả lời |
| --- | --- |
| Runtime build | Node.js 20 |
| Cài dependency | `npm ci` |
| Build | `npm run build` |
| Artifact | `dist/` |
| Run | Nginx serve static hoặc upload CDN |
| Config | `VITE_API_URL` |
| Verify | Mở trang, kiểm tra API call, kiểm tra console error |

Flow:

```text
npm ci -> npm run build -> deploy dist/ -> reload nginx/CDN -> smoke test
```

Lỗi thường gặp:

- Build bằng sai Node version.
- Quên biến môi trường API URL.
- Deploy thiếu folder `assets`.
- Browser cache giữ bản cũ.
- API bị CORS.

## Ví dụ 2: triển khai backend API

Dấu hiệu:

```text
package.json
src/server.js
.env.example
Dockerfile
migrations/
```

Cần xác định:

| Câu hỏi | Ví dụ trả lời |
| --- | --- |
| Runtime | Node.js 20 hoặc Docker |
| Cài dependency | `npm ci --omit=dev` |
| Build | Không cần hoặc `npm run build` |
| Run | `node dist/server.js` |
| Port | `3000` |
| Config | `DATABASE_URL`, `JWT_SECRET`, `PORT` |
| Dependency | PostgreSQL, Redis |
| Verify | `/healthz`, login, API chính |

Flow:

```text
Build image -> Run migration -> Deploy app -> Health check -> Smoke test -> Monitor
```

Lỗi thường gặp:

- Thiếu biến môi trường.
- Database chưa migrate.
- Port bị chiếm.
- Service chạy bằng root.
- Log không đi đâu cả nên khó debug.

## Ví dụ 3: triển khai worker/cron

Worker khác API vì có thể không mở port HTTP.

Cần xác định:

| Câu hỏi | Ví dụ |
| --- | --- |
| Worker đọc từ đâu? | Queue `emails` |
| Worker ghi vào đâu? | Database, email provider |
| Chạy liên tục hay theo lịch? | systemd service hoặc cron |
| Làm sao biết nó sống? | Log, metric heartbeat, queue backlog |
| Nếu lỗi job thì sao? | Retry, dead letter queue |

Tư duy:

- Không có port không có nghĩa là không cần monitoring.
- Với worker, cần quan sát queue backlog, số job fail, retry count.
- Cron cần log rõ lần chạy gần nhất và exit code.

## Bảng câu hỏi nhanh khi nhận dự án mới

| Nhóm | Câu hỏi |
| --- | --- |
| Mục đích | Dự án này làm gì, phục vụ ai? |
| Loại app | FE, BE, fullstack, worker, cron, static site? |
| Runtime | Cần Node/Python/Java/Go version nào? |
| Build | Lệnh build là gì, output ở đâu? |
| Run | Lệnh run là gì, port nào? |
| Config | Biến môi trường bắt buộc là gì? |
| Secret | Secret lưu ở đâu, ai được xem? |
| Data | Có DB/cache/queue/upload không? |
| Network | Domain, reverse proxy, TLS thế nào? |
| Process | systemd, Docker, Kubernetes hay PM2? |
| User | Chạy bằng user riêng chưa? |
| Directory | Code/config/log/data nằm ở đâu? |
| Verify | Health check và smoke test là gì? |
| Observe | Log/metric/alert xem ở đâu? |
| Rollback | Quay lại version trước bằng cách nào? |

## Template ghi chú triển khai dự án

Khi nhận một dự án mới, tạo file:

```bash
mkdir -p ~/devops-lab/project-deploy-thinking
nano ~/devops-lab/project-deploy-thinking/project-profile.md
```

Nội dung:

```markdown
# Project Deploy Profile

## 1. Thông tin chung

- Tên dự án:
- Repo:
- Branch:
- Commit/tag:
- Owner:
- Loại dự án: FE/BE/fullstack/worker/cron/static

## 2. Công nghệ

- Runtime:
- Version:
- Package manager:
- Framework:
- Database/cache/queue:

## 3. File quan trọng

- README:
- File dependency:
- File config mẫu:
- Dockerfile:
- Compose/Kubernetes/systemd:
- Migration:

## 4. Build

- Cài dependency:
- Chạy test:
- Build:
- Artifact:

## 5. Run

- Command:
- Port:
- Process manager:
- User chạy app:
- Working directory:

## 6. Config và secret

- Biến môi trường:
- Secret:
- Nơi lưu secret:

## 7. Data

- Database:
- Migration:
- Upload/shared files:
- Backup:

## 8. Network

- Domain:
- Reverse proxy:
- TLS:
- Health check:

## 9. Verify

- Smoke test:
- Log cần xem:
- Metric cần xem:

## 10. Rollback

- Version trước:
- Cách rollback:
- Khi nào rollback:
```

## Lab: phân tích một dự án bất kỳ

Chọn một repo nhỏ hoặc app demo, rồi trả lời:

1. Dự án thuộc loại gì?
2. Công nghệ và version là gì?
3. File dependency là file nào?
4. File config mẫu nằm ở đâu?
5. Lệnh build là gì?
6. Artifact là gì?
7. Lệnh run là gì?
8. App nghe port nào?
9. Cần database/cache/queue không?
10. Log xem ở đâu?
11. Dự án nên nằm ở thư mục nào trên server?
12. Nên chạy bằng user nào?
13. Health check là gì?
14. Smoke test sau deploy là gì?
15. Rollback thế nào?

Nếu chưa có repo thật, dùng app giả lập:

```text
Tên: demo-api
Loại: Backend API
Runtime: Node.js 20
Build: npm ci && npm run build
Run: node dist/server.js
Port: 3000
Config: PORT, DATABASE_URL, JWT_SECRET
Artifact: Docker image demo-api:<commit-sha>
User: demo-api
Directory: /opt/demo-api
Log: journalctl -u demo-api
Health: GET /healthz
Rollback: quay về image tag trước
```

## Lỗi tư duy thường gặp

| Lỗi | Hậu quả | Cách nghĩ đúng |
| --- | --- | --- |
| Chỉ hỏi "lệnh chạy là gì?" | Chạy được nhưng không vận hành được | Hỏi đủ build, config, log, data, rollback |
| Cài tool theo cảm giác | Sai version, lỗi khó debug | Đọc file dự án và xác định version |
| Chạy app bằng root | Rủi ro bảo mật cao | Tạo user riêng cho dự án |
| Đặt file lung tung | Khó backup, khó rollback | Có layout thư mục chuẩn |
| Config nằm trong source | Lộ secret, khó đổi môi trường | Tách config/secret khỏi code |
| Không biết artifact là gì | Deploy không lặp lại được | Build artifact có version |
| Không biết log ở đâu | Lỗi không debug được | Chuẩn hóa log trước deploy |
| Không có health check | Không biết app sẵn sàng chưa | Tạo endpoint/command kiểm tra |
| Không có rollback plan | Lỗi production kéo dài | Chuẩn bị rollback trước deploy |

## Câu hỏi ôn tập

- Vì sao DevOps không cần code hết chức năng nhưng vẫn phải hiểu dự án?
- Khi nhận dự án mới, bạn đọc những file nào trước?
- Vì sao phải xác định runtime version?
- File chức năng, file cấu hình, file dependency khác nhau thế nào?
- Build khác run như thế nào?
- Artifact là gì trong frontend, backend, Docker?
- Vì sao mỗi dự án nên có thư mục riêng?
- Vì sao mỗi dự án nên có user riêng?
- Khi app lỗi sau deploy, bạn kiểm tra log/process/port bằng lệnh nào?
- Vì sao không nên deploy bằng cách sửa tay trực tiếp trên production?
- Nếu dự án có database migration, bạn cần hỏi gì trước khi deploy?
- Một project deploy profile tốt cần có những phần nào?

## Liên kết nội bộ

- [Tư duy triển khai](tu-duy-trien-khai.md)
- [DevOps là gì?](devops-la-gi.md)
- [Quyền truy cập trong Linux](../02-linux-shell/quyen-truy-cap-trong-linux.md)
- [Các lệnh cơ bản trong Linux](../02-linux-shell/cac-lenh-co-ban.md)
- [CI/CD](../06-ci-cd/index.md)
- [Containers](../05-containers/index.md)
