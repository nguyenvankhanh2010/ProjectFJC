---
title: "Bước 4.4: Kiểm Tra Lambda với EC2 Instance Đã Chấm Dứt"
date: 2025-07-07
weight: 4.4
chapter: false
pre: "<b>4.4 </b>"
---

#### Tại Sao Quan Trọng

Việc chấm dứt EC2 instance khiến snapshot trở thành không sử dụng (không liên kết với volume đang hoạt động), cho phép kiểm tra khả năng phát hiện và xóa snapshot của hàm Lambda.

---

## Hướng Dẫn

1. **Chấm Dứt EC2 Instance**:
   - Vào EC2 Console ( **Services** → **EC2**).
   - Trong phần **Instances**, chọn `TestInstance` đã tạo ở Bước 4.1.
   - Nhấp **Instance State** → **Terminate Instance**.
   - Xác nhận chấm dứt và chờ instance đạt trạng thái **Terminated**.

2. **Chạy Hàm Lambda**:
   - Quay lại Lambda Console và chọn hàm `StaleSnapshotCleaner`.
   - Trong tab **Code**, nhấp **Test** và chọn sự kiện `TestEvent`.
   - Thực thi kiểm tra và xem đầu ra. Hàm sẽ phát hiện và xóa `TestSnapshot` vì volume liên kết (từ instance đã chấm dứt) không còn tồn tại.

   ![Đầu Ra Xóa Lambda](../images/lambda_deletion_output.png?featherlight=false&width=90pc)

---

> **Bạn Có Biết Không?** Chấm dứt EC2 instance không tự động xóa volume EBS hoặc snapshot liên quan, có thể tích lũy chi phí nếu không được dọn dẹp.