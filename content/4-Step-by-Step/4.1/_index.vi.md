---
title: "Bước 2.1: Khởi Tạo EC2 Instance và Tạo Snapshot"
date: 2025-07-07
weight: 4
chapter: false
pre: " <b> 4.1 </b> "
---

# Bước 2.1: Khởi Tạo EC2 Instance và Tạo Snapshot

#### Tại Sao Quan Trọng

Bước này thiết lập một EC2 instance và tạo snapshot để mô phỏng tình huống phổ biến mà các nhà phát triển tạo snapshot để sao lưu nhưng có thể quên xóa chúng sau khi chấm dứt instance, dẫn đến chi phí không cần thiết.

---

## Hướng Dẫn

1. **Đăng Nhập vào AWS Console**:
   - Mở trình duyệt và đăng nhập vào [AWS Management Console](https://aws.amazon.com/console/).
   - Đảm bảo bạn có thông tin đăng nhập và quyền truy cập vào EC2.

2. **Điều Hướng đến EC2 Console**:
   - Từ trang chủ AWS Console, nhấp vào **Services** → **EC2** để truy cập EC2 Console.

3. **Khởi Tạo EC2 Instance**:
   - Trong phần **Instances**, nhấp vào **Instances** ở thanh bên trái.
   - Nhấp **Launch Instance**.
   - Cấu hình instance:
     - **Tên**: `TestInstance`.
     - **AMI**: Chọn **Amazon Linux 2** hoặc **Ubuntu Server LTS**.
     - **Loại Instance**: Chọn `t2.micro` (thuộc bậc miễn phí).
     - **Cặp Khóa**: Chọn cặp khóa RSA PEM hiện có hoặc tạo mới.
     - **Cài Đặt Mạng**: Sử dụng VPC và subnet mặc định, bật **Auto-assign Public IPv4**.
     - **Lưu Trữ**: Giữ volume mặc định 8 GiB gp2.
   - Nhấp **Launch Instance**.
   - Chờ instance hiển thị **2/2 checks passed** trong trạng thái.

![Khởi Tạo EC2 Instance](../images/ec2_launch.png?featherlight=false&width=90pc)

4. **Tạo Snapshot**:
   - Điều hướng đến **Elastic Block Store** → **Volumes** trong EC2 Console.
   - Xác định volume mặc định được gắn với `TestInstance` (tạo tự động khi khởi tạo instance).
   - Vào **Snapshots** → Nhấp **Create Snapshot**.
   - Trong phần **Volume ID**, chọn ID của volume mặc định.
   - Cung cấp **Tên** cho snapshot (ví dụ: `TestSnapshot`).
   - Cuộn xuống và nhấp **Create Snapshot**.

![Tạo Snapshot](../images/snapshot_creation.png?featherlight=false&width=90pc)

---

> **Bạn Có Biết Không?** Snapshot được lưu trữ trong Amazon S3 và phát sinh chi phí dựa trên kích thước của chúng. Việc dọn dẹp snapshot không sử dụng thường xuyên có thể giảm đáng kể hóa đơn AWS của bạn.