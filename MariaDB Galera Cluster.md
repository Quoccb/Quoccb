## 1. Khái niệm Mariadb
**MariaDB là hệ quản trị cơ sở dữ liệu miễn phí được phát triển từ hệ quản trị cơ sở dữ liệu mã nguồn mở MySQL**. MariaDB được phát triển nhằm thay thế cho MySQL, vì thế nó tương thích và có hiệu suất cao hơn so với MySQL. 

## 2. MariaDB Galera Cluster

**MariaDB Galera Cluster** là hệ thống cluser cho phép nhiều primary node CSDL MariaDB đồng thời, tức là có thể thực hiện lệnh `write` trên node bất kỳ và dữ liệu được đồng bộ cho các node khác trong cluster. 


![](https://thomasknierim.com/images/galera-cluster.png)

#### Một số tính năng của Mariadb galera cluster:
-   Đồng bộ dữ liệu hầu như real-time
-   Mô hình active-active nhiều primary
-   Đọc và ghi trên bất kỳ cluster node nào
-   Tự động quản lý quyền/vai trò các node trong cluster, node lỗi tự drop khỏi cluster
-   Tự động quá trình join node mới
-   Client kết nối trực tiếp như một MariaDB thông thường

#### Các port cần mở trong cluster:
-   **Standard MariaDB Port**  (default: 3306) Dành cho các kết nối MySQL client.
-   **Galera Replication Port**  (default: 4567) Dành cho traffic truy cập replication Galera, sử dụng cả truyền tải UDP và TCP trên cổng này.
-   **IST Port**  (default: 4568) Dành cho chuyển trạng thái tăng dần.
-   **SST Port**  (default: 4444) Dành cho tất cả các phương thức  State Snapshot Transfer

**Cluster Failure**: Khi cluster xảy ra lỗi (down node, network partion, ..) MariaDB Cluster sẽ thực hiện cơ chế "quorum calculation" để quyết định partition nào là Primary (Partition có số node lớn hơn 1/2 tổng số node của cluster). Các node không thuộc Primary Partition sẽ chuyển về trạng thái non-Primary (có thể read, không thể write). Sau khi hệ thống trở lại bình thường, cluster sẽ tự động đồng bộ lại data.

MariaDB Galera cluster hỗ trợ Multi-Master bằng cách phát hiện và loại các process gây conflict. Tuy nhiên việc này cũng đồng nghĩa với việc xảy ra hiện tượng dữ liệu bị loại bỏ sẽ không được ghi đầy đủ. Để hạn chế điều đó, Galera  cho phép kích hoạt MySQL tự động commit lại transaction cũ đó với cấu hình `wsrep_retry_autocommit = n`.

![Retry Autocommit](https://drive.google.com/uc?id=0B05rqFCwNCjkSU1tc0lsMUQtYzQ&export=download)

