# Tạo Linux server bằng WSL để thực hành

## Mục tiêu

- Hiểu WSL là gì và vì sao phù hợp để bắt đầu học DevOps trên Windows.
- Biết khi nào nên dùng WSL, khi nào nên dùng máy ảo như VMware.
- Setup một môi trường Ubuntu đủ tốt để thực hành Linux, shell, service, package, networking cơ bản và các bài DevOps sau này.

## WSL là gì?

WSL là viết tắt của **Windows Subsystem for Linux**. Đây là cách chạy môi trường Linux trực tiếp bên trong Windows.

Với WSL2, Windows chạy một Linux kernel thật trong môi trường nhẹ hơn máy ảo truyền thống. Bạn có thể mở Ubuntu, dùng terminal Linux, cài package bằng `apt`, chạy service, Docker, Git, SSH, curl, systemd và nhiều công cụ DevOps khác.

Trên máy hiện tại:

```text
Default Distribution: Ubuntu
Default Version: 2
Ubuntu: Running, WSL2
Systemd: running
```

Nghĩa là máy đã có sẵn Ubuntu trên WSL2, đủ để bắt đầu thực hành.

## Vì sao bài đầu tiên dùng WSL thay vì VMware?

WSL phù hợp cho giai đoạn đầu vì:

- Cài và mở nhanh hơn máy ảo.
- Nhẹ hơn VMware, ít tốn RAM/CPU hơn.
- Dùng chung file với Windows khá tiện.
- Terminal Linux gần giống môi trường server thật.
- Đủ để học Linux command, Bash, package, service, Git, Docker, CI tool, Kubernetes local.

VMware phù hợp hơn khi bạn cần:

- Mô phỏng một máy Linux độc lập hoàn toàn.
- Học boot process, disk partition, network adapter, snapshot sâu hơn.
- Chạy nhiều server riêng biệt như một lab network.
- Thực hành firewall/router/network topology phức tạp.
- Test hệ điều hành khác nhau với kernel và phần cứng ảo riêng.

Kết luận thực tế:

- Giai đoạn đầu học DevOps: dùng WSL để học nhanh và đỡ vướng setup.
- Khi học sâu về system administration hoặc networking: bổ sung VMware/VirtualBox/Proxmox sau.

## Cách mở Ubuntu WSL

Từ PowerShell:

```powershell
wsl
```

Hoặc chỉ định distro:

```powershell
wsl -d Ubuntu
```

Kiểm tra danh sách distro:

```powershell
wsl --list --verbose
```

Kết quả mong muốn:

```text
NAME      STATE    VERSION
Ubuntu    Running  2
```

## Nếu máy chưa có WSL

Chạy PowerShell bằng quyền Administrator:

```powershell
wsl --install -d Ubuntu
```

Sau đó restart máy nếu Windows yêu cầu. Khi mở Ubuntu lần đầu, tạo username và password Linux riêng.

Kiểm tra WSL version:

```powershell
wsl --status
wsl --list --verbose
```

Nếu distro đang ở WSL1, chuyển sang WSL2:

```powershell
wsl --set-version Ubuntu 2
```

## Setup Ubuntu để thực hành DevOps

Trong terminal Ubuntu WSL:

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl wget git vim nano tree htop net-tools dnsutils iproute2 unzip ca-certificates
```

Kiểm tra thông tin hệ thống:

```bash
whoami
pwd
uname -a
cat /etc/os-release
ip addr
df -h
free -h
```

Kiểm tra systemd:

```bash
ps -p 1 -o comm=
systemctl is-system-running
```

Nếu kết quả có `systemd` và trạng thái `running` hoặc `degraded`, bạn có thể thực hành quản lý service bằng `systemctl`.

## Tạo thư mục lab

Trong Ubuntu WSL:

```bash
mkdir -p ~/devops-lab/{scripts,logs,apps,notes}
cd ~/devops-lab
pwd
tree -L 2 .
```

Ý nghĩa:

| Thư mục | Dùng để làm gì |
| --- | --- |
| `scripts` | Lưu Bash script thực hành |
| `logs` | Lưu log giả lập hoặc log bài lab |
| `apps` | Lưu app demo |
| `notes` | Ghi chú nhanh trong Linux |

## Cài SSH server để mô phỏng server

Trong WSL, bạn không bắt buộc phải SSH vào Ubuntu vì có thể mở trực tiếp bằng `wsl`. Nhưng cài SSH giúp mô phỏng cách kết nối server thật.

Trong Ubuntu WSL:

```bash
sudo apt install -y openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
```

Lấy IP của WSL:

```bash
hostname -I
```

Từ PowerShell, thử SSH:

```powershell
ssh <linux-user>@localhost
```

Nếu port mặc định không hoạt động, kiểm tra:

```bash
sudo ss -lntp | grep ':22'
```

## Bài thực hành đầu tiên

Tạo file:

```bash
nano ~/devops-lab/scripts/hello-server.sh
```

Nội dung:

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "Hello DevOps"
echo "User: $(whoami)"
echo "Host: $(hostname)"
echo "Kernel: $(uname -r)"
echo "Disk:"
df -h /
```

Chạy script:

```bash
chmod +x ~/devops-lab/scripts/hello-server.sh
~/devops-lab/scripts/hello-server.sh
```

Mục tiêu của bài này là quen với:

- Shebang `#!/usr/bin/env bash`
- Quyền execute bằng `chmod +x`
- Lệnh hệ thống cơ bản
- Output của script

## Lỗi thường gặp

| Lỗi | Nguyên nhân thường gặp | Cách xử lý |
| --- | --- | --- |
| `wsl: command not found` | Windows chưa bật WSL | Chạy `wsl --install -d Ubuntu` bằng PowerShell Administrator |
| Không nhớ password sudo | Password tạo lúc mở Ubuntu lần đầu | Reset password hoặc tạo user mới nếu cần |
| `systemctl` không chạy | Distro chưa bật systemd hoặc đang dùng WSL cũ | Kiểm tra `ps -p 1 -o comm=` và cập nhật WSL |
| SSH không vào được | Service chưa chạy hoặc port khác | Kiểm tra `systemctl status ssh` và `ss -lntp` |
| File Windows chạy lỗi trong Linux | Khác line ending CRLF/LF hoặc permission | Tạo script trực tiếp trong Linux và `chmod +x` |

## Checklist hoàn thành

- [x] Mở được Ubuntu bằng `wsl`.
- [x] Biết kiểm tra distro bằng `wsl --list --verbose`.
- [x] Chạy được `sudo apt update`.
- [x] Tạo được thư mục `~/devops-lab`.
- [x] Chạy được script `hello-server.sh`.
- [x] Hiểu vì sao dùng WSL trước, VMware sau.

## Câu hỏi ôn tập

- WSL là gì?
- WSL2 khác máy ảo VMware ở điểm nào?
- Vì sao WSL phù hợp để bắt đầu học DevOps?
- Khi nào nên dùng VMware thay vì WSL?
- `sudo apt update` dùng để làm gì?
- `chmod +x` có ý nghĩa gì?
- `systemctl status ssh` giúp kiểm tra điều gì?

## Liên kết nội bộ

- [Module Linux và Shell](index.md)
- [Command Index](../cheatsheets/command-index.md)
- [Networking](../03-networking/index.md)
- [Containers](../05-containers/index.md)
