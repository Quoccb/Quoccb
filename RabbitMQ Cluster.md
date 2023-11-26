## 1. Khái niệm RabbitMQ

**AMQP - Advanced Message Queuing Protocol** là một giao thức mã nguồn mở giúp gửi message hiệu quả với độ tin cậy cao giữa các ứng dụng và message broker.

**Message Broker** là phần mềm dùng để giúp các ứng dụng khác nhau có thể giao tiếp với nhau một cách dễ dàng. 3 nhiệm vụ chính mà Message Broker đảm nhận là xác thực, chuyển đổi và định tuyến cho các tin nhắn giữa những ứng dụng với nhau. 

**RabbitMQ** là một **Message Broker** sử dụng giao thức AMQP để phục vụ cho hoạt động trao đổi message giữa các ứng dụng với nhau. RabbitMQ vận chuyển các message và quản lý những message trên hàng đợi.

![rabbitmq](https://statics.cdn.200lab.io/2023/08/exchanges-topic-fanout-direct.png)

* **Message**: Là đơn vị cơ bản để trao đổi thông tin trong RabbitMQ.
* **Producer** là một ứng dụng (hoặc phiên bản ứng dụng) gửi message tới RabbitMQ. Đây là điểm khởi đầu trong workflow của RabbitMQ.
*** Binding**: Là các ràng buộc được thiết lập để xác định mối quan hệ giữa exchange và queue. Chúng chỉ định các quy tắc định tuyến để gửi message từ exchange tới queue thích hợp. Một queue có thể liên kết với nhiều exchange và một exchange có thể có nhiều liên kết tới các queue khác nhau.
* **Exchange** đóng vai trò như một bộ định tuyến message. Khi producer gửi message tới RabbitMQ, message sẽ được gửi tới exchange. Sau đó, exchange sẽ định tuyến message đến queue thích hợp.
	* **Direct exchange**: Trong direct exchange, message được định tuyến đến queue dựa trên routing key. Nếu routing key của message khớp với routing key của queue, message sẽ được chuyển đến queue đó. Ngược lại, nếu routing key của message không khớp với bất kỳ routing key của queue nào, message sẽ bị loại bỏ.
	* **Default exchange**: Là một direct exchange mặc định, được khai báo trước mà không có tên, cho phép định tuyến message trực tiếp tới queue với tên là routing key của message. Mỗi queue đều tự động liên kết với exchange mặc định bằng một routing key giống tên của queue.
	* **Fanout exchange**: Trong fanout exchange, message được định tuyến đến tất cả các queue được liên kết với nó, bất kể chúng có routing key như nào.
	* **Topic exchange**: Trong topic exchange, message được định tuyến đến queue dựa trên topic, cho phép sử dụng regular expression pattern trong routing key như `*` để đại diện cho một ký tự và `#` đại diện cho không hoặc nhiều ký tự.
	* **Header exchange**: Trong header exchange, message được định tuyến dựa trên thuộc tính header của message thay vì routing key.

		Tham số `x-match` trong binding xác định cách so sánh các cặp key/value của header trong message với header của binding. Có 2 giá trị cho tham số `x-match` là `any` và `all`.
		*  **x-match: any** : chỉ cần một thuộc tính trong header của message phải khớp với header của binding.
		* **x-match: all** : tất cả các thuộc tính trong header của message phải khớp với header của binding.
* **Queues:** Trong RabbitMQ, **queue** là một danh sách các message được lưu trữ cho tới khi chúng được tiêu thụ bởi consumer. Message được định tuyến tới queue thông qua bindings từ exchange. RabbitMQ cho phép tạo nhiều queue với các cấu hình khác nhau. Mỗi queue có một tên, ngoài ra còn có các thuộc tính tùy chọn khác như độ bền, thời gian hết hạn của message và độ dài tối đa của message.
* **Consumers:** Consumers là một ứng dụng nhận `message` từ một hay nhiều queue. Client có thể đóng kết nối tới RabbitMQ. Khi RabbitMQ phát hiện mất kết nối, việc gửi message sẽ dừng lại.
	* **Cơ chế hoạt động**: 
		* **push**: là chế độ hoạt động mặc định, RabbitMQ sẽ push message đến consumer
		* **pull**: consumer chủ động poll message từ queue
	* **Delivery acknowledgment mode**:
		* **Automatic**: không yêu cầu xác nhận của consumer, còn gọi là "fire and forget".
		* **Manual**: yêu cầu xác nhận của consumer.
	## 2. RabbitMQ Cluster
	Môt Rabbitmq Cluster là một nhóm các erlang node làm việc cùng với nhau. Mỗi erlang node có một rabbitmq application hoạt động và cùng chia sẻ tài nguyên: user, vhost, queue, exchange…
		
	*(Trong mô hình cluster, các node sử dụng phương thức trao đổi giữa của erlang. Khi sử dụng phương thức này, hai erlang node chỉ nói chuyện được với nhau khi có cùng erlang cookie. Erlang cookie chỉ là một chuỗi ký tự. Khi startup một rabbitmq server lần đầu tiên, mặc định một erlang cookie ngẫu nhiên được sinh ra nằm trong  `/var/lib/rabbitmq/.erlang.cookie`)*

**Quorum queue**: Quorum queue (QQ) là một kiểu hàng đợi của RabbitMQ triển khai FIFO queue dựa trên thuật toán đồng thuận Raft. QQ được giới thiệu từ RabbitMQ phiên bản 3.8.0. *(Trước QQ, cách duy nhất để replica dữ liệu trong RabbitMQ là queue mirroring (QM)).	

Rabbitmq cluster khá nhạy cảm với **Network Partition** dẫn đến bị **Split-Brain**. Để khôi phục lại Cluster khi gặp sự cố split-brain (cluster bị chia thành các partition độc lập) cần chọn một partition làm chuẩn, sau đó stop tất cả các node trong partition khác rồi start lại. Các node sẽ được đồng bộ lại sau khi rejoin cluster.

Rabbitmq cung cấp 3 cách để tự động xử lý network partition tránh bị split-brain đó là: `autoheal`, `pause_minority` và `pause_if_all_down` (cấu hình mặc định là `ignore`.
*  `pause_minority`: Rabbitmq sẽ tự động dừng các node trong minority partition (Partition có số node nhỏ hơn hoặc bằng tổng số node của cluster).
* `pause-if-all-down`: Administrator sẽ list ra các node. Rabbitmq sẽ tự động dừng các node bị mất kết nối đến tất cả các node có trong list.
* `autoheal`: Khi **Partition** xảy ra, Rabbitmq sẽ tự động chọn một "winning partition" và restart tất cả các node **không thuộc** "winning partition". Partition có nhiều client kết nối nhất sẽ được chọn làm winning partition (Nếu số client bằng nhau thì sẽ chọn partition có nhiều node nhất. Nếu số node cũng bằng nhau thì sẽ chọn ngẫu nhiên)

**Khôi phục cluster trong trường hợp tất cả các node down:**
* Các node down lần lượt: Phải boot node down cuối cùng lên trước tiên, các node còn lại không cần theo thứ tự.
* Các node down đồng thời hoặc down lần lượt nhưng node down cuối cùng không boot lên được: Cần phải ép một node không phải node down cuối cùng làm khởi điểm để boot lên trước.

      # rabbitmqctl force_boot
      Forcing boot for Mnesia dir /var/lib/rabbitmq/mnesia/rabbit@rabbit1 ...
      # service rabbitmq-server start
      Starting rabbitmq-server: SUCCESS
      rabbitmq-server.
