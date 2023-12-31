## Khái niệm về Cloud computing.

**Định nghĩa của Cloud Computing theo NIST:**

> “Cloud Computing là mô hình dịch vụ cho phép người dùng truy cập tài
> nguyên điện toán dùng chung (mạng, sever, lưu trữ, ứng dụng, dịch vụ)
> thông qua kết nối mạng một cách dễ dàng, mọi lúc mọi nơi, theo yêu
> cầu. Tài nguyên điện toán đám mây này có thể được thiết lập hoặc hủy
> bỏ nhanh chóng bởi người dùng mà không cần sự can thiệp của Nhà cung
> cấp dịch vụ”.

### Mô hình 5-4-3 trong Cloud Computing.

Cloud computing bao gồm 5 đặc điểm, 4 mô hình triển khai, và 3 mô hình dịch vụ. Trong đó:

#### 5 Đặc điểm của cloud computing:
-   **On-demand self-service( Khả năng tự phục vụ)**: Người dùng có thể đơn phương cấp phát tài nguyên điện toán theo yêu cầu mà không cần sự can thiệp của nhà cung cấp.
-   **Broad Network access(Truy cập qua mạng)**: Khả năng có sẵn trên mạng và được truy cập sử dụng các tiêu chuẩn chung.
-   **Resource pooling(Cung cấp tài nguyên theo nhóm)**: Tài nguyên phần cứng hay phần mềm được gộp thành các nhóm(pool) sau đó được chia lại cho các người dùng theo yêu cầu.
-   **Rapid elastycity(Khả năng thu hồi và cấp phát tài nguyên)**: Người dùng có khả năng thu hồi, cấp phát tài nguyên nhanh chóng mà không cần đến sự tương tác đến nhà cung cấp.
-   **Measured services(Khả năng đo lường dịch vụ)**: Là khả năng chi trả theo mức độ sử dụng dịch vụ của người dùng.

#### 4 Mô hình triển khai.

-   **Public Cloud**: là dịch vụ trên nền tảng Cloud Computing cung cấp tài nguyên điện toán công cộng cho nhiều cá nhân, tổ chức sử dụng.
    -   Đối tượng sử dụng: Người dùng bên ngoài internet. Đối tượng quản lý dịch vụ: nhà cung cấp.
    -   Ưu điểm:
        -   Phục vụ được nhiều người, không bị giới hạn bởi không gian hay thời gian
        -   Tiết kiệm hệ thống máy chủ, điện năng, nhân công cho doanh nghiệp.
    -   Nhược điểm:
        -   Các doanh nghiệp bị phụ thuộc vào nhà cung cấp, không có toàn quyền quản lý.
        -   Không kiểm soát được độ an toàn, bảo mật cho dữ liệu của doanh nghiệp vì mọi tài nguyên đều do nhà cung cấp quản lý.
-   **Private Cloud**: Đám mây riêng là dịch vụ điện toán đám mây được sử dụng cho một nhóm người dùng cụ thể(như một doanh nghiệp), không công khai ra ngoài. Xu hướng tất yếu cho các doanh nghiệp muốn tối ưu hóa hạ tầng công nghệ thông tin.
    -   Đối tượng sử dụng: Nội bộ doanh nghiệp quản lý và sử dụng
    -   Ưu điểm:
        -   Chủ động doanh nghiệp xây dựng, nâng cấp, sử dụng và quản lý. Bảo mật thông tin nội bộ hơn.
    -   Nhược điểm:
        -   Khó khăn về chi phí, công nghệ để triển khai, nâng cấp và duy trì.
-   **Community Cloud**: Đám mây cộng đồng, là dịch vụ dựa trên nền tảng điện toán đám mây do các công ty hợp tác phát triển cho mục đích sử dụng cộng đồng.
    -   Đối tượng sử dụng: Một đám mây cộng đồng có thể được thiết lập bởi một số tổ chức có yêu cầu tương tự và tìm cách chia sẻ cơ sở hạ tầng để thực hiện một số lợi ích của điện toán đám mây.
        
    -   Ưu điểm: Có thể đáp ứng về sự riêng tư, an ninh hoặc tuân thủ các chính sách tốt hơn.
        
    -   Nhược điểm: Tốn kém.
        
-   **Hybrid Cloud**: Là mô hinh kết hợp giữa Private cloud và Public Cloud.
    -   Đối tượng sử dụng: Doanh nghiệp và nhà cung cấp quản lý theo sự thỏa thuận. Người sử dụng có thể sử dụng các dịch vụ của nhà cung cấp và dịch vụ riêng của doanh nghiệp.
      -   Ưu điểm: Doanh nghiệp 1 lúc có thể sử dụng được nhiều dịch vụ mà không bị giới hạn.
    -   Nhược điểm: 
        -   Khó khăn trong việc triển khai và quản lý.
        -   Tốn nhiều chi phí.

#### 3 Mô hình dịch vụ:
-   **Infrastructure as a Service (IaaS)**:
    -   Cung cấp tài nguyên hạ tầng - network, server, storage (thường là dưới hình thức của một máy ảo) như một dịch vụ.
    -   Dịch vụ IaaS phổ biến hiện nay trên thế giới như Amazon Cloud, Google Cloud, Microsoft.
-   **Platform as a Service (PaaS)**:
    -   Cung cấp môi trường để khách hàng có thể sử dụng ngôn ngữ lập trình để phát triển và triển khai các ứng dụng trên nền tảng chung với khả năng kiểm soát môi trường và ứng dụng triển khai.
    -   IBM Workload Deployer, Google App Engine, Windows Azure, Force.com từ Salesforce là những ví dụ về PaaS
-   **Software as a Service (SaaS)**:
    -   Là mô hình mà khách hàng sử dụng các ứng dụng chuyên dụng mà không cần quản lý hay kiểm soát các tài nguyên như hạ tầng, nền tảng, hay ứng dụng.
    -   Nhà cung cấp dịch vụ triển khai, quản lý gần như toàn bộ hệ thống, người dùng sẽ sử dụng những tài nguyên được cung cấp

![](https://i.imgur.com/tcOvtuy.png)