# Module 2: Linux và Shell

## Mục tiêu

- Dùng terminal để điều hướng, kiểm tra file, process, service và logs.
- Hiểu permissions, users, groups, environment variables và systemd.
- Viết Bash script nhỏ để tự động hóa tác vụ lặp lại.

## Chủ đề chính

- Filesystem: `/`, `/etc`, `/var`, `/home`, `/tmp`, `/usr`
- File permissions: owner, group, mode, chmod, chown
- Process: `ps`, `top`, `kill`, exit code
- Service: `systemctl`, unit file, restart, status
- Logs: `journalctl`, `/var/log`
- Shell tools: `grep`, `sed`, `awk`, `find`, `xargs`, pipe, redirect
- Bash scripting: variable, condition, loop, function

## Bài học

- [Tạo Linux server bằng WSL để thực hành](wsl-linux-server.md)
- [Hệ thống file trong Linux](he-thong-file-linux.md)
- [Các lệnh cơ bản trong Linux](cac-lenh-co-ban.md)
- [Quyền truy cập trong Linux](quyen-truy-cap-trong-linux.md)

## Lab gợi ý

Tạo script `healthcheck.sh` kiểm tra:

- Disk còn trống bao nhiêu
- Memory đang dùng
- Một port có đang mở không
- Một process có đang chạy không

## Câu hỏi ôn tập

- Exit code `0` có nghĩa là gì?
- Pipe khác redirect thế nào?
- Khi service không chạy, bạn kiểm tra những lệnh nào trước?
- Permission `chmod 755` nghĩa là gì?
