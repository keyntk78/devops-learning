# Quyền truy cập trong Linux

## Mục tiêu

- Hiểu Linux quản lý **user**, **group**, **quyền sở hữu** và **quyền truy cập** như thế nào.
- Phân biệt `useradd` và `adduser`.
- Đọc được các file quan trọng như `/etc/passwd`, `/etc/shadow`, `/etc/group`.
- Biết tạo, xóa user/group và thêm user vào group.
- Hiểu output của `ls -l`.
- Dùng được `chmod`, `chown`, `chgrp`, `umask`, `id`, `groups`, `sudo`.
- Debug được lỗi `Permission denied` ở mức cơ bản.

## Bức tranh lớn

Linux là hệ điều hành nhiều người dùng. Mỗi file/thư mục đều có:

- Một **owner user**: người sở hữu file.
- Một **owner group**: nhóm sở hữu file.
- Một bộ **permission**: ai được đọc, ghi, chạy.

Mô hình cơ bản:

```text
File/Directory
├── Owner user
├── Owner group
└── Permissions
    ├── User permissions
    ├── Group permissions
    └── Others permissions
```

Khi bạn chạy một lệnh, Linux kiểm tra:

1. Bạn là user nào?
2. Bạn thuộc group nào?
3. File/thư mục đó thuộc user/group nào?
4. Permission có cho phép hành động đó không?

Nếu không đủ quyền, bạn thường thấy lỗi:

```text
Permission denied
```

## User là gì?

User là tài khoản trong Linux. Mỗi user có một định danh số gọi là **UID**.

Kiểm tra user hiện tại:

```bash
whoami
```

Kết quả mẫu:

```text
student
```

Giải thích:

- Lệnh đang chạy dưới user `student`.
- Nếu kết quả là `root`, bạn đang có quyền quản trị rất cao.

Xem thông tin user hiện tại:

```bash
id
```

Kết quả mẫu:

```text
uid=1000(student) gid=1000(student) groups=1000(student),27(sudo)
```

Giải thích:

- `uid=1000(student)`: user hiện tại là `student`, UID là `1000`.
- `gid=1000(student)`: primary group là `student`.
- `groups=...`: user thuộc các group nào.
- `sudo`: group cho phép user chạy lệnh quản trị qua `sudo`.

## Root user là gì?

`root` là user quản trị cao nhất trong Linux.

Root có thể:

- Cài/gỡ package.
- Sửa file hệ thống.
- Tạo/xóa user.
- Thay đổi owner và permission.
- Start/stop service.

Không nên dùng root cho mọi việc hằng ngày. Thay vào đó, dùng user thường và chỉ thêm `sudo` khi cần quyền quản trị.

Ví dụ:

```bash
sudo apt update
```

Giải thích:

- `apt update` cần quyền sửa dữ liệu hệ thống.
- `sudo` tạm thời chạy lệnh với quyền cao hơn.

## Group là gì?

Group là nhóm user. Group giúp cấp quyền cho nhiều user cùng lúc.

Ví dụ:

- Group `sudo`: user trong group này có thể dùng `sudo`.
- Group `docker`: user trong group này có thể chạy Docker không cần `sudo`.
- Group `www-data`: thường liên quan đến web server.
- Group `developers`: nhóm tự tạo cho team developer.

Xem group của user hiện tại:

```bash
groups
```

Kết quả mẫu:

```text
student sudo docker
```

Giải thích:

- User hiện tại thuộc group `student`, `sudo`, `docker`.
- Nếu một file cho phép group `docker` truy cập, user này có thể được hưởng quyền đó.

## File `/etc/passwd`

File `/etc/passwd` chứa thông tin user cơ bản.

Xem một vài dòng:

```bash
head -n 5 /etc/passwd
```

Kết quả mẫu:

```text
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
student:x:1000:1000:Student:/home/student:/bin/bash
```

Một dòng có dạng:

```text
username:x:UID:GID:GECOS:home:shell
```

Giải thích từng phần:

| Phần | Ý nghĩa | Ví dụ |
| --- | --- | --- |
| `username` | Tên user | `student` |
| `x` | Password hash không nằm ở đây, thường nằm trong `/etc/shadow` | `x` |
| `UID` | User ID | `1000` |
| `GID` | Primary group ID | `1000` |
| `GECOS` | Mô tả/user info | `Student` |
| `home` | Thư mục home | `/home/student` |
| `shell` | Shell đăng nhập | `/bin/bash` |

Lưu ý:

- `/etc/passwd` thường có thể đọc bởi user thường.
- Password thật không nên nằm trong file này.

## File `/etc/shadow`

File `/etc/shadow` chứa password hash và thông tin hết hạn mật khẩu.

Xem thử:

```bash
sudo head -n 3 /etc/shadow
```

Kết quả mẫu:

```text
root:*:19876:0:99999:7:::
student:$y$j9T$...:19876:0:99999:7:::
```

Giải thích:

- File này cần `sudo` vì chứa thông tin nhạy cảm.
- Dấu `*` hoặc `!` thường nghĩa là tài khoản bị khóa password hoặc không dùng password login trực tiếp.
- Chuỗi dài sau username là password hash, không phải password plain text.

Không sửa `/etc/shadow` bằng tay nếu chưa thật sự hiểu. Dùng các lệnh như `passwd`, `usermod`, `chage`.

## File `/etc/group`

File `/etc/group` chứa danh sách group.

Xem một vài dòng:

```bash
head -n 10 /etc/group
```

Kết quả mẫu:

```text
root:x:0:
sudo:x:27:student
developers:x:1001:devdemo
```

Một dòng có dạng:

```text
groupname:x:GID:members
```

Giải thích:

| Phần | Ý nghĩa | Ví dụ |
| --- | --- | --- |
| `groupname` | Tên group | `developers` |
| `x` | Placeholder password group | `x` |
| `GID` | Group ID | `1001` |
| `members` | User phụ thuộc group này | `devdemo` |

## `useradd` và `adduser` khác nhau thế nào?

Trên Ubuntu/Debian:

| Lệnh | Đặc điểm | Khi nào dùng |
| --- | --- | --- |
| `useradd` | Lệnh cấp thấp, ít hỏi, cần truyền option rõ ràng | Dùng trong script hoặc khi muốn kiểm soát chi tiết |
| `adduser` | Lệnh thân thiện hơn, hỏi tương tác, tự tạo home dễ hơn | Dùng khi học hoặc tạo user thủ công trên Ubuntu |

Ví dụ với `adduser`:

```bash
sudo adduser devdemo
```

Kết quả mẫu:

```text
Adding user `devdemo' ...
Adding new group `devdemo' (1001) ...
Adding new user `devdemo' (1001) with group `devdemo' ...
Creating home directory `/home/devdemo' ...
New password:
Retype new password:
```

Giải thích:

- `adduser` tự tạo user, group riêng và home directory.
- Lệnh hỏi password và một số thông tin người dùng.
- Phù hợp khi làm thủ công.

Ví dụ với `useradd`:

```bash
sudo useradd -m -s /bin/bash devopsdemo
sudo passwd devopsdemo
```

Giải thích:

- `-m`: tạo home directory.
- `-s /bin/bash`: đặt shell đăng nhập là Bash.
- `passwd`: đặt password cho user.
- Nếu quên `-m`, user có thể không có thư mục `/home/devopsdemo`.

## Tạo user

Tạo user bằng `adduser`:

```bash
sudo adduser devdemo
```

Kiểm tra user:

```bash
id devdemo
grep "^devdemo:" /etc/passwd
```

Kết quả mẫu:

```text
uid=1001(devdemo) gid=1001(devdemo) groups=1001(devdemo)
devdemo:x:1001:1001:Developer Demo:/home/devdemo:/bin/bash
```

Giải thích:

- User `devdemo` đã được tạo.
- Home là `/home/devdemo`.
- Shell là `/bin/bash`.
- Primary group cũng là `devdemo`.

## Xóa user

Xóa user nhưng giữ lại home:

```bash
sudo deluser devdemo
```

Xóa user và xóa luôn home:

```bash
sudo deluser --remove-home devdemo
```

Trên một số distro có thể dùng:

```bash
sudo userdel -r devdemo
```

Giải thích:

- `deluser` thân thiện hơn trên Ubuntu/Debian.
- `userdel -r` xóa user và home directory.
- Cẩn thận khi xóa user thật vì có thể mất dữ liệu trong home.

Kiểm tra user còn tồn tại không:

```bash
id devdemo
```

Kết quả mẫu nếu user đã bị xóa:

```text
id: 'devdemo': no such user
```

## Tạo group

Tạo group:

```bash
sudo groupadd developers
```

Kiểm tra group:

```bash
grep "^developers:" /etc/group
```

Kết quả mẫu:

```text
developers:x:1002:
```

Giải thích:

- Group `developers` đã được tạo.
- `1002` là GID.
- Phần members đang trống vì chưa thêm user nào.

## Thêm user vào group

Thêm user vào group phụ:

```bash
sudo usermod -aG developers devdemo
```

Giải thích:

- `usermod`: sửa thông tin user.
- `-G developers`: đặt supplementary group.
- `-a`: append, thêm vào group mới mà không xóa group cũ.
- Luôn dùng `-aG` khi thêm group phụ. Nếu dùng `-G` mà quên `-a`, bạn có thể làm mất các group phụ hiện tại của user.

Kiểm tra:

```bash
id devdemo
groups devdemo
```

Kết quả mẫu:

```text
uid=1001(devdemo) gid=1001(devdemo) groups=1001(devdemo),1002(developers)
devdemo : devdemo developers
```

Lưu ý:

- Nếu đang đăng nhập bằng user vừa được thêm group, bạn cần logout/login lại để group mới có hiệu lực.
- Trong WSL, có thể đóng terminal Ubuntu rồi mở lại.

## Xóa user khỏi group

Trên Ubuntu/Debian:

```bash
sudo deluser devdemo developers
```

Kiểm tra lại:

```bash
groups devdemo
```

Kết quả mẫu:

```text
devdemo : devdemo
```

Giải thích:

- User `devdemo` không còn thuộc group `developers`.

## Xóa group

Xóa group:

```bash
sudo groupdel developers
```

Kiểm tra:

```bash
grep "^developers:" /etc/group
```

Nếu không có output, group đã bị xóa.

Lưu ý:

- Không xóa group hệ thống nếu không chắc.
- Nếu group đang là primary group của user nào đó, bạn cần đổi primary group trước.

## Đọc output `ls -l`

Chạy:

```bash
ls -l
```

Kết quả mẫu:

```text
-rw-r--r-- 1 student developers  24 Jul 14 09:10 app.conf
drwxr-x--- 2 student developers 4.0K Jul 14 09:12 scripts
```

Dòng file có dạng:

```text
-rw-r--r-- 1 student developers 24 Jul 14 09:10 app.conf
```

Giải thích:

| Phần | Ý nghĩa |
| --- | --- |
| `-rw-r--r--` | Loại file và quyền truy cập |
| `1` | Số hard link |
| `student` | Owner user |
| `developers` | Owner group |
| `24` | Dung lượng byte |
| `Jul 14 09:10` | Thời gian sửa gần nhất |
| `app.conf` | Tên file |

Ký tự đầu tiên:

| Ký tự | Ý nghĩa |
| --- | --- |
| `-` | File thường |
| `d` | Directory |
| `l` | Symbolic link |
| `c` | Character device |
| `b` | Block device |

## Permission `r`, `w`, `x`

Linux chia quyền thành 3 nhóm:

```text
user   group  others
rwx    rwx    rwx
```

Ý nghĩa với file:

| Quyền | Tên | Với file |
| --- | --- | --- |
| `r` | read | Đọc nội dung file |
| `w` | write | Sửa nội dung file |
| `x` | execute | Chạy file như chương trình/script |

Ý nghĩa với directory:

| Quyền | Tên | Với directory |
| --- | --- | --- |
| `r` | read | Liệt kê tên file bên trong |
| `w` | write | Tạo/xóa/đổi tên file bên trong |
| `x` | execute | Đi vào thư mục và truy cập file bên trong |

Điểm dễ nhầm:

- Với directory, `x` không phải "chạy thư mục".
- Directory cần `x` để `cd` vào.
- Muốn liệt kê được nội dung thư mục thường cần cả `r` và `x`.
- Muốn tạo/xóa file trong thư mục thường cần `w` và `x` trên thư mục.

## Giải mã `-rw-r--r--`

Ví dụ:

```text
-rw-r--r--
```

Tách ra:

```text
-   rw-   r--   r--
|   |     |     |
|   |     |     others
|   |     group
|   user
file type
```

Ý nghĩa:

| Nhóm | Quyền | Ý nghĩa |
| --- | --- | --- |
| User | `rw-` | Owner được đọc và ghi |
| Group | `r--` | Group chỉ được đọc |
| Others | `r--` | Người khác chỉ được đọc |

Ví dụ directory:

```text
drwxr-x---
```

Ý nghĩa:

| Nhóm | Quyền | Ý nghĩa |
| --- | --- | --- |
| User | `rwx` | Owner đọc, ghi, đi vào được |
| Group | `r-x` | Group đọc và đi vào được, không ghi |
| Others | `---` | Người khác không có quyền |

## Quyền dạng số

Mỗi quyền có giá trị:

| Quyền | Số |
| --- | --- |
| `r` | 4 |
| `w` | 2 |
| `x` | 1 |

Cộng lại để ra quyền:

| Số | Quyền | Ý nghĩa |
| --- | --- | --- |
| `7` | `rwx` | Đọc, ghi, chạy |
| `6` | `rw-` | Đọc, ghi |
| `5` | `r-x` | Đọc, chạy |
| `4` | `r--` | Chỉ đọc |
| `0` | `---` | Không quyền |

Ví dụ `chmod 755`:

```text
755 = user 7, group 5, others 5
    = rwx, r-x, r-x
```

Ví dụ `chmod 644`:

```text
644 = user 6, group 4, others 4
    = rw-, r--, r--
```

Các mode thường gặp:

| Mode | Quyền | Dùng khi nào |
| --- | --- | --- |
| `600` | `rw-------` | File secret, private key |
| `644` | `rw-r--r--` | File text/config đọc công khai |
| `640` | `rw-r-----` | File config chỉ owner và group đọc |
| `700` | `rwx------` | Script/thư mục riêng của owner |
| `755` | `rwxr-xr-x` | Script hoặc thư mục nhiều người cần đọc/chạy |
| `750` | `rwxr-x---` | Thư mục cho owner và group, chặn others |
| `775` | `rwxrwxr-x` | Thư mục nhóm cùng ghi, others chỉ đọc/chạy |

## `chmod`: đổi quyền truy cập

Tạo file test:

```bash
mkdir -p ~/devops-lab/permissions
cd ~/devops-lab/permissions
echo 'echo "Hello permission"' > hello.sh
ls -l hello.sh
```

Kết quả mẫu:

```text
-rw-r--r-- 1 student student 24 Jul 14 09:20 hello.sh
```

Thử chạy:

```bash
./hello.sh
```

Kết quả mẫu:

```text
bash: ./hello.sh: Permission denied
```

Giải thích:

- File chưa có quyền execute.
- Quyền hiện tại là `rw-r--r--`, không có `x`.

Thêm quyền execute:

```bash
chmod +x hello.sh
ls -l hello.sh
./hello.sh
```

Kết quả mẫu:

```text
-rwxr-xr-x 1 student student 24 Jul 14 09:20 hello.sh
Hello permission
```

Giải thích:

- `chmod +x` thêm quyền execute.
- Script chạy được.

Dùng số:

```bash
chmod 700 hello.sh
ls -l hello.sh
```

Kết quả mẫu:

```text
-rwx------ 1 student student 24 Jul 14 09:20 hello.sh
```

Giải thích:

- Owner có `rwx`.
- Group và others không có quyền.

Dùng ký hiệu:

```bash
chmod u+x hello.sh
chmod g-w hello.sh
chmod o-rwx hello.sh
```

Ý nghĩa:

| Lệnh | Ý nghĩa |
| --- | --- |
| `chmod u+x file` | Thêm execute cho owner |
| `chmod g-w file` | Bỏ write của group |
| `chmod o-rwx file` | Bỏ mọi quyền của others |
| `chmod a+r file` | Thêm read cho tất cả |

## `chown`: đổi owner user/group

Công dụng: đổi người sở hữu file/thư mục.

Tạo file:

```bash
touch app.conf
ls -l app.conf
```

Kết quả mẫu:

```text
-rw-r--r-- 1 student student 0 Jul 14 09:30 app.conf
```

Đổi owner:

```bash
sudo chown devdemo app.conf
ls -l app.conf
```

Kết quả mẫu:

```text
-rw-r--r-- 1 devdemo student 0 Jul 14 09:30 app.conf
```

Giải thích:

- Owner user đổi từ `student` sang `devdemo`.
- Owner group vẫn là `student`.

Đổi cả user và group:

```bash
sudo chown devdemo:developers app.conf
ls -l app.conf
```

Kết quả mẫu:

```text
-rw-r--r-- 1 devdemo developers 0 Jul 14 09:30 app.conf
```

Giải thích:

- Owner user là `devdemo`.
- Owner group là `developers`.

Đổi owner đệ quy cho thư mục:

```bash
sudo chown -R devdemo:developers project/
```

Cẩn thận:

- `-R` áp dụng cho toàn bộ file/thư mục con.
- Trước khi dùng `-R`, luôn kiểm tra `pwd` và `ls`.
- Không chạy đệ quy trên thư mục hệ thống nếu không chắc.

## `chgrp`: đổi group sở hữu

Công dụng: đổi owner group nhưng giữ owner user.

Ví dụ:

```bash
sudo chgrp developers app.conf
ls -l app.conf
```

Kết quả mẫu:

```text
-rw-r--r-- 1 student developers 0 Jul 14 09:35 app.conf
```

Giải thích:

- Owner user vẫn là `student`.
- Owner group đổi thành `developers`.

## `umask`: quyền mặc định khi tạo file/thư mục

`umask` quyết định quyền mặc định bị trừ đi khi tạo file/thư mục mới.

Xem umask hiện tại:

```bash
umask
```

Kết quả mẫu:

```text
0022
```

Với `umask 022`, quyền mặc định thường là:

| Loại | Quyền mặc định thường thấy |
| --- | --- |
| File | `644` hoặc `rw-r--r--` |
| Directory | `755` hoặc `rwxr-xr-x` |

Ví dụ:

```bash
umask 022
touch file-022.txt
mkdir dir-022
ls -ld file-022.txt dir-022
```

Kết quả mẫu:

```text
drwxr-xr-x 2 student student 4096 Jul 14 09:40 dir-022
-rw-r--r-- 1 student student    0 Jul 14 09:40 file-022.txt
```

Giải thích:

- Directory có `755`.
- File có `644`.
- File mới thường không tự có execute.

Ví dụ chặt hơn:

```bash
umask 077
touch file-077.txt
mkdir dir-077
ls -ld file-077.txt dir-077
```

Kết quả mẫu:

```text
drwx------ 2 student student 4096 Jul 14 09:41 dir-077
-rw------- 1 student student    0 Jul 14 09:41 file-077.txt
```

Giải thích:

- Chỉ owner có quyền.
- Group và others không có quyền.

## Quyền đặc biệt: SUID, SGID, Sticky Bit

Ngoài `rwx`, Linux còn có một số quyền đặc biệt.

### SUID

SUID làm chương trình chạy với quyền của owner file.

Ví dụ thường gặp:

```bash
ls -l /usr/bin/passwd
```

Kết quả mẫu:

```text
-rwsr-xr-x 1 root root 68208 Jul 14 09:45 /usr/bin/passwd
```

Giải thích:

- Chữ `s` ở phần quyền user là SUID.
- Lệnh `passwd` cần sửa dữ liệu mật khẩu, nên phải chạy với quyền đặc biệt.
- Không tự ý đặt SUID cho script/app nếu không hiểu rủi ro bảo mật.

### SGID

SGID trên directory làm file mới bên trong thừa hưởng group của directory.

Ví dụ:

```bash
mkdir shared
sudo chgrp developers shared
chmod 2775 shared
ls -ld shared
```

Kết quả mẫu:

```text
drwxrwsr-x 2 student developers 4096 Jul 14 09:50 shared
```

Giải thích:

- Chữ `s` ở phần group là SGID.
- File/thư mục mới trong `shared` thường sẽ thuộc group `developers`.
- Hữu ích cho thư mục làm việc nhóm.

### Sticky Bit

Sticky bit thường dùng cho thư mục chung, ví dụ `/tmp`.

Kiểm tra:

```bash
ls -ld /tmp
```

Kết quả mẫu:

```text
drwxrwxrwt 18 root root 4096 Jul 14 09:55 /tmp
```

Giải thích:

- Chữ `t` ở cuối là sticky bit.
- Nhiều user có thể tạo file trong `/tmp`.
- Nhưng user thường chỉ xóa file của chính họ, không xóa file của người khác.

## ACL: phân quyền chi tiết hơn

Permission cơ bản chỉ có user, group, others. ACL cho phép cấp quyền chi tiết cho user/group cụ thể.

Kiểm tra ACL:

```bash
getfacl app.conf
```

Kết quả mẫu:

```text
# file: app.conf
# owner: student
# group: developers
user::rw-
group::r--
other::r--
```

Cấp quyền read/write cho user cụ thể:

```bash
setfacl -m u:devdemo:rw app.conf
getfacl app.conf
```

Kết quả mẫu:

```text
user::rw-
user:devdemo:rw-
group::r--
mask::rw-
other::r--
```

Giải thích:

- `user:devdemo:rw-` nghĩa là user `devdemo` được đọc và ghi file này.
- ACL hữu ích khi không muốn đổi owner/group chính.

Nếu chưa có lệnh ACL:

```bash
sudo apt install -y acl
```

Gỡ ACL của user:

```bash
setfacl -x u:devdemo app.conf
```

## Debug lỗi `Permission denied`

Khi gặp:

```text
Permission denied
```

Kiểm tra theo thứ tự:

1. Tôi đang là user nào?

```bash
whoami
id
```

2. File/thư mục thuộc owner/group nào?

```bash
ls -l file
ls -ld directory
```

3. Tôi có thuộc group sở hữu không?

```bash
groups
```

4. Với file, tôi cần quyền gì?

| Hành động | Quyền cần |
| --- | --- |
| Đọc file | `r` |
| Sửa file | `w` |
| Chạy script | `x` |

5. Với directory, tôi cần quyền gì?

| Hành động | Quyền cần |
| --- | --- |
| `cd` vào thư mục | `x` |
| `ls` nội dung | `r` và thường cần `x` |
| Tạo file trong thư mục | `w` và `x` |
| Xóa file trong thư mục | `w` và `x` trên thư mục |

6. Có cần `sudo` không?

```bash
sudo command
```

Không dùng `sudo` để che lỗi nếu vấn đề đúng ra nên sửa owner/group/permission.

## Các lệnh quan trọng

| Lệnh | Công dụng | Ví dụ |
| --- | --- | --- |
| `whoami` | Xem user hiện tại | `whoami` |
| `id` | Xem UID, GID, group | `id` |
| `groups` | Xem group của user | `groups devdemo` |
| `adduser` | Tạo user thân thiện trên Ubuntu | `sudo adduser devdemo` |
| `useradd` | Tạo user cấp thấp | `sudo useradd -m -s /bin/bash devopsdemo` |
| `passwd` | Đặt/đổi password | `sudo passwd devdemo` |
| `deluser` | Xóa user trên Ubuntu | `sudo deluser --remove-home devdemo` |
| `userdel` | Xóa user cấp thấp | `sudo userdel -r devdemo` |
| `groupadd` | Tạo group | `sudo groupadd developers` |
| `groupdel` | Xóa group | `sudo groupdel developers` |
| `usermod -aG` | Thêm user vào group phụ | `sudo usermod -aG developers devdemo` |
| `chmod` | Đổi permission | `chmod 755 script.sh` |
| `chown` | Đổi owner | `sudo chown user:group file` |
| `chgrp` | Đổi group sở hữu | `sudo chgrp developers file` |
| `umask` | Xem/đặt quyền mặc định | `umask 022` |
| `getfacl` | Xem ACL | `getfacl file` |
| `setfacl` | Sửa ACL | `setfacl -m u:devdemo:rw file` |

## Lab 1: tạo user, group và phân quyền thư mục

Mục tiêu: tạo một group `developers`, user `devdemo`, thư mục dùng chung và phân quyền cho group.

Chạy trong Ubuntu WSL:

```bash
sudo groupadd developers
sudo useradd -m -s /bin/bash devdemo
sudo passwd devdemo
sudo usermod -aG developers devdemo
id devdemo
```

Kết quả mẫu:

```text
uid=1001(devdemo) gid=1001(devdemo) groups=1001(devdemo),1002(developers)
```

Tạo thư mục dùng chung:

```bash
sudo mkdir -p /opt/devops-shared
sudo chown root:developers /opt/devops-shared
sudo chmod 2775 /opt/devops-shared
ls -ld /opt/devops-shared
```

Kết quả mẫu:

```text
drwxrwsr-x 2 root developers 4096 Jul 14 10:10 /opt/devops-shared
```

Giải thích:

- Owner user là `root`.
- Owner group là `developers`.
- Group có quyền ghi vì có `rwx`.
- SGID bật vì có `s`, giúp file mới thừa hưởng group `developers`.

## Lab 2: hiểu `chmod` bằng file script

Tạo script:

```bash
mkdir -p ~/devops-lab/permissions
cd ~/devops-lab/permissions
echo 'echo "permission lab"' > run.sh
ls -l run.sh
```

Kết quả mẫu:

```text
-rw-r--r-- 1 student student 22 Jul 14 10:15 run.sh
```

Thử chạy:

```bash
./run.sh
```

Kết quả mẫu:

```text
bash: ./run.sh: Permission denied
```

Sửa quyền:

```bash
chmod 755 run.sh
ls -l run.sh
./run.sh
```

Kết quả mẫu:

```text
-rwxr-xr-x 1 student student 22 Jul 14 10:15 run.sh
permission lab
```

Giải thích:

- `755` nghĩa là owner `rwx`, group `r-x`, others `r-x`.
- Script chạy được vì có quyền `x`.

## Dọn dẹp lab

Nếu bạn đã tạo user/group demo và muốn dọn:

```bash
sudo deluser --remove-home devdemo
sudo groupdel developers
sudo rm -r /opt/devops-shared
```

Cẩn thận:

- Chỉ chạy dọn dẹp nếu chắc đây là user/group/thư mục demo.
- Không xóa user/group thật đang dùng cho công việc.

## Lỗi thường gặp

| Lỗi | Nguyên nhân thường gặp | Cách kiểm tra/sửa |
| --- | --- | --- |
| `Permission denied` khi chạy script | File thiếu quyền execute | `ls -l script.sh`, sau đó `chmod +x script.sh` |
| Không sửa được file config | User không phải owner, group không có write | `ls -l file`, cân nhắc `sudo`, `chown`, `chmod` |
| Đã thêm user vào group nhưng chưa có hiệu lực | Session cũ chưa nhận group mới | Logout/login lại hoặc mở terminal mới |
| `useradd` tạo user nhưng không có home | Quên option `-m` | Dùng `sudo useradd -m -s /bin/bash user` |
| Dùng `usermod -G` làm mất group cũ | Quên `-a` | Dùng `sudo usermod -aG group user` |
| Directory có `w` nhưng vẫn không vào được | Thiếu quyền `x` trên directory | Thêm execute phù hợp, ví dụ `chmod g+x dir` |
| File secret quá mở | Mode như `644` cho phép others đọc | Dùng `chmod 600 secret-file` |

## Checklist hoàn thành

- [ ] Giải thích được user, group, UID, GID.
- [ ] Đọc được một dòng trong `/etc/passwd`.
- [ ] Đọc được một dòng trong `/etc/group`.
- [ ] Phân biệt được `useradd` và `adduser`.
- [ ] Tạo và xóa user demo.
- [ ] Tạo group và thêm user vào group.
- [ ] Đọc được output `ls -l`.
- [ ] Giải thích được `r`, `w`, `x` với file và directory.
- [ ] Hiểu `chmod 755`, `chmod 644`, `chmod 600`.
- [ ] Dùng được `chown`, `chgrp`, `umask`.
- [ ] Biết cách debug `Permission denied`.

## Câu hỏi ôn tập

- User và group khác nhau thế nào?
- UID và GID dùng để làm gì?
- `/etc/passwd` chứa những trường nào?
- Vì sao password hash nằm trong `/etc/shadow` thay vì `/etc/passwd`?
- `adduser` khác `useradd` như thế nào?
- Vì sao nên dùng `usermod -aG` thay vì chỉ `usermod -G`?
- Trong `-rw-r--r--`, owner có quyền gì?
- Với directory, quyền `x` có ý nghĩa gì?
- `chmod 755` nghĩa là gì?
- Khi nào nên dùng `chmod 600`?
- `chown user:group file` thay đổi điều gì?
- `umask 022` thường tạo file và directory với quyền nào?
- Sticky bit trên `/tmp` giúp giải quyết vấn đề gì?
- Khi gặp `Permission denied`, bạn kiểm tra những lệnh nào trước?

## Liên kết nội bộ

- [Hệ thống file trong Linux](he-thong-file-linux.md)
- [Các lệnh cơ bản trong Linux](cac-lenh-co-ban.md)
- [Tạo Linux server bằng WSL để thực hành](wsl-linux-server.md)
- [Security](../11-security/index.md)
