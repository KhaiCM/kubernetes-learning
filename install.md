## Các thành phần cần cài

Trước khi cài Minikube, bạn cần 3 công cụ chính:

| Công cụ      | Vai trò                                     | Gợi ý cài đặt                        |
| ------------ | ------------------------------------------- | ------------------------------------ |
| **Docker**   | Dùng làm driver chạy container cho Minikube | Đã có sẵn nếu bạn dùng để học Docker |
| **kubectl**  | CLI để tương tác với cluster Kubernetes     | Cài riêng                            |
| **Minikube** | Tạo & quản lý cluster local                 | Cài riêng                            |

### Cài đặt từng bước

#### 1 — Cài kubectl

```bash
sudo apt update
sudo apt install -y curl
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client
```
Check:
> kubectl version --client

#### 2 — Cài Minikube

> curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

Check:
> minikube version

####  3 — Start cluster

> minikube start --driver=docker

Check:
> minikube status

#### 4 - Kiểm tra hoạt động

> kubectl get nodes

```
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   2m    v1.34.0
```

**Chạy thử 1 pod “hello world”:**

```
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl expose deployment hello-minikube --type=NodePort --port=8080
minikube service hello-minikube
```

### 5. Một số lệnh cơ bản với Minikube

| Mục đích               | Lệnh                    |
| ---------------------- | ----------------------- |
| Xem danh sách cluster  | `minikube profile list` |
| Dừng cluster           | `minikube stop`         |
| Mở dashboard trực quan | `minikube dashboard`    |
| SSH vào node           | `minikube ssh`          |
| Xóa cluster            | `minikube delete`       |
| Xóa 1 pod              | `kubectl delete pod <name>`                    |
| Xóa từ YAML            | `kubectl delete -f <file>.yaml`                |
| Xóa Deployment         | `kubectl delete deployment <name>`             |
| Tạm dừng workload      | `kubectl scale deployment <name> --replicas=0` |

