# Các lệnh cơ bản trong Linux

## Mục tiêu

- Biết công dụng của các lệnh Linux cơ bản.
- Biết dùng lệnh trong tình huống thực tế.
- Đọc được output mẫu và hiểu kết quả đang nói gì.
- Có một lab nhỏ để luyện thao tác file, thư mục, log và dung lượng.

## Chuẩn bị thư mục thực hành

Chạy trong Ubuntu WSL:

```bash
mkdir -p ~/devops-lab/basic-commands/{apps,logs,notes,scripts,backups}
cd ~/devops-lab/basic-commands
echo "app=demo-api" > apps/app.conf
echo "2026-07-13 INFO service started" > logs/app.log
echo "Linux command note" > notes/commands.md
```

Kiểm tra:

```bash
tree .
```

Kết quả mẫu:

```text
.
├── apps
│   └── app.conf
├── backups
├── logs
│   └── app.log
├── notes
│   └── commands.md
└── scripts
```

Giải thích:

- `.` là thư mục hiện tại.
- `apps`, `logs`, `notes`, `scripts`, `backups` là thư mục con.
- `app.conf`, `app.log`, `commands.md` là file mẫu để luyện lệnh.

## `pwd`: xem mình đang đứng ở đâu

Công dụng: in ra thư mục hiện tại.

Dùng khi:

- Bạn bị lạc trong terminal.
- Trước khi chạy lệnh nguy hiểm như `rm`, `mv`, `cp`.
- Muốn ghi lại đường dẫn tuyệt đối vào tài liệu.

Ví dụ:

```bash
pwd
```

Kết quả mẫu:

```text
/home/student/devops-lab/basic-commands
```

Giải thích:

- Bạn đang ở thư mục `basic-commands`.
- Đường dẫn bắt đầu bằng `/`, nên đây là đường dẫn tuyệt đối.

## `ls`: liệt kê file và thư mục

Công dụng: xem bên trong thư mục có gì.

Dùng khi:

- Muốn biết file có tồn tại không.
- Muốn xem quyền, owner, dung lượng, thời gian sửa file.
- Muốn kiểm tra thư mục trước khi copy/xóa.

Ví dụ:

```bash
ls -lah
```

Kết quả mẫu:

```text
total 20K
drwxr-xr-x 7 student student 4.0K Jul 13 22:40 .
drwxr-xr-x 3 student student 4.0K Jul 13 22:39 ..
drwxr-xr-x 2 student student 4.0K Jul 13 22:40 apps
drwxr-xr-x 2 student student 4.0K Jul 13 22:40 backups
drwxr-xr-x 2 student student 4.0K Jul 13 22:40 logs
drwxr-xr-x 2 student student 4.0K Jul 13 22:40 notes
drwxr-xr-x 2 student student 4.0K Jul 13 22:40 scripts
```

Giải thích:

- `d` ở đầu `drwxr-xr-x` nghĩa là directory.
- `student student` là owner và group.
- `4.0K` là dung lượng hiển thị dễ đọc nhờ `-h`.
- `.` là thư mục hiện tại, `..` là thư mục cha.

Option thường dùng:

| Lệnh | Công dụng |
| --- | --- |
| `ls` | Xem danh sách ngắn |
| `ls -l` | Xem chi tiết |
| `ls -a` | Xem cả file ẩn |
| `ls -h` | Hiển thị dung lượng dễ đọc |
| `ls -lah` | Kết hợp chi tiết, file ẩn, dung lượng dễ đọc |

## `cd`: di chuyển giữa các thư mục

Công dụng: đổi thư mục hiện tại.

Dùng khi:

- Muốn vào thư mục chứa code, log, config.
- Muốn quay về home.
- Muốn đi lên thư mục cha.

Ví dụ:

```bash
cd logs
pwd
```

Kết quả mẫu:

```text
/home/student/devops-lab/basic-commands/logs
```

Giải thích:

- `cd logs` đi vào thư mục `logs` từ vị trí hiện tại.
- `pwd` xác nhận vị trí sau khi di chuyển.

Các cách dùng phổ biến:

| Lệnh | Ý nghĩa |
| --- | --- |
| `cd ~` | Về home của user |
| `cd ..` | Lên thư mục cha |
| `cd -` | Quay lại thư mục trước đó |
| `cd /etc` | Đi tới đường dẫn tuyệt đối `/etc` |
| `cd ./logs` | Đi vào thư mục `logs` từ vị trí hiện tại |

## `mkdir`: tạo thư mục

Công dụng: tạo thư mục mới.

Dùng khi:

- Tạo workspace lab.
- Tạo thư mục chứa logs, scripts, config.
- Tạo nhiều cấp thư mục cùng lúc.

Ví dụ:

```bash
mkdir reports
ls
```

Kết quả mẫu:

```text
app.log  reports
```

Giải thích:

- `reports` là thư mục mới trong thư mục hiện tại.
- Nếu thư mục đã tồn tại, `mkdir reports` sẽ báo lỗi.

Tạo nhiều cấp thư mục:

```bash
mkdir -p ~/devops-lab/basic-commands/backups/daily
```

Giải thích:

- `-p` tạo luôn thư mục cha nếu chưa tồn tại.
- Nếu thư mục đã có, lệnh không báo lỗi.

## `touch`: tạo file rỗng hoặc cập nhật thời gian sửa

Công dụng: tạo file rỗng nếu file chưa có, hoặc cập nhật timestamp nếu file đã tồn tại.

Dùng khi:

- Tạo nhanh file để test.
- Tạo file log/config mẫu.
- Cập nhật thời gian sửa file.

Ví dụ:

```bash
touch notes/today.md
ls -l notes
```

Kết quả mẫu:

```text
-rw-r--r-- 1 student student  0 Jul 13 22:45 today.md
-rw-r--r-- 1 student student 19 Jul 13 22:40 commands.md
```

Giải thích:

- `today.md` có size `0`, nghĩa là file rỗng.
- Ký tự đầu `-` nghĩa là file thường, không phải thư mục.

## `cat`: xem nhanh nội dung file

Công dụng: in toàn bộ nội dung file ra terminal.

Dùng khi:

- File ngắn.
- Muốn xem nhanh config hoặc note.
- Muốn nối nội dung file vào pipeline.

Ví dụ:

```bash
cat apps/app.conf
```

Kết quả mẫu:

```text
app=demo-api
```

Giải thích:

- File `app.conf` có một dòng cấu hình.
- `cat` phù hợp vì file ngắn.

## `less`: xem file dài từng trang

Công dụng: mở file để đọc từng trang.

Dùng khi:

- File dài.
- Log nhiều dòng.
- Muốn tìm kiếm trong file khi đang xem.

Ví dụ:

```bash
less /var/log/syslog
```

Phím thường dùng trong `less`:

| Phím | Công dụng |
| --- | --- |
| `Space` | Xuống một trang |
| `b` | Lên một trang |
| `/error` | Tìm chữ `error` |
| `n` | Tới kết quả tìm kiếm tiếp theo |
| `q` | Thoát |

Giải thích:

- `less` không in hết file ra terminal ngay.
- Bạn đọc được file lớn dễ hơn và thoát bằng `q`.

## `head` và `tail`: xem đầu/cuối file

Công dụng:

- `head` xem các dòng đầu.
- `tail` xem các dòng cuối.

Dùng khi:

- Muốn xem file bắt đầu như thế nào.
- Muốn xem log mới nhất ở cuối file.
- Muốn theo dõi log realtime bằng `tail -f`.

Ví dụ:

```bash
tail logs/app.log
```

Kết quả mẫu:

```text
2026-07-13 INFO service started
```

Giải thích:

- `tail` mặc định hiển thị 10 dòng cuối.
- File mẫu chỉ có 1 dòng nên output có 1 dòng.

Theo dõi log realtime:

```bash
tail -f logs/app.log
```

Giải thích:

- `-f` giữ terminal mở và in thêm dòng mới khi file log được ghi tiếp.
- Dùng rất nhiều khi debug service.

## `cp`: copy file hoặc thư mục

Công dụng: sao chép file/thư mục.

Dùng khi:

- Backup file config trước khi sửa.
- Copy log ra thư mục khác.
- Nhân bản file mẫu.

Ví dụ:

```bash
cp apps/app.conf apps/app.conf.bak
ls apps
```

Kết quả mẫu:

```text
app.conf  app.conf.bak
```

Giải thích:

- `app.conf.bak` là bản copy của `app.conf`.
- Đây là thói quen tốt trước khi sửa config.

Copy thư mục:

```bash
cp -r notes notes-backup
```

Giải thích:

- `-r` nghĩa là recursive, copy cả thư mục và nội dung bên trong.

## `mv`: di chuyển hoặc đổi tên

Công dụng: move file/thư mục hoặc rename.

Dùng khi:

- Đổi tên file.
- Chuyển file sang thư mục khác.
- Sắp xếp lại cấu trúc project.

Ví dụ đổi tên:

```bash
mv notes/today.md notes/linux-today.md
ls notes
```

Kết quả mẫu:

```text
commands.md  linux-today.md
```

Giải thích:

- File `today.md` đã được đổi tên thành `linux-today.md`.

Ví dụ di chuyển:

```bash
mv apps/app.conf.bak backups/
ls backups
```

Kết quả mẫu:

```text
app.conf.bak
```

Giải thích:

- File backup đã được chuyển vào thư mục `backups`.

## `rm`: xóa file hoặc thư mục

Công dụng: xóa file/thư mục.

Dùng khi:

- Xóa file test.
- Dọn log hoặc artifact không cần nữa.
- Xóa thư mục lab sau khi đã backup.

Ví dụ:

```bash
rm notes/linux-today.md
ls notes
```

Kết quả mẫu:

```text
commands.md
```

Giải thích:

- File `linux-today.md` đã bị xóa.
- Linux thường không có Recycle Bin cho lệnh `rm`.

Xóa thư mục:

```bash
rm -r notes-backup
```

Giải thích:

- `-r` xóa thư mục và nội dung bên trong.
- Luôn chạy `pwd` và `ls` trước khi xóa để chắc bạn đang ở đúng chỗ.

## `file`: xem loại file

Công dụng: đoán loại file dựa trên nội dung.

Dùng khi:

- Không chắc file là text, binary, archive hay executable.
- Debug file tải về không đúng định dạng.
- Kiểm tra script có phải text không.

Ví dụ:

```bash
file apps/app.conf
```

Kết quả mẫu:

```text
apps/app.conf: ASCII text
```

Giải thích:

- File này là text thường.
- Có thể mở bằng `cat`, `less`, `vim`, `nano`.

## `stat`: xem metadata của file

Công dụng: xem thông tin chi tiết về file.

Dùng khi:

- Kiểm tra quyền, owner, thời gian sửa file.
- Debug file có được cập nhật hay chưa.
- So sánh timestamp khi deploy.

Ví dụ:

```bash
stat apps/app.conf
```

Kết quả mẫu:

```text
  File: apps/app.conf
  Size: 13         Blocks: 8          IO Block: 4096   regular file
Access: (0644/-rw-r--r--)  Uid: ( 1000/ student)   Gid: ( 1000/ student)
Access: 2026-07-13 22:40:00.000000000 +0700
Modify: 2026-07-13 22:40:00.000000000 +0700
Change: 2026-07-13 22:40:00.000000000 +0700
```

Giải thích:

- `Size: 13` là dung lượng file theo byte.
- `0644/-rw-r--r--` là quyền truy cập.
- `Uid/Gid` cho biết owner và group.
- `Modify` là thời điểm nội dung file thay đổi.

## `tree`: xem cây thư mục

Công dụng: hiển thị cấu trúc thư mục dạng cây.

Dùng khi:

- Muốn hiểu cấu trúc project.
- Viết tài liệu hướng dẫn.
- Kiểm tra lab đã tạo đúng chưa.

Ví dụ:

```bash
tree -L 2 ~/devops-lab/basic-commands
```

Kết quả mẫu:

```text
/home/student/devops-lab/basic-commands
├── apps
│   └── app.conf
├── backups
├── logs
│   └── app.log
├── notes
│   └── commands.md
└── scripts
```

Giải thích:

- `-L 2` chỉ hiển thị sâu tối đa 2 cấp.
- Hữu ích khi project có rất nhiều thư mục con.

Nếu chưa có `tree`:

```bash
sudo apt install -y tree
```

## `find`: tìm file theo tên hoặc loại

Công dụng: tìm file/thư mục trong cây thư mục.

Dùng khi:

- Không nhớ file nằm ở đâu.
- Tìm log, config, script.
- Tìm file cũ để dọn dẹp.

Ví dụ tìm file `.conf`:

```bash
find ~/devops-lab/basic-commands -name "*.conf"
```

Kết quả mẫu:

```text
/home/student/devops-lab/basic-commands/apps/app.conf
```

Giải thích:

- `find` bắt đầu tìm từ `~/devops-lab/basic-commands`.
- `-name "*.conf"` lọc các file có tên kết thúc bằng `.conf`.

Ví dụ chỉ tìm thư mục:

```bash
find ~/devops-lab/basic-commands -type d -name "logs"
```

Kết quả mẫu:

```text
/home/student/devops-lab/basic-commands/logs
```

Giải thích:

- `-type d` nghĩa là chỉ tìm directory.
- Nếu tìm file thường thì dùng `-type f`.

## `grep`: tìm chữ trong file hoặc output

Công dụng: tìm dòng chứa nội dung khớp với pattern.

Dùng khi:

- Tìm lỗi trong log.
- Tìm config theo key.
- Lọc output của lệnh khác.

Ví dụ:

```bash
grep "INFO" logs/app.log
```

Kết quả mẫu:

```text
2026-07-13 INFO service started
```

Giải thích:

- Dòng này được in ra vì có chữ `INFO`.

Hiển thị số dòng:

```bash
grep -n "INFO" logs/app.log
```

Kết quả mẫu:

```text
1:2026-07-13 INFO service started
```

Giải thích:

- `1:` nghĩa là kết quả nằm ở dòng số 1.

Kết hợp với pipe:

```bash
ls -lah /var/log | grep "syslog"
```

Giải thích:

- `ls -lah /var/log` liệt kê log.
- `|` đưa output sang `grep`.
- `grep "syslog"` chỉ giữ dòng có chữ `syslog`.

## `df`: kiểm tra dung lượng filesystem

Công dụng: xem dung lượng ổ đĩa/filesystem.

Dùng khi:

- Server báo hết disk.
- Database hoặc log ghi không được.
- Trước khi deploy artifact lớn.

Ví dụ:

```bash
df -h /
```

Kết quả mẫu:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdc        251G   12G  227G   5% /
```

Giải thích:

- `Size`: tổng dung lượng filesystem.
- `Used`: đã dùng.
- `Avail`: còn trống.
- `Use%`: phần trăm đã dùng.
- `Mounted on /`: filesystem này đang mount tại root `/`.

## `du`: kiểm tra dung lượng thư mục/file

Công dụng: xem thư mục hoặc file đang chiếm bao nhiêu dung lượng.

Dùng khi:

- `df` báo disk gần đầy và bạn cần tìm thư mục nào chiếm nhiều.
- Kiểm tra log, cache, artifact.
- Dọn dẹp file lớn.

Ví dụ:

```bash
du -sh ~/devops-lab/basic-commands
```

Kết quả mẫu:

```text
28K     /home/student/devops-lab/basic-commands
```

Giải thích:

- `-s` chỉ hiển thị tổng.
- `-h` hiển thị dễ đọc.
- Thư mục lab đang dùng khoảng `28K`.

## `echo`, `>` và `>>`: ghi nội dung nhanh vào file

Công dụng:

- `echo` in text ra terminal.
- `>` ghi đè nội dung vào file.
- `>>` thêm nội dung vào cuối file.

Dùng khi:

- Tạo config mẫu.
- Ghi log giả lập.
- Test redirect trong shell.

Ví dụ ghi đè:

```bash
echo "env=dev" > apps/app.conf
cat apps/app.conf
```

Kết quả:

```text
env=dev
```

Giải thích:

- `>` thay toàn bộ nội dung cũ bằng `env=dev`.

Ví dụ ghi thêm:

```bash
echo "port=8080" >> apps/app.conf
cat apps/app.conf
```

Kết quả:

```text
env=dev
port=8080
```

Giải thích:

- `>>` thêm dòng mới vào cuối file.
- Nội dung cũ vẫn còn.

## `chmod`: đổi quyền file

Công dụng: thay đổi quyền đọc, ghi, chạy của file/thư mục.

Dùng khi:

- Script không chạy được vì thiếu quyền execute.
- Cần giới hạn quyền file secret.
- Cần hiểu lỗi `Permission denied`.

Ví dụ:

```bash
echo 'echo "hello linux"' > scripts/hello.sh
./scripts/hello.sh
```

Kết quả mẫu:

```text
bash: ./scripts/hello.sh: Permission denied
```

Giải thích:

- File chưa có quyền execute.
- Cần thêm quyền chạy bằng `chmod +x`.

Thêm quyền execute:

```bash
chmod +x scripts/hello.sh
./scripts/hello.sh
```

Kết quả mẫu:

```text
hello linux
```

Giải thích:

- Sau `chmod +x`, script có thể chạy như một chương trình.

## `sudo`: chạy lệnh với quyền quản trị

Công dụng: chạy lệnh bằng quyền cao hơn, thường là root.

Dùng khi:

- Cài package.
- Sửa file hệ thống.
- Start/stop service.
- Đọc log hoặc thư mục bị giới hạn quyền.

Ví dụ:

```bash
sudo apt update
```

Kết quả mẫu:

```text
Hit:1 http://archive.ubuntu.com/ubuntu noble InRelease
Reading package lists... Done
```

Giải thích:

- `apt update` cập nhật danh sách package từ repository.
- `sudo` cần vì thao tác này ảnh hưởng hệ thống.

Lưu ý:

- Không dùng `sudo` nếu không cần.
- Trước khi chạy lệnh với `sudo`, hiểu lệnh đó làm gì.
- Đặc biệt cẩn thận với các lệnh xóa file khi chạy bằng `sudo`.

## `whoami`: xem user hiện tại

Công dụng: in ra username đang chạy lệnh.

Dùng khi:

- Không chắc mình đang đăng nhập bằng user nào.
- Kiểm tra script/service đang chạy bằng user nào.
- Debug lỗi permission.

Ví dụ:

```bash
whoami
```

Kết quả mẫu:

```text
student
```

Giải thích:

- Lệnh hiện đang chạy dưới user `student`.
- Nếu kết quả là `root`, bạn đang có quyền quản trị cao, cần cẩn thận hơn.

## `history`: xem lịch sử lệnh đã chạy

Công dụng: hiển thị các lệnh đã chạy trước đó trong shell.

Dùng khi:

- Muốn nhớ lại lệnh vừa chạy.
- Ghi chép lại các bước thực hành.
- Tìm lại lệnh gây lỗi để phân tích.

Ví dụ:

```bash
history | tail
```

Kết quả mẫu:

```text
  42  cd ~/devops-lab/basic-commands
  43  ls -lah
  44  cat apps/app.conf
  45  history | tail
```

Giải thích:

- Cột đầu là số thứ tự trong lịch sử.
- Cột sau là lệnh đã chạy.
- `tail` chỉ lấy các dòng cuối để output gọn hơn.

## `free`: xem bộ nhớ RAM

Công dụng: hiển thị tình trạng memory.

Dùng khi:

- Server chậm và nghi thiếu RAM.
- Kiểm tra app có dùng quá nhiều memory không.
- Trước/sau khi chạy service nặng.

Ví dụ:

```bash
free -h
```

Kết quả mẫu:

```text
               total        used        free      shared  buff/cache   available
Mem:           7.6Gi       1.2Gi       5.1Gi        24Mi       1.3Gi       6.1Gi
Swap:          2.0Gi          0B       2.0Gi
```

Giải thích:

- `total`: tổng RAM.
- `used`: RAM đang dùng.
- `free`: RAM còn trống hoàn toàn.
- `buff/cache`: RAM Linux dùng để cache, có thể giải phóng khi cần.
- `available`: lượng RAM còn có thể dùng cho app mới.

## `top`: xem process realtime

Công dụng: xem process đang chạy và mức dùng CPU/RAM theo thời gian thực.

Dùng khi:

- Server chậm, CPU cao.
- Muốn biết process nào đang ăn tài nguyên.
- Debug app bị treo hoặc chạy bất thường.

Ví dụ:

```bash
top
```

Kết quả mẫu:

```text
top - 22:55:01 up 2:10,  1 user,  load average: 0.03, 0.05, 0.01
Tasks:  42 total,   1 running,  41 sleeping
%Cpu(s):  1.0 us,  0.5 sy,  0.0 ni, 98.5 id
MiB Mem :   7800.0 total,   5200.0 free,   1200.0 used,   1400.0 buff/cache
```

Giải thích:

- `load average`: tải trung bình của hệ thống.
- `Tasks`: số process.
- `%Cpu(s)`: phần trăm CPU đang dùng.
- `id` là idle, CPU đang rảnh.
- Thoát `top` bằng phím `q`.

## `ps`: xem danh sách process

Công dụng: liệt kê process đang chạy.

Dùng khi:

- Tìm process của app/service.
- Lấy PID để debug hoặc kill process.
- Kiểm tra lệnh có đang chạy nền không.

Ví dụ:

```bash
ps aux | grep ssh
```

Kết quả mẫu:

```text
root        512  0.0  0.1  15432  6200 ?        Ss   22:10   0:00 sshd: /usr/sbin/sshd -D
student     900  0.0  0.0   4028  2100 pts/0    S+   22:56   0:00 grep --color=auto ssh
```

Giải thích:

- Dòng `sshd` cho biết SSH server đang chạy.
- Cột đầu là user chạy process.
- Cột thứ hai là PID.
- Dòng `grep ssh` là chính lệnh tìm kiếm vừa chạy.

## `hostnamectl`: xem thông tin máy

Công dụng: xem hostname, OS, kernel, kiến trúc máy.

Dùng khi:

- Kiểm tra đang ở server nào.
- Ghi thông tin môi trường vào tài liệu.
- Debug khác biệt giữa các máy.

Ví dụ:

```bash
hostnamectl
```

Kết quả mẫu:

```text
 Static hostname: key
       Icon name: computer-vm
         Chassis: vm
      Machine ID: 1234567890abcdef
 Operating System: Ubuntu 26.04 LTS
          Kernel: Linux 6.18.33.2-microsoft-standard-WSL2
    Architecture: x86-64
```

Giải thích:

- `Static hostname` là tên máy.
- `Operating System` là bản Linux đang dùng.
- `Kernel` cho biết kernel, trong WSL thường có chữ `microsoft-standard-WSL2`.

## `reboot`: khởi động lại máy

Công dụng: restart hệ thống.

Dùng khi:

- Cập nhật kernel hoặc service yêu cầu restart.
- Lab cần kiểm tra service có tự chạy sau reboot không.
- Máy ở trạng thái lỗi khó khôi phục bằng restart service.

Ví dụ:

```bash
sudo reboot
```

Kết quả:

```text
Connection to server closed.
```

Giải thích:

- Máy hoặc session sẽ bị ngắt vì hệ thống đang khởi động lại.
- Trong WSL, thường dùng từ PowerShell: `wsl --shutdown`, rồi mở lại Ubuntu.

## `apt`: cài và quản lý package

Công dụng: quản lý package trên Ubuntu/Debian.

Dùng khi:

- Cài công cụ mới.
- Cập nhật danh sách package.
- Gỡ phần mềm không cần nữa.

Ví dụ:

```bash
sudo apt update
sudo apt install -y tree
```

Kết quả mẫu:

```text
Reading package lists... Done
Building dependency tree... Done
The following NEW packages will be installed:
  tree
```

Giải thích:

- `apt update` cập nhật danh sách package từ repository.
- `apt install -y tree` cài `tree` và tự động trả lời yes.

Lệnh thường dùng:

| Lệnh | Công dụng |
| --- | --- |
| `sudo apt update` | Cập nhật danh sách package |
| `sudo apt upgrade -y` | Nâng cấp package đã cài |
| `sudo apt install -y <package>` | Cài package |
| `sudo apt remove <package>` | Gỡ package |
| `apt search <keyword>` | Tìm package |

## `ssh`: truy cập server từ xa

Công dụng: đăng nhập vào server Linux qua mạng.

Dùng khi:

- Kết nối vào VPS/cloud server.
- Mô phỏng thao tác với server thật.
- Chạy lệnh, xem log, sửa config trên máy remote.

Ví dụ:

```bash
ssh student@localhost
```

Kết quả mẫu:

```text
student@localhost's password:
Welcome to Ubuntu 26.04 LTS
student@key:~$
```

Giải thích:

- `student` là user muốn đăng nhập.
- `localhost` là máy đích.
- Sau khi đăng nhập thành công, prompt chuyển sang shell của server.

## `ping`: kiểm tra máy có phản hồi mạng không

Công dụng: gửi gói ICMP để kiểm tra host có reachable không.

Dùng khi:

- Kiểm tra kết nối mạng cơ bản.
- Kiểm tra DNS có resolve được domain không.
- Phân biệt lỗi mạng với lỗi ứng dụng.

Ví dụ:

```bash
ping -c 4 example.com
```

Kết quả mẫu:

```text
64 bytes from 93.184.216.34: icmp_seq=1 ttl=56 time=23.4 ms
64 bytes from 93.184.216.34: icmp_seq=2 ttl=56 time=22.9 ms
--- example.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss
```

Giải thích:

- `-c 4` gửi 4 gói rồi dừng.
- `0% packet loss` nghĩa là không mất gói.
- `time` là độ trễ mỗi gói.

## `traceroute`: xem đường đi mạng

Công dụng: hiển thị các hop mạng từ máy bạn đến host đích.

Dùng khi:

- Debug mạng chậm hoặc đứt ở đâu đó.
- Kiểm tra request đi qua các router nào.
- So sánh đường đi mạng giữa các môi trường.

Ví dụ:

```bash
traceroute example.com
```

Kết quả mẫu:

```text
1  172.20.0.1    1.2 ms
2  192.168.1.1   3.4 ms
3  10.10.10.1    8.9 ms
```

Giải thích:

- Mỗi dòng là một hop mạng.
- Số `ms` là thời gian phản hồi của hop đó.
- Nếu chưa có lệnh này, cài bằng `sudo apt install -y traceroute`.

## `telnet`: kiểm tra port TCP đơn giản

Công dụng: thử kết nối TCP đến host và port.

Dùng khi:

- Kiểm tra port có mở không.
- Debug service có lắng nghe port không.
- Kiểm tra firewall/security group có chặn không.

Ví dụ:

```bash
telnet example.com 80
```

Kết quả mẫu khi kết nối được:

```text
Trying 93.184.216.34...
Connected to example.com.
Escape character is '^]'.
```

Giải thích:

- `Connected` nghĩa là port TCP 80 mở và kết nối được.
- Nếu timeout hoặc refused, có thể service không chạy, port sai hoặc bị firewall chặn.
- Nếu chưa có lệnh này, cài bằng `sudo apt install -y telnet`.

## `netstat` và `ss`: xem port đang lắng nghe

Công dụng: kiểm tra socket, port và process mạng.

Dùng khi:

- Kiểm tra service có mở port chưa.
- Debug lỗi app không truy cập được.
- Xem process nào đang chiếm port.

Ví dụ hiện đại với `ss`:

```bash
sudo ss -lntp
```

Kết quả mẫu:

```text
State  Recv-Q Send-Q Local Address:Port  Peer Address:Port Process
LISTEN 0      128          0.0.0.0:22         0.0.0.0:*     users:(("sshd",pid=512,fd=3))
```

Giải thích:

- `LISTEN` nghĩa là service đang lắng nghe kết nối.
- `0.0.0.0:22` nghĩa là port 22 mở trên mọi interface.
- `sshd` là process đang giữ port.

Ví dụ cũ với `netstat`:

```bash
sudo netstat -lntp
```

Ghi nhớ:

- `ss` là công cụ mới hơn, thường có sẵn.
- `netstat` thuộc package `net-tools`, có thể cần cài bằng `sudo apt install -y net-tools`.

## Bảng tóm tắt

| Lệnh | Công dụng chính | Ví dụ |
| --- | --- | --- |
| `pwd` | Xem thư mục hiện tại | `pwd` |
| `ls` | Liệt kê file/thư mục | `ls -lah` |
| `cd` | Di chuyển thư mục | `cd ~/devops-lab` |
| `mkdir` | Tạo thư mục | `mkdir -p logs/app` |
| `touch` | Tạo file rỗng | `touch app.log` |
| `cat` | Xem file ngắn | `cat app.conf` |
| `less` | Xem file dài | `less /var/log/syslog` |
| `head` | Xem đầu file | `head -n 20 app.log` |
| `tail` | Xem cuối file/log realtime | `tail -f app.log` |
| `cp` | Copy file/thư mục | `cp app.conf app.conf.bak` |
| `mv` | Di chuyển/đổi tên | `mv old.txt new.txt` |
| `rm` | Xóa file/thư mục | `rm old.txt` |
| `file` | Xem loại file | `file app.conf` |
| `stat` | Xem metadata | `stat app.conf` |
| `tree` | Xem cây thư mục | `tree -L 2 .` |
| `find` | Tìm file | `find . -name "*.log"` |
| `grep` | Tìm chữ trong file/output | `grep -n "ERROR" app.log` |
| `df` | Xem dung lượng filesystem | `df -h /` |
| `du` | Xem dung lượng thư mục | `du -sh .` |
| `chmod` | Đổi quyền file | `chmod +x script.sh` |
| `sudo` | Chạy lệnh quyền admin | `sudo systemctl status ssh` |
| `whoami` | Xem user hiện tại | `whoami` |
| `history` | Xem lịch sử lệnh | `history | tail` |
| `free` | Xem RAM | `free -h` |
| `top` | Xem process realtime | `top` |
| `ps` | Xem process | `ps aux | grep ssh` |
| `hostnamectl` | Xem thông tin máy | `hostnamectl` |
| `reboot` | Restart hệ thống | `sudo reboot` |
| `apt` | Quản lý package Ubuntu | `sudo apt install -y tree` |
| `ssh` | Truy cập server từ xa | `ssh user@host` |
| `ping` | Kiểm tra host phản hồi mạng | `ping -c 4 example.com` |
| `traceroute` | Xem đường đi mạng | `traceroute example.com` |
| `telnet` | Kiểm tra port TCP | `telnet example.com 80` |
| `ss` | Xem port/socket | `sudo ss -lntp` |
| `netstat` | Xem port/socket kiểu cũ | `sudo netstat -lntp` |

## Lab: đi một vòng các lệnh cơ bản

Mục tiêu: luyện các lệnh theo một flow giống khi debug server.

Chạy từng bước:

```bash
cd ~/devops-lab/basic-commands
pwd
ls -lah
tree -L 2 .
cat apps/app.conf
echo "2026-07-13 ERROR database timeout" >> logs/app.log
tail logs/app.log
grep -n "ERROR" logs/app.log
cp logs/app.log logs/app.log.bak
find . -name "*.log*"
du -sh .
df -h /
whoami
free -h
ps aux | grep ssh
sudo ss -lntp
```

Bạn nên tự giải thích được:

- Mình đang đứng ở thư mục nào?
- Có những file/thư mục nào?
- File config chứa gì?
- Log mới nhất là gì?
- Dòng lỗi nằm ở dòng số mấy?
- File backup đã được tạo chưa?
- Thư mục lab chiếm bao nhiêu dung lượng?
- Filesystem root còn trống bao nhiêu?
- User hiện tại là ai?
- Máy còn bao nhiêu RAM available?
- Port nào đang lắng nghe?

## Câu hỏi ôn tập

- Khi bị lạc trong terminal, dùng lệnh nào?
- `ls -lah` khác `ls` ở điểm nào?
- Khi nào dùng `cat`, khi nào dùng `less`?
- `tail -f` hữu ích khi debug service như thế nào?
- Vì sao nên `cp file file.bak` trước khi sửa config?
- `find` và `grep` khác nhau thế nào?
- `df` khác `du` thế nào?
- Vì sao phải cẩn thận với `rm`?
- `chmod +x` giải quyết lỗi gì?
- Khi nào cần dùng `sudo`?
- `ping` kiểm tra được điều gì?
- `ss -lntp` giúp debug port như thế nào?

## Liên kết nội bộ

- [Hệ thống file trong Linux](he-thong-file-linux.md)
- [Tạo Linux server bằng WSL để thực hành](wsl-linux-server.md)
- [Command Index](../cheatsheets/command-index.md)
