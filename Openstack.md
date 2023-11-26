## Openstack là gì
OpenStack là một phần mềm mã nguồn mở, dùng để triển khai Cloud Computing, bao gồm private cloud và public cloud.

![openstack-software-diagram](https://netsa.vn/wp-content/uploads/2016/04/openstack-software-diagram.png)

Openstack là một cloud software được thiết kế để chạy trên các sản phẩm phần cứng như x86, ARM. Nó không có yêu cầu gì về đặc tính phần mềm hay phần cứng, nó tích hợp với các hệ thống kế thừa và các sản phẩm bên thứ ba.
Openstack gồm một số thành phần chính sau:

 
 - **Nova**: Là module quản lý và cung cấp máy ảo. Nó hỗ trợ nhiều hypervisors gồm KVM, QEMU, LXC, XenServer... Nova là một công cụ mạnh mẽ mà có thể điều khiển toàn bộ các công việc: networking, CPU, storage, memory, tạo, điều khiển và xóa bỏ máy ảo, security, access control. Bạn có thể điều khiển tất cả bằng lệnh hoặc từ giao diện dashboard trên web.
 - **Glance**: là OpenStack Image Service, quản lý các disk image ảo. Glance hỗ trợ các ảnh Raw, Hyper-V (VHD), VirtualBox (VDI), Qemu (qcow2) và VMWare (VMDK, OVF). Bạn có thể thực hiện: cập nhật thêm các virtual disk images, cấu hình các public và private image và điều khiển việc truy cập vào chúng, và tất nhiên là có thể tạo và xóa chúng.
 - **Keystone**: quản lý xác thực cho user và projects.
 - **Neutron**: Là thành phần quản lý network cho các máy ảo. Cung cấp chức năng network as a service. Đây là hệ thống có các tính chất pluggable, scalable và API-driven.
 - **Horizon**: cung cấp cho người quản trị cũng như người dùng giao diện đồ họa để truy cập, cung cấp và tự động tài nguyên cloud. Việc thiết kế có thể mở rộng giúp dễ dàng thêm vào các sản phẩm cũng như dịch vụ ngoài như billing, monitoring và các công cụ giám sát khác.
 - **Cinder**: Dịch vụ cung cấp block storage cho các máy ảo. Nó chịu trách nhiệm quản lý vòng đời của các block device, từ tạo, đến gán và gỡ volume cho máy ảo.
 - 
## Các thành phần của Openstack

 ### 1. Openstack Nova:
 ![What is Openstack Nova Service? - GeeksforGeeks](https://media.geeksforgeeks.org/wp-content/uploads/20200803004701/NovaArchitecture.jpg)

-   **nova-api** Tiếp nhận và phản hồi các lời gọi API từ người dùng cuối. Dịch vụ này hỗ trợ OpenStack Compute API, Amazon EC2 API và một API quản trị đặc biệt cho những người dùng thực hiện các tác vụ quản trị. Nó thực hiện một số chính sách và khởi tạo hầu hết các hoạt động điều phối, chẳng hạn như tạo máy ảo.
-  **nova-compute** Một worker daemon thực hiện tác vụ quản lý vòng đời các máy ảo như: tạo và hủy các instance thông qua các hypervisor APIs. Ví dụ:
    
    -   XenAPI đối với XenServer/XCP
    -   libvirt đối với KVM hoặc QEMU
    -   VMwareAPI đối với VMware
    
    Tiến trình xử lý của  **nova-compute**  khá phức tạp, về cơ bản thì daemon này sẽ tiếp nhận các hành động từ hàng đợi và thực hiện một chuỗi các lệnh hệ thống như vận hành máy ảo KVM và cập nhật trạng thái của máy ảo đó vào cơ sở dữ liệu.
-   **nova-scheduler** Daemon này lấy các yêu cầu tạo máy ảo từ hàng đợi và xác định xem server compute nào sẽ được chọn để vận hành máy ảo.
-   **nova-conductor** Là module trung gian tương tác giữa  **nova-compute**  và cơ sở dữ liệu. Nó hủy tất cả các truy cập trự tiếp vào cơ sở dữ liệu tạo ra bởi  **nova-compute**  nhằm mục đích bảo mật, tránh trường hợp máy ảo bị xóa mà không có chủ ý của người dùng.
-   **nova-novncproxy** Cung cấp một proxy để truy cập máy ảo đang chạy thông qua kết nối VNC. Hỗ trợ các novnc client chạy trên trình duyệt.
### 2. Keystone
Keystone là **OpenStack project cung cấp các dịch vụ Identity, Token, Catalog, Policy cho các project khác trong OpenStack**. Nó triển khai Identity API của OpenStack. Hai tính năng chính của Keystone: 
- User Management: keystone xác thực tài khoản người dùng và chỉ định xem người dùng có quyền được làm gì.
-   Service Catalog: Cung cấp một danh mục các dịch vụ sẵn sàng cùng với các API endpoints để truy cập các dịch vụ đó.

Một số khái niệm trong Keystone:
-   **Authentication**: Là quá trình xác nhận danh tính của người dùng dựa trên thông tin đăng nhập của người đó(credential)). Khi xác thực được danh tính người dùng, nó cấp cho người dùng một token xác thực. Người dùng sẽ cung cấp token đó cho mỗi yêu cầu sau đó.
-   **Credentials**: Là thông tin dùng để xác thực người dùng. Ví dụ như username, password và API key, hay là token mà được cung cấp.
-   **Domain**: Là một thực thể API v3 dịch vụ nhận dạng, tập hợp của các project cà người dùng để xác định danh giới để quản trị xác thực. Có thể là một cá nhân hay tổ chức, hoặc của nhà quản trị.
-   **Endpoint**: Là một địa chỉ truy cập mạng, thường là địa chỉ URL của một dịch vụ để từ đó truy cập vào dịch vụ.
-   **Group**: Là một thực thể API v3 dịch vụ nhận dạng, là một nhóm những người dùng nằm trong một domain. Quyền của một group được thêm vào một domain hay một project sẽ được áp dụng cho tất cả user của group đó.
-   **Openstackclient**: Là một công cụ dòng lệnh cung cấp giao diện để truy cập các dịch vụ Openstack.
-   **Project**: sử dụng để nhóm hoặc cô lập tài nguyên, hoặc định danh các đối tượng. Tùy thuộc vào nhà quản lý
-   **Region**: Là một thực thể API v3 dịch vụ nhận dạng, đại diện cho một bộ phận chung trong triển khai Openstack. Có thể tạo và liên kết chúng với các region phụ để tạo cấu trúc dạng cây. Region không có ý nghĩa về mặt địa lý nhưng tên của region thường được đặt theo tên khu vực địa lý(vd: asia-east)
-   **Roles**: Là tập hợp các quyền hạn và đặc quyền của người dùng để thực hiện các hành động cụ thể. Token được gửi đến người dùng sau khi xác thực sẽ bao gồm cả một tập hợp các quyền. Khi người dùng yêu cầu một dịch vụ, dịch vụ này sẽ kiểm tra quyền hạn của người dùng và cung cấp dịch vụ trong quyền hạn đó.
-   **Service**: Một dịch vụ Openstack, như Compute (nova), Object Storage (swift), hay Image service (glance), là một hay nhiều endpoint mà qua đó người dùng có thể truy cập tài nguyên và thực hiện các hành động.
-   **Token**: Một chuỗi gồm chữ và chữ số cho phép truy cập vào các tài nguyên và API Openstack. Token có thể bị thu hồi bất kỳ lúc nào và có giá trị trong thời gian hữu hạn.
    
-   **User**: Đại diện cho một người dùng, hệ thống hay dịch vụ mà sử dụng dịch vụ Openstack

### 3. Openstack Neutron

Neutron - là code name cho Openstack networking servive, sử dụng để cung cấp “kết nối mạng như là một dịch vụ”, được quản lý bởi các Openstack service khác. Nó có một API để người dùng hoặc các dịch vụ khác có thể tạo, quản lý các tài nguyên mạng như network, subnet, firewall, router,…

Openstack Networking (neutron) cho phép tạo và gán các interface được sử dụng bởi các dịch vụ khác đến mạng. Với kiến trúc plugable, các plug-in có thể được sử dụng để triển khai các thiết bị, phần mềm khác nhau. Điều này khiến Neutron có tính linh hoạt trong kiến trúc và triển khai.

Các thành phần của Neutron:

-   **neutron-server**
    -   Chấp nhận và chuyển hướng các API request đến các plugin thích hợp để xử lý.
-   **Openstack Networking plug-in và agent**
    -   Có chức năng cắm và gỡ port, tạo mạng hoặc subnet, và cung cấp các địa chỉ IP. Các plug-in hay agent sẽ khác phụ thuộc vào nhà cung cấp và các công nghệ được sử dụng trong một môi trường điện toán đám mây cụ thể. Một số agent hoặc plug-in mà neutron cung cấp ví dụ như switch ảo hoặc cứng của Cisco, NEC OpenFlow products, Open vSwitch, Linux bridging, và VMware NSX product.
    -   Các agent phổ biến là  _L3 agent_(layer 3) và  _DHCP agent_  và plugin agent.
  
   Openstack networking chủ yếu làm việc với Openstack compute để cung cấp kết nối mạng cho máy ảo.

### 4. Openstack Block Storage - Cinder

Cinder là code name của dịch vụ Openstack Block Storage, cung cấp thiết bị block storage theo dạng volume cho các máy ảo, cho ironic bare metal host, các container,… Cách mà lưu trữ được phân phối và sử dụng phụ thuộc vào một hoặc nhiều Block Storage driver(ví dụ như LVM, NFS, SAN,…)
#### Các chức năng chính
-   Cung cấp tài nguyên dạng persistent block storage(volume) cho các máy ảo, do đó có thể gỡ volume từ máy ảo này và gán vào một máy ảo khác mà không mất dữ liệu.
-   Các volume có thể tồn tại độc lập với các máy ảo, tức là khi máy ảo bị xóa, volume vẫn có thể tồn tại.
-   Phân chia tài nguyên lưu trữ thành các khối gọi là Cinder volume.
-   Cung cấp các API như là tạo, xóa, backup, restore, tạo snapshot, clone volume và nhiều hơn nữa. Những API này thực hiện bởi các backend lưu trữ mà được cinder hỗ trợ.
-   Các kiến trúc plugin drive cho phép nhiều lựa chọn làm backend storage hoặc multiple backend.

Dịch vụ Openstack Block Storage có các thành phần sau:

![Laying Cinder Block (Volumes) In OpenStack, Part 1: The Basics – Cloud  Architect Musings](https://varchitectthoughts.files.wordpress.com/2013/11/screen-shot-2013-11-18-at-11-00-24-am.png)

-   **cinder-api**: Nhận các API request và gửi chúng về  **cinder-volume**  để thực hiện.
-   **cinder-volume**: Tương tác trực tiếp với dịch vụ Openstack Block Storage, và các tiến trình như  **cinder-scheduler**. Nó cũng tương tác với các tiến trình này qua message queue. cinder-volume sử lý các yêu cầu đọc và ghi được gửi đến dịch vụ Block Storage. Nó có thể tương tác với nhiều loại lưu trữ thông qua kiến trúc driver.
- **cinder-backup daemon**: dịch vụ này cung cấp sao lưu bất kì loại volume nào đến một nhà cung cấp lưu trữ sao lưu. Nó cũng có thể tương tác với nhiều nhà cung cấp lưu trữ khác nhau.

### 4. Openstack Placement.

-   Placement API service được giới thiệu trong phiên bản OpenStack Newton trong nova repository và được tách ra placement repository trong phiên bản OpenStack Stein. Đây là một REST API stack và mô hình dữ liệu để theo dõi và thống kê mức độ sử dụng của mỗi resource provider, cùng với các lớp tài nguyên khác nhau.
    
-   Ví dụ, một resource provider có thể là một compute node, một share storage pool, hoặc một IP allocation pool. Placement service theo dõi inventory và usage của mỗi provider. Ví dụ, một instance được tạo trên một compute node có thể tiêu tốn RAM và CPU từ một compute node resource provider, disk từ external share storage pool resource provider và địa chỉ IP từ external IP pool resource provider.
    
-   Các loại tài nguyên tiêu thụ được theo dõi như classes. Placement service cung cấp một bộ các resource class tiêu chuẩn (Ví dụ như DISK_GB, MEMORY_MB và VCPU) và cung cấp khả năng tự định nghĩa các resource class nếu cần.
    
-   Placement hoạt động như một dịch vụ web trên một mô hình dữ liệu. Việc cài đặt bao gồm tạo cơ sở dữ liệu và cài đặt, cấu hình dịch vụ web.
    
-   Lưu ý: Placement được yêu cầu bởi một vài OpenStack service, đặc biệt là Nova, vậy nên Placement nên được cài đặt trước một số service nhưng phải cài sau Keystone.

### 6. Openstack Glance

-   Là Image services bao gồm việc tìm kiếm, đăng ký, thu thập các images của các máy ảo. Glance cung cấp RESTful API cho phép truy vấn metadata của image máy ảo cũng như thu thập image thực sự
-   Images của máy ảo thông qua Glance có thể lưu trữ ở nhiều vị trí khác nhau từ hệ thống file thông thường cho tới hệ thống object-storage như OpenStack Swift.
-   Trong Glance, các images được lưu trữ giống như các template. Các Template này sử dụng để vận hành máy ảo mới. Glance là giải pháp để quản lý các ảnh đĩa trên cloud. Nó cũng có thể lấy bản snapshots từ các máy ảo đang chạy để thực hiện dự phòng cho các VM và trạng thái các máy ảo đó.

Glance bao gồm các thành phần sau:
![OpenStack - Nova and Glance](https://www.guruadvisor.net/images/numero0/openstack_glance_image_store.png)

-   **glance-api:** tiếp nhận lời gọi API để tìm kiếm, thu thập và lưu trữ image
-   **glance-registry:** thực hiện tác vụ lưu trữ, xử lý và thu thập metadata của images
-   **database:** cơ sở dữ liệu lưu trữ metadata của image
-   **storage repository:** được tích hợp với nhiều thành phần khác trong OpenStack như hệ thống file thông thường, Amazon và HTTP phục vụ cho chức năng lưu trữ images

