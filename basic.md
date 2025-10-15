## What is Kubernetes?

Kubernetes là một hệ thống mã nguồn mở dùng để tự động hóa việc deployment, scaling và quản lý các ứng dụng được chứa trong container”.

## The benefits of Kubernetes

Kubernetes có khả năng di động cao trên nhiều nền tảng đám mây và đơn giản hóa việc quản lý container trên bất kỳ nền tảng nào đang được sử dụng. Kubernetes giúp dễ dàng đạt được khả năng scalability, flexibility và productivity.

## What features does Kubernetes offer?

Kubernetes là một trong những dự án phần mềm nguồn mở phát triển nhanh nhất hiện nay. Dưới đây là một số lý do:
- Có thể gửi các triển khai đến một đám mây hoặc nhiều dịch vụ đám mây mà không làm mất bất kỳ chức năng hoặc hiệu suất nào của ứng dụng.
- Khả năng tự động hóa của Kubernetes xử lý việc lập lịch và triển khai container bất kể container đó đến từ đâu (tại chỗ, đám mây hoặc các nền tảng khác). Tính năng tự động   hóa cũng tự động tăng hoặc giảm quy mô để tăng hiệu quả và giảm thiểu lãng phí, đồng thời tạo ra các container mới nếu khối lượng công việc lớn.
- Kubernetes cho phép khôi phục lại thay đổi của ứng dụng nếu có sự cố xảy ra.
- Bản chất mã nguồn mở của Kubernetes cho phép người dùng tận dụng hệ sinh thái rộng lớn của các công cụ mã nguồn mở.
- Phần mềm không bao giờ lỗi thời do đã có phiên bản trước đó – nó luôn được cập nhật.

## The role of containers

Container là một công nghệ gọn nhẹ cho phép bạn chạy ứng dụng và các dependencies của nó một cách an toàn mà không ảnh hưởng đến các container khác hoặc hệ điều hành (HĐH) của bạn. Điều này giúp container linh hoạt và có khả năng mở rộng hơn so với việc sử dụng các công cụ quản lý ứng dụng khác, chẳng hạn như máy ảo (VM) hoặc máy chủ vật lý (bare metal). Giống như VM, container có thể repeat ứng dụng khi nó đang được phát triển, nhưng không giống như VM, container không sao chép hệ điều hành mỗi lần mà thay vào đó chia sẻ cơ sở hạ tầng, công nghệ container (ví dụ: Docker) và hệ điều hành với máy chủ. Container gọn nhẹ và dễ chạy hơn trên đám mây vì hệ điều hành không bị sao chép cùng với ứng dụng, nhưng công nghệ container có thể khó quản lý nếu không có công cụ.

## Kubernetes Architecture

Kubernetes có thể được hình dung như một hệ thống được xây dựng theo từng layers, với mỗi layers cao hơn trừu tượng hóa độ phức tạp của các layers thấp hơn.

Kubernetes tập hợp các máy vật lý hoặc máy ảo riêng lẻ thành một cluster, sử dụng shared network để giao tiếp giữa mỗi server, dù là máy vật lý hay máy ảo. cluster Kubernetes này là nền tảng vật lý nơi tất cả các Kubernetes components, capabilities và workloads của Kubernetes được cấu hình.

Mỗi server trong cluster Kubernetes đều được giao một vai trò trong hệ sinh thái Kubernetes. Một server (hoặc một nhóm nhỏ trong các triển khai có tính khả dụng cao) hoạt động như master server . Server này đóng vai trò là gateway và bộ não cho cluster bằng cách cung cấp API Kubernetes cho user và client, kiểm tra tình trạng các server khác, quyết định cách phân chia và phân công công việc tốt nhất (được gọi là "scheduling") và điều phối giao tiếp giữa các components khác (đôi khi được gọi là điều phối container). Master server đóng vai trò là primary point với cluster và chịu trách nhiệm cho hầu hết các logic tập trung mà Kubernetes cung cấp.

Các server khác trong cluster được chỉ định là các nút (node) : server chịu trách nhiệm tiếp nhận và chạy workloads bằng local and resources bên ngoài. Để hỗ trợ việc isolation, management, và flexibility, Kubernetes chạy các applications và services trong các container , vì vậy mỗi node cần được trang bị một môi trường chạy container (như Docker hoặc rkt). Node này nhận lệnh làm việc từ master server và tạo hoặc hủy container tương ứng, đồng thời điều chỉnh các networking rules để định tuyến và forward traffic một cách phù hợp.

Như đã đề cập ở trên, các applications và services được chạy trên cụm bên trong các container. Các thành phần cơ bản đảm bảo trạng thái mong muốn của các ứng dụng khớp với trạng thái thực tế của cluster. User tương tác với cluster bằng cách giao tiếp trực tiếp với main Kubernetes API server hoặc với client và libraries. Để khởi động một application or service, một a declarative plan được gửi dưới dạng JSON hoặc YAML, xác định những gì cần tạo và cách quản lý nó. Sau đó, Master server sẽ tiếp nhận plan và tìm ra cách chạy nó trên infrastructure bằng cách kiểm tra các yêu cầu và trạng thái hiện tại của hệ thống. Nhóm các ứng dụng do người dùng định nghĩa này chạy theo một kế hoạch cụ thể đại diện cho layer cuối cùng của Kubernetes.

## Master Server Components

Master server đóng vai trò là primary control plane cho các cluster Kubernetes. Nó đóng vai trò là main contact point for administrators và users, đồng thời cung cấp nhiều hệ thống cluster-wide cho các node làm việc tương đối đơn giản. Nhìn chung, các thành phần trên master server phối hợp với nhau để tiếp nhận request của user, xác định cách tốt nhất để lên schedule cho các container workload containers, authenticate clients and nodes, điều chỉnh cluster-wide networking và quản lý scaling and health checking responsibilities

Các thành phần này có thể được cài đặt trên một server duy nhất hoặc phân phối trên nhiều server.

### etcd

Một trong những thành phần cơ bản mà Kubernetes cần để hoạt động là kho lưu trữ cấu hình có sẵn trên toàn cầu. Project etcd được phát triển bởi nhóm CoreOS (operating system), là một stores key-value distributed, lightweight, có thể được config để trải rộng trên nhiều node. etcd lưu toàn bộ dữ liệu cấu hình và trạng thái của Kubernetes

Kubernetes sử dụng etcd để lưu trữ data config, có thể được access bởi từng node trong cluster. data này có thể được sử dụng để service discovery và giúp các component tự config hoặc config lại theo thông tin cập nhật. Nó cũng giúp maintain cluster state với các tính năng như leader election và  distributed locking. Bằng cách cung cấp API HTTP/JSON đơn giản, interface để thiết lập hoặc truy xuất giá trị rất trực quan.

Giống như hầu hết các thành phần khác trong control plane, etcd có thể được config trên một master server duy nhất hoặc, trong production được phân phối giữa nhiều server. Yêu cầu duy nhất là nó phải có thể truy cập mạng được từ mỗi server Kubernetes.

- etcd được thiết kế phân tán → có thể chạy trên nhiều node khác nhau (gọi là etcd cluster).
- Mỗi thay đổi (vd: tạo Pod, xóa Service) đều được ghi vào etcd thông qua API Server.
- Các thành phần khác của Kubernetes (Controller, Scheduler, Kubelet…) sẽ đọc / xem thay đổi trong etcd thông qua API Server để cập nhật hành động tương ứng.

Ex Etcd lưu gì
| Thành phần | Dữ liệu được lưu trong etcd |
| -----------|-----------------------------|
| Pod | Tên, trạng thái (Running, Pending…), vị trí Node |
| Deployment | Thông tin replicas, labels, version |
| Service | Địa chỉ IP, port, selector |
| ConfigMap / Secret | Các cấu hình và biến môi trường |
| Node | Trạng thái, thông tin sức khỏe |

=> etcd = “bộ nhớ trung tâm” của Kubernetes, lưu mọi thứ về trạng thái của cluster.
Không có etcd → Kubernetes không biết mình đang điều khiển cái gì.

### kube-apiserver

kube-apiserver là thành phần trung tâm (gateway) của toàn bộ Kubernetes cluster.
Nó là “bộ não điều phối” — nơi tất cả các thành phần khác (và cả bạn) phải giao tiếp thông qua.

| Vai trò	| Giải thích |
|-----------| -----------|
| 1. Giao tiếp trung tâm (entry point) | Mọi lệnh, thao tác, hoặc truy vấn trong Kubernetes đều đi qua API Server. |
| 2. Quản lý trạng thái cluster | API Server là cầu nối giữa người dùng ↔ etcd, đảm bảo trạng thái mong muốn (desired state) và trạng thái thực tế (actual state) luôn đồng bộ |
| 3. Giao tiếp giữa các thành phần | Controller, Scheduler, Kubelet... đều dùng API Server để đọc/ghi dữ liệu về cluster. |
| 4. Cung cấp REST API chuẩn | API Server triển khai giao tiếp theo chuẩn RESTful API, giúp bất kỳ công cụ nào (như kubectl, hoặc ứng dụng ngoài) đều có thể tương tác dễ dàng. |

Ví dụ chạy lệnh:

> kubectl create -f deployment.yaml

Luồng hoạt động:

- kubectl gửi request REST đến API Server (qua HTTPS).
- API Server xác thực (authentication), kiểm tra quyền (authorization).
- Nếu hợp lệ → ghi thông tin Deployment vào etcd (desired state).
- Controller Manager phát hiện thay đổi trong etcd → tạo ReplicaSet, Pod, v.v.
- Scheduler chọn node phù hợp cho các Pod đó.

=> Tất cả giao tiếp đều thông qua API Server, chứ không có thành phần nào nói chuyện trực tiếp với nhau.

Một số nhiệm vụ khác:

- Authentication: Kiểm tra ai đang gửi request (qua token, cert, v.v.)
- Authorization: Kiểm tra người đó có quyền thực hiện hành động không (RBAC)
- Admission Control: Kiểm tra và chặn/cho phép request trước khi ghi vào etcd
- Validation: Đảm bảo dữ liệu hợp lệ trước khi lưu

kubectl chỉ là client mặc định để bạn tương tác với API Server.
Thực tế, bạn có thể dùng bất kỳ HTTP client nào để gọi REST API (Postman, curl, v.v.) vì API Server hoạt động theo chuẩn RESTful.

### kube-controller-manager

kube-controller-manager là một service trong control plane (máy master), nó chịu trách nhiệm duy trì trạng thái mong muốn (desired state) của cluster.

Hiểu đơn giản: Controller Manager = “người giám sát” trong Kubernetes. Nó theo dõi xem hệ thống thực tế đang như thế nào, và tự động điều chỉnh để khớp với trạng thái mà người dùng đã khai báo.

#### Cách hoạt động

- Bạn khai báo cấu hình (ví dụ: Deployment.yaml), nói rằng muốn 3 replicas của một Pod.
- API Server ghi thông tin này vào etcd.
- Controller Manager theo dõi (watch) etcd thông qua API Server.
- Khi nó thấy rằng chỉ có 2 Pods đang chạy thực tế, nhưng bạn muốn 3 -> nó sẽ ra lệnh tạo thêm 1 Pod để khớp lại - desired state.

#### Các loại controller quan trọng

kube-controller-manager thực chất chạy nhiều controller nhỏ bên trong, mỗi cái phụ trách một mảng cụ thể.
Dưới đây là một số controller tiêu biểu:

Một số controller tiêu biểu:

| Tên controller | Vai trò |
|----------------|---------|
| Replication / ReplicaSet Controller | Đảm bảo số lượng bản sao (replica) của Pod đúng như khai báo |
| Node Controller | Theo dõi trạng thái của các Node (sống/chết) và phản ứng nếu Node bị mất |
| Deployment Controller |Quản lý việc rolling update, rollback của ứng dụng |
| Service Account & Token Controller | Quản lý các tài khoản dịch vụ và token cho Pod |
| Endpoint Controller | Cập nhật danh sách endpoint cho Service (Pod nào thuộc Service nào) |
| Job / CronJob Controller | Quản lý các công việc chạy một lần hoặc theo lịch |

#### Chu trình hoạt động (loop)

- Mỗi controller chạy theo một vòng lặp liên tục (control loop):
- Quan sát trạng thái hiện tại qua API Server (đọc từ etcd).
- So sánh với trạng thái mong muốn (desired state).
- Thực hiện hành động để đồng bộ hai trạng thái.

 => Quá trình này gọi là “self-healing” của Kubernetes.

### kube-scheduler

kube-scheduler là thành phần chịu trách nhiệm chọn node phù hợp để chạy Pod.

Hiểu đơn giản: kube-scheduler là “người phân việc” trong Kubernetes.

Controller Manager có thể yêu cầu: “Cần chạy 3 Pod mới cho ứng dụng A.” Nhưng không chỉ rõ chạy ở Node nào.
-> Lúc này Scheduler sẽ quyết định: Pod nào sẽ được gán (assign) vào Node nào.

#### Vai trò của kube-scheduler

| Chức năng chính | Mô tả |
|-----------------|-------|
| Đọc yêu cầu (requirements) | Đọc thông tin từ Pod spec (CPU, memory, labels, affinity, v.v.) |
| Đọc thông tin cluster | Biết được Node nào đang rảnh, Node nào đã quá tải |
| Chọn Node phù hợp nhất | Dựa trên thuật toán scoring & filtering |
| Gán Pod vào Node | Ghi thông tin scheduling vào etcd qua API Server |

#### Cách hoạt động cụ thể

1. Controller Manager tạo ra một Pod (chưa có node gán) → trạng thái là Pending.
2. Scheduler đọc danh sách Pod Pending từ API Server.
3. Với mỗi Pod, Scheduler:
    - Kiểm tra yêu cầu tài nguyên (ví dụ: cần 500Mi RAM, 1 CPU).
    - Loại bỏ những Node không đủ tài nguyên.
    - Áp dụng các tiêu chí khác (nodeSelector, affinity, taints/tolerations…).
    - Tính điểm (score) cho Node nào phù hợp nhất.
4. Chọn Node có điểm cao nhất và gán Pod vào Node đó.
5. Ghi kết quả (Pod → Node mapping) vào etcd qua API Server.

#### Một số tiêu chí khi Scheduler chọn Node

| Tiêu chí                          | Ý nghĩa                                                          |
| --------------------------------- | ---------------------------------------------------------------- |
| **Resource Requests/Limits**      | Node có đủ CPU/RAM không                                         |
| **Node Affinity / Anti-affinity** | Pod có cần chạy chung hoặc tránh Node nào không                  |
| **Pod Affinity / Anti-affinity**  | Pod cần (hoặc tránh) chạy cùng Pod khác                          |
| **Taints/Tolerations**            | Node có bị “đánh dấu” đặc biệt mà Pod này có thể chịu được không |
| **Topology Spread**               | Phân bố Pod đều trên nhiều zone hoặc node khác nhau              |

### cloud-controller-manager

Cloud Controller Manager (CCM) là một thành phần trong control plane giúp Kubernetes giao tiếp với hạ tầng cloud bên ngoài.

Hiểu đơn giản: Nó là “cầu nối” giữa Kubernetes và nhà cung cấp cloud (AWS, GCP, Azure…).

#### Vì sao cần Cloud Controller Manager?

Kubernetes ban đầu được thiết kế để chạy trên nhiều môi trường khác nhau (bare metal, on-premise, cloud...).

Tuy nhiên, mỗi cloud provider lại có:
- Cách quản lý VM khác nhau
- Cách tạo Load Balancer khác nhau
- Cách gắn Storage khác nhau

Vì vậy, cần có một lớp trung gian giúp Kubernetes hiểu và giao tiếp thống nhất với từng loại hạ tầng đó — đó chính là Cloud Controller Manager.

### Nhiệm vụ chính của Cloud Controller Manager

| Nhiệm vụ | Mô tả |
| -------- | ---------------------- |
| **1. Quản lý Node trên cloud** | Khi một Node (VM instance) trong cloud bị xóa hoặc không còn tồn tại, CCM sẽ cập nhật lại trạng thái của Node đó trong Kubernetes (đánh dấu `NotReady` hoặc xóa khỏi cluster). |
| **2. Quản lý Routes / Networking** | Tạo và cập nhật route, network hoặc load balancer tương ứng với Service trong Kubernetes (ví dụ: Service type `LoadBalancer`).                                                 |
| **3. Quản lý Persistent Volumes** | Gắn (attach/detach) ổ đĩa cloud (VD: EBS của AWS, Persistent Disk của GCP) cho các Pod cần storage. |
| **4. Đồng bộ thông tin với cloud provider** | Đảm bảo thông tin tài nguyên trong Kubernetes luôn khớp với trạng thái thật trong cloud. |

#### Các thành phần con trong Cloud Controller Manager
Bên trong CCM có nhiều "controller nhỏ" tương tự như kube-controller-manager:

| Controller             | Nhiệm vụ                                                                        |
| ---------------------- | ------------------------------------------------------------------------------- |
| **Node Controller**    | Quản lý trạng thái Node dựa vào thông tin từ cloud (instance còn tồn tại không) |
| **Route Controller**   | Cấu hình routing giữa Node trong cloud                                          |
| **Service Controller** | Tạo/bỏ load balancer trên cloud khi có Service kiểu `LoadBalancer`              |
| **Volume Controller**  | Quản lý attach/detach disk cloud cho Pod                                        |

Cloud Controller Manager (CCM) là lớp trung gian giữa Kubernetes và các cloud provider

Nó giúp Kubernetes:
- Hiểu được tài nguyên của cloud,
- Tự động tạo/điều chỉnh hạ tầng (VM, storage, LB),
- Đảm bảo trạng thái trong Kubernetes khớp với thực tế ngoài cloud.

> Kube-controller-manager điều khiển logic bên trong cluster.
> Cloud-controller-manager điều khiển tài nguyên bên ngoài (ở cloud).

### Tóm tắt sự liên quan giữa các thành phần

| Thành phần                   | Vai trò chính                    | Ví dụ trong luồng                         |
| ---------------------------- | -------------------------------- | ----------------------------------------- |
| **kubectl / API Server**     | Giao tiếp và validate request    | Bạn apply YAML                            |
| **etcd**                     | Lưu trữ trạng thái cluster       | Lưu thông tin về Deployment, Pod, Service |
| **controller-manager**       | Giữ hệ thống đạt “desired state” | Tạo Pod, quản lý replicas                 |
| **scheduler**                | Chọn node để chạy workload       | Phân phối Pod lên Node                    |
| **kubelet (Node)**           | Thực thi Pod thực tế             | Kéo image và chạy container               |
| **cloud-controller-manager** | Giao tiếp với cloud provider     | Tạo LoadBalancer thật trên AWS/GCP        |

Ex: apply file YML cho ứng dụng PHP + Nginx

> kubectl apply -f php-nginx.yml

1. **kubectl** Request REST API đến kube-apiserver.
- API Server là trung tâm điều phối và điểm giao tiếp duy nhất với hệ thống Kubernetes.
2. **kube-apiserver**
- Nhận request YAML.
- Validate nội dung (Deployment, Service, v.v.).
- Lưu toàn bộ dữ liệu vào etcd (cơ sở dữ liệu chính của cluster).
- etcd sẽ chứa toàn bộ cấu hình state mong muốn của hệ thống: “Tôi muốn có 3 pod PHP-Nginx”.
3. **etcd**

- Lưu lại trạng thái mong muốn (desired state):
```
Deployment: php-nginx
Replicas: 3
Image: php:8.2-fpm + nginx:latest
Service: type LoadBalancer
```
- etcd không chạy code — nó chỉ lưu dữ liệu trạng thái toàn cục của cluster.
- Khi có thay đổi (ví dụ thêm deployment), etcd sẽ phát sự kiện thông qua kube-apiserver.
4. **kube-controller-manager**

- Controller-manager “quan sát” (watch) qua kube-apiserver để phát hiện thay đổi trong etcd.
- Nó thấy có Deployment mới, và có 3 replicas được yêu cầu.
- Nó gửi yêu cầu tạo ReplicaSet và các Pod tương ứng.
- Nó cũng quản lý Service, Endpoint, Node, v.v.
- Nếu sau này có 1 Pod bị crash → controller-manager sẽ tự tạo Pod mới để khôi phục lại đúng 3 replicas.

5. **kube-scheduler**

- Khi controller-manager yêu cầu tạo Pod mới → scheduler sẽ quyết định “chạy pod này ở node nào”.
- Scheduler dựa vào:
- Resource còn trống (CPU, RAM)
- Node selector, affinity, taints, v.v.
- Sau đó scheduler gửi thông tin quyết định này về cho kube-apiserver.

6. **Node components (kubelet, container runtime)**

- Mỗi Node (worker) có kubelet.
- kubelet sẽ hỏi kube-apiserver: “Có Pod nào được assign cho tôi không?”
- Khi có → kubelet sẽ yêu cầu container runtime (Docker / containerd) kéo image và chạy container.

7. **cloud-controller-manager (CCM)**

- Service của bạn có type: LoadBalancer
- CCM sẽ can thiệp:
    - Gọi API tới cloud provider (AWS/GCP/Azure/DigitalOcean, v.v.) để tạo Load Balancer thật.
    - Gắn LB đó vào IP public.
    - Cập nhật thông tin IP vào Service trong etcd → kube-apiserver.
- Ví dụ trên AWS:
    - CCM gọi AWS ELB → tạo 1 LoadBalancer thật.
    - LB đó tự động route đến 3 node đang chạy Pod PHP-Nginx.

8. **kube-apiserver cập nhật trạng thái**

- Nhận phản hồi từ CCM → ghi lại IP LB vào etcd.
- Cập nhật YAML “Service” cho bạn:

```
kubectl get svc
NAME                 TYPE           CLUSTER-IP     EXTERNAL-IP
php-nginx-service    LoadBalancer   10.0.0.15      18.203.90.12
```
- Giờ bạn có thể truy cập app qua http://18.203.90.12.

## Core Kubernetes concepts and definitions

- **Pod**: Pod : Một khái niệm trừu tượng đại diện cho một nhóm gồm một hoặc nhiều container ứng dụng. Brendan giải thích: “Pod chỉ là một đơn vị cho biết đây là các container đại diện cho trang web front-end, hoặc đây là các container đại diện cho hệ thống thanh toán”.
- **Node**: Một máy worker trong Kubernetes, có thể là máy ảo (VM) hoặc máy vật lý (ví dụ: máy tính), tùy thuộc vào cluster. Node thường bao gồm Docker, các pod ("nhóm container") và máy ảo hoặc máy tính chứa hệ điều hành.
- **Cluster**: Mức độ trừu tượng cao nhất trong Kubernetes, bao gồm tất cả các nodes, pod và một node chính – duy trì trạng thái mong muốn của ứng dụng bằng cách sắp xếp các nodes.
- **Service**: Định nghĩa một tập hợp logic các pod (ví dụ: "hệ thống thanh toán") và thiết lập chính sách về người có thể truy cập chúng. "Pod đến rồi đi, nhưng dịch vụ thì tồn tại mãi mãi", Brendan nói. "Một pod sẽ được lên scheduled vào một node. Nhưng nếu node đó biến mất, việc pod này là thành viên của service này đồng nghĩa với việc tôi phải tìm một nơi khác để tạo một pod mới có chứa container này đang chạy bên trong nó." Dịch vụ cho phép Kubernetes định tuyến lưu lượng đến ứng dụng của bạn bất kể pod đang chạy ở đâu.


# 🧭 Kubernetes Architecture

                       ┌──────────────────────────────┐
                       │        Control Plane         │
                       └──────────────────────────────┘
                                   │
          ┌────────────────────────┼─────────────────────────┐
          │                        │                         │
┌──────────────────┐    ┌────────────────────┐     ┌─────────────────────────┐
│  kube-apiserver  │ ⇆  │    etcd (DB)       │     │ kube-controller-manager │
│  (REST API layer)│    │   Cluster state DB │     │ Ensures desired state   │
│  Entry point for │    │   Key-value        │     │  Maintains desired state│
│  Gateway to all  │    │  Config store      │     │  (Pods, ReplicaSets...) │
│  cluster actions │    └────────────────────┘     └─────────────────────────┘
│  via kubectl, UI │
└──────────────────┘
└──────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
                        ┌────────────────────────┐
                        │     kube-scheduler     │
                        │ Decides which Node     │
                        │ runs which Pod         │
                        │ based on resources,    │
                        │ affinity, etc.         │
                        └────────────────────────┘
                                    │
                                    ▼
                            ┌──────────────────────────┐
                            │  cloud-controller-manager│
                            │ Integrates with cloud API│
                            │ (AWS, GCP, Azure...)     │
                            │ - Creates Load Balancers │
                            │ - Manages Volumes, Routes│
                            └──────────────────────────┘
                                        │
                                        ▼
       ┌─────────────────────────────────────────────────────────────────┐
       │                  Worker Nodes (Data plane)                      │
       └─────────────────────────────────────────────────────────────────┘
          │                              │                              │
   ┌──────────────┐              ┌──────────────┐                ┌──────────────┐
   │   Node #1    |              │   Node #2    |                │   Node #3    |
   │──────────────│ ⇆ API Server │──────────────│  ⇆ API Server  │──────────────│
   |   kubelet    │              │   kubelet    │                │   kubelet    │
   | (Pod manager)│              │   kubelet    │                │   kubelet    │
   │ Communicates │              │ Communicates │                │ Communicates │
   │ with API srv │              │ with API srv │                │ with API srv │
   │ Runs Pods via│              │ Runs Pods via│                │ Runs Pods via│
   │ containerd/  │              │ containerd/  │                │ containerd/  │
   │   /Docker    │              │ /Docker      │                │ /Docker      │
   └──────────────┘              └──────────────┘                └──────────────┘
          │                             │                                │
   ┌──────────────┐             ┌──────────────┐                  ┌──────────────┐
   │    Pod(s)    │             │    Pod(s)    │                  │    Pod(s)    │
   │ (Nginx, PHP, │             │ (MySQL, API) │                  │ (Vue, Cache) │
   └──────────────┘             └──────────────┘                  └──────────────┘

# 🧠 Conceptual Overview

| Layer | Component | Role |
|--------|------------|------|
| **Control Plane** | kube-apiserver | Gateway to the cluster; exposes REST API |
|                   | etcd | Stores cluster state and configuration |
|                   | kube-controller-manager | Ensures actual state matches desired state |
|                   | kube-scheduler | Assigns Pods to available Nodes |
|                   | cloud-controller-manager | Integrates Kubernetes with cloud resources |
| **Data Plane (Worker Node)** | kubelet | Runs and monitors Pods on the Node |
|                              | container runtime (Docker/containerd) | Pulls images, runs containers |
|                              | kube-proxy | Handles networking and routing for Pods |
| **Client** | kubectl / API / UI | Interface for users to interact with the cluster |

# 🧠 Data Flow (Simplified)
1. You run `kubectl apply -f app.yaml`
2. `kubectl` → sends request to **kube-apiserver**
3. **kube-apiserver** → saves configuration in **etcd**
4. **controller-manager** detects new workloads
5. **scheduler** assigns Pods to Nodes
6. **kubelet** on each Node starts containers via container runtime
7. **cloud-controller-manager** (if in cloud) provisions load balancer or storage
8. Cluster reaches desired state 🚀

