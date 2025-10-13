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

Về cơ bản, Kubernetes tập hợp các máy vật lý hoặc máy ảo riêng lẻ thành một cluster, sử dụng shared network để giao tiếp giữa mỗi server, dù là máy vật lý hay máy ảo. cluster Kubernetes này là nền tảng vật lý nơi tất cả các Kubernetes components, capabilities và  workloads của Kubernetes được cấu hình.

## Core Kubernetes concepts and definitions

- **Pod**: Pod : Một khái niệm trừu tượng đại diện cho một nhóm gồm một hoặc nhiều container ứng dụng. Brendan giải thích: “Pod chỉ là một đơn vị cho biết đây là các container đại diện cho trang web front-end, hoặc đây là các container đại diện cho hệ thống thanh toán”.
- **Node**: Một máy worker trong Kubernetes, có thể là máy ảo (VM) hoặc máy vật lý (ví dụ: máy tính), tùy thuộc vào cluster. Node thường bao gồm Docker, các pod ("nhóm container") và máy ảo hoặc máy tính chứa hệ điều hành.
- **Cluster**: Mức độ trừu tượng cao nhất trong Kubernetes, bao gồm tất cả các nodes, pod và một node chính – duy trì trạng thái mong muốn của ứng dụng bằng cách sắp xếp các nodes.
- **Service**: Định nghĩa một tập hợp logic các pod (ví dụ: "hệ thống thanh toán") và thiết lập chính sách về người có thể truy cập chúng. "Pod đến rồi đi, nhưng dịch vụ thì tồn tại mãi mãi", Brendan nói. "Một pod sẽ được lên scheduled vào một node. Nhưng nếu node đó biến mất, việc pod này là thành viên của service này đồng nghĩa với việc tôi phải tìm một nơi khác để tạo một pod mới có chứa container này đang chạy bên trong nó." Dịch vụ cho phép Kubernetes định tuyến lưu lượng đến ứng dụng của bạn bất kể pod đang chạy ở đâu.
