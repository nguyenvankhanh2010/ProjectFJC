---
title: "Bước 4.1: Khởi Tạo EC2 Instance và Tạo Snapshot"
date: 2025-07-07
weight: 4.1
chapter: false
pre: "<b>4.1 </b>"
---

# Bước 4.1: Khởi Tạo EC2 Instance và Tạo Snapshot

#### Tại Sao Quan Trọng

Bước này thiết lập một EC2 instance và tạo snapshot để mô phỏng tình huống phổ biến khi snapshot được tạo để sao lưu nhưng có thể bị quên sau khi chấm dứt instance, dẫn đến chi phí không cần thiết.

---

## Hướng Dẫn

1. **Đăng Nhập vào AWS Console**:
   - Mở trình duyệt và truy cập [AWS Management Console](https://aws.amazon.com/console/).
   - Đăng nhập bằng thông tin xác thực có quyền truy cập dịch vụ EC2.

2. **Điều Hướng đến EC2 Console**:
   - Từ trang chủ AWS Console, nhấp **Services** ở menu trên cùng, sau đó chọn **EC2** để vào EC2 Console.

3. **Khởi Tạo EC2 Instance**:
   - Trong phần **Instances** của EC2 Console, nhấp **Instances** ở thanh bên trái.
   - Nhấp nút **Launch Instance**.
   - Cấu hình instance với các thiết lập sau:
     - **Tên**: `TestInstance`
     - **AMI**: Chọn **Amazon Linux 2** hoặc **Ubuntu Server LTS** (đảm bảo thuộc free tier).
     - **Loại Instance**: Chọn `t2.micro` (thuộc free tier).
     - **Cặp Khóa**: Chọn cặp khóa RSA PEM hiện có hoặc tạo mới (ví dụ: `my-key-pair`).
     - **Cài Đặt Mạng**: Sử dụng VPC và subnet mặc định, bật **Auto-assign Public IPv4 address**.
     - **Lưu Trữ**: Giữ volume mặc định 8 GiB `gp2` (SSD đa năng).
   - Xem lại cài đặt, sau đó nhấp **Launch Instance**.
   - Chờ instance đạt trạng thái **Running** và hiển thị **2/2 checks passed** trong cột trạng thái.

   ![Khởi Tạo EC2](/images/ec2_launch.png?featherlight=false&width=90pc)

4. **Điều Hướng đến Volumes**:
   - Trong EC2 Console, vào **Elastic Block Store** → **Volumes** ở thanh bên trái.
   - Xác định volume mặc định được gắn với `TestInstance` (tạo tự động khi khởi tạo instance). Volume này sẽ được liệt kê với ID instance và có dung lượng 8 GiB.

   ![Chọn Volume](/images/volume_selection.png?featherlight=false&width=90pc)

5. **Tạo Snapshot**:
   - Vào **Elastic Block Store** → **Snapshots** ở thanh bên trái.
   - Nhấp nút **Create Snapshot**.
   - Trong trang **Create Snapshot**:
     - **Volume ID**: Chọn ID của volume mặc định gắn với `TestInstance`.
     - **Tên**: Nhập tên cho snapshot, ví dụ: `TestSnapshot`.
     - **Mô Tả** (tùy chọn): Thêm mô tả như "Snapshot cho volume TestInstance".
   - Nhấp **Create Snapshot** để hoàn tất.

   ![Tạo Snapshot](/images/snapshot_creation.png?featherlight=false&width=90pc)
   ![Xác Nhận Snapshot](/images/snapshot_confirmation.png?featherlight=false&width=90pc)

---

> **Bạn Có Biết Không?** Snapshot được lưu trong Amazon S3 và phát sinh chi phí dựa trên kích thước. Dọn dẹp snapshot không sử dụng thường xuyên có thể giảm đáng kể hóa đơn AWS.