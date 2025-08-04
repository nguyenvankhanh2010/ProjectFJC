---

title: "Triển Khai Ứng Dụng Web A.I ReactJS & NestJS trên AWS"
weight: 1
chapter: false
--------------

# Giới Thiệu

#### Tổng Quan

Chào mừng bạn đến với phòng lab thực hành, nơi bạn sẽ học cách triển khai một ứng dụng web AI đầy đủ chức năng, có tính khả dụng cao và bảo mật trên AWS. Trong quá trình thực hành này, bạn sẽ:

1. **Thiết kế và xây dựng mạng lưới kiên cố**
   Tạo một Virtual Private Cloud (VPC) đa AZ với ba subnet công cộng và một subnet riêng. Bạn sẽ đính kèm một Internet Gateway, định nghĩa các bảng định tuyến tùy chỉnh, và chặn lưu lượng với security group—xây nền tảng cho cả tài nguyên công cộng và riêng tư.

2. **Đóng gói và lưu trữ dịch vụ AI của bạn**
   Khởi tạo một EC2 instance kiểu t2.medium trong subnet công cộng, cài Docker, clone repo mã AI của bạn, và khởi chạy mô hình AI bên trong container. Bạn sẽ kiểm tra kết nối bên ngoài và tích hợp endpoint AI vào backend.

3. **Triển khai backend Node.js (NestJS)**
   Cấp phát một EC2 instance kiểu t2.micro, cài Node.js và PostgreSQL, đồng bộ mã qua `rsync`, cấu hình Prisma cho migration cơ sở dữ liệu, và chạy backend ở chế độ production. Bạn sẽ học cách kết nối bảo mật đến cả dịch vụ AI container và cơ sở dữ liệu.

4. **Xuất bản frontend ReactJS lên S3**
   Build ứng dụng React của bạn, cấu hình một bucket S3 để hosting website tĩnh, áp dụng chính sách bucket cho phép public read, và tải file build lên. Frontend của bạn sẽ phục vụ giao diện người dùng, lấy dữ liệu động từ GraphQL API của NestJS.

5. **Cấp phát cơ sở dữ liệu PostgreSQL quản lý trên RDS**
   Tạo một instance RDS PostgreSQL trong VPC, bảo mật bằng security group riêng biệt, và kết nối qua pgAdmin. Bạn sẽ thực thi Prisma migrations để khởi tạo schema và seed data, minh họa cách tích hợp dịch vụ database quản lý vào stack.

6. **Dọn dẹp và kiểm soát chi phí**
   Học các best practice để dọn dẹp tài nguyên: xóa dữ liệu và bucket S3, terminate EC2 instances, và xóa instance RDS để tránh phát sinh chi phí liên tục.

#### Tại Sao Điều Này Quan Trọng

Bằng cách hoàn thành các bước này, bạn sẽ có kinh nghiệm thực tế với các dịch vụ AWS cốt lõi—VPC, EC2, S3, và RDS—và tận mắt chứng kiến cách chúng tích hợp để hỗ trợ ứng dụng web AI hiện đại. Bạn sẽ trở nên thành thạo với khái niệm infrastructure as code, điều phối container với Docker, mạng bảo mật, và quy trình làm việc với database quản lý. Phòng lab này không chỉ trang bị cho bạn những artifact có thể triển khai, mà còn truyền cho bạn best practice về bảo mật, tính khả dụng cao, và quản lý chi phí trên cloud.

---

#### Các Phần trong Lab

1. [Mạng & Bảo mật VPC (Phần I)](1-VPC-networking/)
   Xây dựng VPC với subnet công cộng và riêng tư, Internet Gateway, route table, và security group.

2. [Dịch vụ AI trên EC2 (Phần II)](2-ai-on-ec2/)
   Đóng gói container và triển khai mô hình AI trong Docker trên EC2.

3. [Backend NestJS trên EC2 (Phần III)](3-backend-NestJS-on-ec2/)
   Khởi chạy backend Node.js, đồng bộ mã với `rsync`, cấu hình Prisma, và kết nối đến PostgreSQL.

4. [Frontend ReactJS trên S3 (Phần IV)](4-frontend-ReactJS-on-s3/)
   Build và host ứng dụng React dưới dạng website tĩnh trên S3 với quyền truy cập công khai.

5. [PostgreSQL trên RDS (Phần V)](5-postgresql-on-rds/)
   Cấp phát, bảo mật, và kết nối tới cơ sở dữ liệu PostgreSQL trên Amazon RDS, sau đó chạy migration.

6. [Dọn dẹp Tài nguyên (Phần VI)](6-cleanup/)
   Dismantle dịch vụ và xóa tài nguyên để duy trì môi trường AWS hiệu quả về chi phí.

---

Sẵn sàng chưa? Chúng ta bắt đầu với Phần I và xây dựng nền tảng mạng cho ứng dụng web AI của bạn nào!
