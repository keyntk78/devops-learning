# Agent Guide For DevOps Wiki

Bạn là agent hỗ trợ người học DevOps trong repository này. Trước khi thêm hoặc sửa nội dung, hãy đọc:

- `README.md`
- `docs/index.md`
- `docs/00-khoi-dong/roadmap.md`
- `docs/00-khoi-dong/progress.md`
- `docs/agent/context.md`

## Nguyên tắc ghi chép

- Viết bằng tiếng Việt, rõ ràng, thực hành được.
- Ưu tiên giải thích bằng ví dụ, lệnh, sơ đồ tư duy dạng Markdown và checklist.
- Không biến wiki thành tài liệu quá dài. Mỗi trang nên có mục tiêu, ý chính, ví dụ/lệnh, lỗi thường gặp, câu hỏi ôn tập và liên kết liên quan.
- Khi chưa chắc một thông tin có còn đúng với công cụ hiện tại hay không, hãy kiểm chứng trước hoặc đánh dấu là cần kiểm chứng.
- Sau khi thêm bài học mới, cập nhật `docs/00-khoi-dong/progress.md` và thêm liên kết từ trang module tương ứng.
- Không xóa ghi chú cũ của người học. Nếu cần sửa lớn, giữ lại nội dung hữu ích hoặc chuyển vào mục "Ghi chú cũ".

## Minh họa trực quan

- Chủ động thêm minh họa khi chủ đề có flow, kiến trúc, quan hệ thành phần, vòng đời request, pipeline, permission, network path, deployment strategy hoặc quy trình debug.
- Ưu tiên Mermaid trong Markdown cho sơ đồ flow, sequence, state, architecture đơn giản vì dễ sửa và lưu cùng tài liệu.
- Có thể đề xuất hoặc tạo Excalidraw khi cần sơ đồ kiểu phác thảo, so sánh nhiều thành phần, hoặc muốn giải thích bằng hình tự do hơn.
- Có thể tìm ảnh minh họa trên mạng khi cần ví dụ thực tế về giao diện công cụ, dashboard, kiến trúc cloud, Kubernetes object, network diagram hoặc ảnh sản phẩm/công cụ. Khi dùng ảnh từ web, ghi nguồn/link và tránh dùng ảnh không liên quan hoặc chỉ trang trí.
- Không thêm hình cho có. Hình phải giúp người học hiểu nhanh hơn, ví dụ: "request đi qua DNS -> Load Balancer -> App -> Database", "CI/CD pipeline", "Linux permission rwx", "blue-green deployment".
- Khi bài có sơ đồ, đặt ngay sau phần "Bức tranh lớn" hoặc trước lab để người học nhìn được toàn cảnh trước khi làm lệnh.

## Cấu trúc trang bài học

Mỗi bài học nên bám theo `docs/templates/note.md`:

1. Mục tiêu
2. Bức tranh lớn
3. Sơ đồ minh họa nếu phù hợp
4. Khái niệm chính
5. Lệnh hoặc ví dụ
6. Lab nhỏ
7. Lỗi thường gặp
8. Câu hỏi ôn tập
9. Liên kết nội bộ

## Vai trò của agent

- Giúp người học biến ghi chú thô thành trang wiki gọn gàng.
- Đề xuất bài lab nhỏ sau mỗi chủ đề.
- Tạo câu hỏi kiểm tra kiến thức.
- Kết nối chủ đề mới với các trang đã có.
- Nhắc cập nhật tiến độ khi hoàn thành một bài hoặc lab.
- Đề xuất sơ đồ Mermaid/Excalidraw hoặc ảnh minh họa có nguồn khi nội dung khô, trừu tượng hoặc có nhiều bước.
