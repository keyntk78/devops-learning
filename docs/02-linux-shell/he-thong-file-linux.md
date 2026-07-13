# Hệ thống file trong Linux

## Mục tiêu

- Hiểu Linux tổ chức file theo một cây thư mục bắt đầu từ `/`.
- Biết công dụng của các thư mục quan trọng như `/home`, `/etc`, `/var`, `/tmp`, `/usr`, `/proc`.
- Phân biệt đường dẫn tuyệt đối, đường dẫn tương đối và các ký hiệu `.`, `..`, `~`.
- Biết nên tìm file cấu hình, log, script và dữ liệu tạm ở đâu.

## Bức tranh lớn

Trong Linux, mọi thứ nằm dưới một cây thư mục bắt đầu từ `/`, gọi là **root directory**.

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── proc
├── root
├── tmp
├── usr
└── var
```

Khác với Windows hay dùng nhiều ổ như `C:\`, `D:\`, Linux dùng một cây chung. Ổ đĩa, thiết bị, process, file config, log và thư mục người dùng đều được biểu diễn trong cây này.

## Các thư mục quan trọng

| Thư mục | Công dụng | Dùng khi nào |
| --- | --- | --- |
| `/` | Gốc của toàn bộ hệ thống file | Khi cần hiểu mọi đường dẫn tuyệt đối bắt đầu từ đâu |
| `/home` | Chứa thư mục của user thường | Lưu code, script, ghi chú, lab cá nhân |
| `/root` | Home của user `root` | Khi thao tác bằng tài khoản quản trị hệ thống |
| `/etc` | File cấu hình hệ thống và service | Xem/sửa config như SSH, nginx, systemd |
| `/var` | Dữ liệu thay đổi theo thời gian | Xem log, cache, spool, dữ liệu runtime |
| `/var/log` | Log hệ thống và service | Debug lỗi service hoặc kiểm tra sự kiện |
| `/tmp` | File tạm | Tạo file test nhanh, dữ liệu có thể bị xóa |
| `/usr` | Chương trình và thư viện dùng chung | Xem binary, library, tài nguyên cài sẵn |
| `/bin` | Lệnh cơ bản của hệ thống | Chứa các lệnh như `ls`, `cat`, `cp` |
| `/sbin` | Lệnh quản trị hệ thống | Một số lệnh admin, thường cần `sudo` |
| `/dev` | Thiết bị được biểu diễn như file | Ổ đĩa, terminal, random device |
| `/proc` | Thông tin kernel và process dạng file ảo | Debug process, CPU, memory ở mức hệ thống |
| `/mnt` | Điểm mount thủ công | Trong WSL, ổ Windows thường nằm ở `/mnt/c` |
| `/opt` | Phần mềm cài thêm ngoài package mặc định | Một số tool vendor đặt ở đây |

## Đường dẫn tuyệt đối

Đường dẫn tuyệt đối bắt đầu từ `/`.

```bash
/home/student/devops-lab
/etc/ssh/sshd_config
/var/log/syslog
```

Dùng khi:

- Muốn chỉ rõ chính xác file/thư mục nằm ở đâu.
- Viết tài liệu, script hoặc config cần đường dẫn ổn định.
- Debug service vì log/config thường nằm ở đường dẫn tuyệt đối.

Ví dụ:

```bash
cd /etc
pwd
```

Kết quả mẫu:

```text
/etc
```

Giải thích:

- `cd /etc` đi thẳng đến thư mục `/etc` từ bất kỳ vị trí nào.
- `pwd` in ra vị trí hiện tại, xác nhận bạn đang ở `/etc`.

## Đường dẫn tương đối

Đường dẫn tương đối bắt đầu từ thư mục hiện tại.

```bash
scripts/hello.sh
../notes/today.md
./run.sh
```

Dùng khi:

- Đang làm việc trong một project hoặc lab.
- Muốn thao tác với file gần vị trí hiện tại.
- Viết lệnh ngắn hơn thay vì ghi toàn bộ đường dẫn.

Ý nghĩa nhanh:

| Ký hiệu | Ý nghĩa | Ví dụ |
| --- | --- | --- |
| `.` | Thư mục hiện tại | `./run.sh` |
| `..` | Thư mục cha | `cd ..` |
| `~` | Home của user hiện tại | `cd ~` |
| `/` | Root directory | `cd /` |

Ví dụ:

```bash
cd ~/devops-lab
pwd
```

Kết quả mẫu:

```text
/home/student/devops-lab
```

Giải thích:

- `~` được shell mở rộng thành home của user, ví dụ `/home/student`.
- `pwd` cho thấy đường dẫn tuyệt đối sau khi di chuyển.

## WSL và thư mục Windows

Khi dùng Ubuntu trong WSL, ổ Windows thường được mount vào `/mnt`.

Ví dụ:

```bash
ls /mnt
```

Kết quả mẫu:

```text
c  d
```

Giải thích:

- `/mnt/c` tương ứng với ổ `C:\` trên Windows.
- `/mnt/d` tương ứng với ổ `D:\` nếu máy có ổ D.

Ví dụ đi tới workspace Windows hiện tại:

```bash
cd /mnt/d/lerning/devops
pwd
```

Kết quả mẫu:

```text
/mnt/d/lerning/devops
```

Ghi nhớ:

- File Linux nên đặt trong home Linux, ví dụ `~/devops-lab`, để thao tác nhanh và ít lỗi permission.
- File tài liệu/wiki trong workspace Windows có thể truy cập qua `/mnt/d/...`.

## Lab: khám phá cây thư mục Linux

Chạy trong Ubuntu WSL:

```bash
cd /
pwd
ls
cd /etc
pwd
cd /var/log
pwd
cd ~
mkdir -p ~/devops-lab/linux-files/{apps,logs,notes,scripts}
cd ~/devops-lab/linux-files
pwd
```

Kết quả mẫu:

```text
/
bin boot dev etc home proc root tmp usr var
/etc
/var/log
/home/student/devops-lab/linux-files
```

Giải thích:

- `cd /` đưa bạn về gốc hệ thống.
- `ls` ở `/` cho thấy các thư mục cấp cao nhất.
- `/etc` là nơi hay chứa file cấu hình.
- `/var/log` là nơi hay chứa log.
- `~` đưa bạn về home của user hiện tại.
- `~/devops-lab/linux-files` là thư mục lab cá nhân để thực hành an toàn.

## Câu hỏi ôn tập

- Vì sao Linux bắt đầu từ `/` thay vì `C:\` như Windows?
- `/home` dùng để làm gì?
- `/etc` thường chứa loại file nào?
- `/var/log` quan trọng thế nào khi debug service?
- `~`, `.`, `..` khác nhau ra sao?
- Trong WSL, ổ `C:\` của Windows thường nằm ở đâu?
- Khi viết script, khi nào nên dùng đường dẫn tuyệt đối?

## Liên kết nội bộ

- [Các lệnh cơ bản trong Linux](cac-lenh-co-ban.md)
- [Tạo Linux server bằng WSL để thực hành](wsl-linux-server.md)
- [Command Index](../cheatsheets/command-index.md)

