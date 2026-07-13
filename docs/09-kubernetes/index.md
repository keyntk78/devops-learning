# Module 9: Kubernetes

## Mục tiêu

- Hiểu Kubernetes như một control plane điều phối container.
- Deploy app bằng manifest.
- Expose service, quản lý config, scale và đọc logs.

## Chủ đề chính

- Cluster, node, namespace
- Pod, ReplicaSet, Deployment
- Service: ClusterIP, NodePort, LoadBalancer
- Ingress
- ConfigMap, Secret
- Volume và PersistentVolumeClaim
- Probe: liveness, readiness, startup
- Helm và GitOps cơ bản

## Lab gợi ý

Deploy app demo vào Kubernetes local:

- Tạo namespace
- Tạo Deployment
- Tạo Service
- Thêm ConfigMap
- Thêm readiness probe
- Scale replicas
- Xem logs và events

## Câu hỏi ôn tập

- Pod khác container thế nào?
- Readiness probe ảnh hưởng routing ra sao?
- Service chọn pod bằng cách nào?
- Vì sao không nên sửa trực tiếp object production bằng tay?

