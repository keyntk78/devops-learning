# DevOps là gì?

## Mục tiêu

- Hiểu DevOps là cách kết hợp phát triển phần mềm và vận hành hệ thống để đưa sản phẩm ra production nhanh hơn, ổn định hơn và ít lỗi hơn.
- Nhìn được flow cơ bản từ lúc viết code đến lúc người dùng sử dụng.
- Phân biệt DevOps mindset với việc chỉ học một nhóm công cụ.

## Bức tranh lớn

Trước đây, developer thường viết code xong rồi chuyển cho team vận hành deploy. Nếu production lỗi, hai bên dễ bị tách trách nhiệm: bên code nói "máy chạy khác", bên vận hành nói "code có vấn đề".

DevOps cố gắng giảm khoảng cách đó bằng cộng tác, tự động hóa, đo lường và phản hồi nhanh. Một team DevOps tốt không chỉ deploy nhanh, mà còn biết hệ thống đang khỏe hay không, lỗi ở đâu, rollback thế nào và cải thiện quy trình sau mỗi lần gặp sự cố.

## Flow cơ bản

```text
Developer -> Git -> CI -> Build -> Deploy -> Monitor -> Feedback
```

Giải thích ngắn:

| Bước | Ý nghĩa |
| --- | --- |
| Developer | Viết code, sửa lỗi, thêm tính năng |
| Git | Lưu lịch sử thay đổi và phối hợp với người khác |
| CI | Tự động kiểm tra code sau mỗi thay đổi |
| Build | Tạo artifact hoặc container image có thể triển khai |
| Deploy | Đưa phiên bản mới lên môi trường chạy thật |
| Monitor | Theo dõi logs, metrics, alert và trải nghiệm người dùng |
| Feedback | Dùng dữ liệu thực tế để sửa lỗi và cải thiện hệ thống |

## DevOps không chỉ là công cụ

DevOps có nhiều công cụ như Git, Docker, Kubernetes, Terraform, Prometheus, Grafana, GitHub Actions. Nhưng bản chất DevOps không phải là học thật nhiều tool rời rạc.

Câu hỏi quan trọng hơn là:

- Công cụ này giúp giảm lỗi thủ công ở bước nào?
- Nó giúp phát hiện lỗi sớm hơn hay muộn hơn?
- Nó giúp rollback, quan sát hoặc bảo mật tốt hơn không?
- Nếu production lỗi, mình dùng nó để tìm nguyên nhân như thế nào?

## Ví dụ thực tế

Nếu chưa có DevOps tốt:

- Deploy bằng tay, mỗi người làm một kiểu.
- Không biết chính xác version nào đang chạy.
- Lỗi chỉ được phát hiện khi người dùng báo.
- Rollback chậm hoặc không có quy trình.
- Log phân tán, khó tìm nguyên nhân.

Nếu có DevOps tốt:

- Pipeline tự động test và build.
- Mỗi release có version rõ ràng.
- Deploy có checklist, approval và rollback.
- Có logs, metrics, alert để phát hiện lỗi sớm.
- Sau sự cố có postmortem để cải thiện hệ thống.

## Khái niệm chính

| Khái niệm | Giải thích bằng lời của mình | Ví dụ |
| --- | --- | --- |
| CI | Tự động kiểm tra code thường xuyên | Push code lên Git thì pipeline chạy test |
| CD | Tự động chuẩn bị hoặc triển khai release | Build image xong deploy lên staging |
| Automation | Giảm thao tác tay lặp lại | Script deploy thay vì copy file thủ công |
| Feedback loop | Vòng phản hồi để biết thay đổi có tốt không | Alert báo API lỗi sau release |
| Rollback | Quay lại phiên bản ổn định trước đó | Release mới lỗi thì quay về image cũ |

## Lab nhỏ

Mục tiêu: tự vẽ flow triển khai của một ứng dụng web đơn giản.

Các bước:

1. Chọn một app tưởng tượng, ví dụ `todo-api`.
2. Vẽ flow từ developer push code đến production.
3. Đánh dấu bước nào có thể tự động hóa.
4. Đánh dấu bước nào cần kiểm tra hoặc approval.
5. Ghi rủi ro nếu bước đó làm thủ công.

Mẫu:

```text
Code -> GitHub -> CI test -> Docker build -> Push image -> Deploy staging -> Approval -> Deploy production -> Monitor
```

## Lỗi hiểu nhầm thường gặp

| Hiểu nhầm                           | Cách nghĩ đúng hơn                                                                                               |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| DevOps là một người làm hết mọi thứ | DevOps là cách làm việc và hệ thống thực hành; có thể có vai trò DevOps engineer nhưng không phải người gánh hết |
| Biết Kubernetes là biết DevOps      | Kubernetes chỉ là một phần trong vận hành container                                                              |
| CI/CD chỉ để deploy nhanh           | CI/CD còn giúp kiểm tra, chuẩn hóa và giảm rủi ro release                                                        |
| Monitoring chỉ cần khi hệ thống lớn | Hệ thống nhỏ cũng cần logs và dấu hiệu sức khỏe cơ bản                                                           |

## Câu hỏi ôn tập

- DevOps giải quyết vấn đề gì giữa development và operations?
- CI giúp phát hiện lỗi sớm như thế nào?
- Vì sao deploy thủ công dễ gây lỗi?
- Feedback loop là gì?
- Nếu production lỗi sau deploy, bạn muốn kiểm tra những thông tin nào đầu tiên?
- Rollback quan trọng vì sao?
- Vì sao DevOps không nên được hiểu là chỉ học tool?

## Liên kết nội bộ

- [Lộ trình học DevOps](../00-khoi-dong/roadmap.md)
- [Cách học và ghi chép](../00-khoi-dong/how-to-study.md)
- [CI/CD](../06-ci-cd/index.md)
- [Observability](../10-observability/index.md)
- [SRE và Operations](../12-sre-operations/index.md)

