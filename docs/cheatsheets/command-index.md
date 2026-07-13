# Command Index

## Linux

```bash
pwd
ls -la
cd /path/to/dir
cat file.txt
tail -f app.log
ps aux
chmod 755 script.sh
chown user:group file
chgrp developers file
id
groups
whoami
```

## Linux Users, Groups, Permissions

```bash
sudo adduser devdemo
sudo useradd -m -s /bin/bash devopsdemo
sudo passwd devdemo
sudo deluser --remove-home devdemo
sudo groupadd developers
sudo usermod -aG developers devdemo
id devdemo
groups devdemo
ls -l
chmod 755 script.sh
chmod 600 secret.txt
sudo chown user:group file
sudo chgrp developers file
umask
getfacl file
setfacl -m u:devdemo:rw file
```

## Networking

```bash
curl -I https://example.com
nslookup example.com
ping example.com
```

PowerShell:

```powershell
Test-NetConnection example.com -Port 443
Resolve-DnsName example.com
```

## Docker

```bash
docker build -t app:local .
docker run --rm -p 8080:8080 app:local
docker ps
docker logs <container>
docker compose up
docker compose down
```

## Kubernetes

```bash
kubectl get pods
kubectl describe pod <pod>
kubectl logs <pod>
kubectl apply -f manifest.yaml
kubectl rollout status deployment/<name>
kubectl rollout undo deployment/<name>
```

## Terraform

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform destroy
```
