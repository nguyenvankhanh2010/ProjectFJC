---
title: "Bước 2.4: Kiểm Tra Lambda với EC2 Instance Đã Chấm Dứt"
date: 2025-07-07
weight: 4
chapter: false
pre: " <b> 4.4 </b> "
---

# Bước 2.4: Kiểm Tra Lambda với EC2 Instance Đã Chấm Dứt

#### Tại Sao Quan Trọng

Việc chấm dứt EC2 instance tạo ra tình huống mà snapshot trở thành không sử dụng (không liên kết với volume đang hoạt động). Bước này kiểm tra khả năng của hàm Lambda trong việc phát hiện và xóa các snapshot như vậy, xác nhận chức năng tiết kiệm chi phí.

---

## Hướng Dẫn

1. **Chấm Dứt EC2 Instance**:
   - Điều hướng đến **EC2 Console** → **Instances**.
   - Chọn `TestInstance` → Nhấp **Instance State** → **Terminate Instance**.
   - Xác nhận chấm dứt và chờ trạng thái instance hiển thị **Terminated**.

![Chấm Dứt EC2](../images/ec2_termination.png?featherlight=false&width=90pc)

2. **Chạy Hàm Lambda**:
   - Điều hướng đến **Lambda Console** → Chọn hàm `StaleSnapshotCleaner`.
   - Trong phần **Code**, nhấp **Test** để chạy hàm với `TestEvent`.
   - Kiểm tra đầu ra, sẽ cho biết snapshot (`TestSnapshot`) đã bị xóa vì volume liên quan không còn tồn tại.

![Đầu Ra Xóa Lambda](../images/lambda_deletion_output.png?featherlight=false&width=90pc)

---

> **Bạn Có Biết Không?** Việc chấm dứt EC2 instance không tự động xóa volume EBS hoặc snapshot liên quan, đó là lý do tại sao cần dọn dẹp thủ công hoặc tự động để quản lý chi phí.