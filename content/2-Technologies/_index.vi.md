---
title: "2. Các công nghệ sử dụng"
date: 2025-08-04
weight: 2
chapter: false
---

Trong phần này, bạn sẽ tìm hiểu các dịch vụ AWS chính được dùng trong giải pháp phát hiện tài nguyên thừa. Mỗi dịch vụ đều có vai trò riêng giúp quản lý chi phí lưu trữ hiệu quả.

---

## <b> 2.1 </b> AWS EC2

**Amazon Elastic Compute Cloud (EC2)** là dịch vụ máy chủ ảo linh hoạt.  
Trong workshop này, EC2 đóng vai trò workload để tạo ra volume và snapshot.  
Bạn sẽ khởi tạo instance, gắn volume, rồi xoá để minh hoạ snapshot thừa gây lãng phí.

<img src="https://raw.githubusercontent.com/phamr39/ezidev-imagestorage/master/aws-saa-c03/aws-5-gioi-thieu-ve-aws-ec2/Amazon-EC2.jpg" alt="Amazon EC2" style="width:70%;max-width:600px;display:block;margin:auto;" />

*Amazon EC2 – Máy chủ ảo trên Cloud*

---

## <b> 2.2 </b> EBS (Elastic Block Store)

**Amazon EBS** cung cấp lưu trữ dạng block cho EC2.  
Khi bạn khởi tạo EC2, một EBS volume mặc định sẽ được gắn kèm.  
Bạn sẽ tạo snapshot của volume này để mô phỏng sao lưu — và thấy chi phí phát sinh nếu quên xoá.

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQfYFsIM-aH7AtvdQrrthx4tfDrJzz1Cj6QvQ&s" alt="Amazon EBS" style="width:70%;max-width:600px;display:block;margin:auto;" />

*Amazon EBS – Lưu trữ block cho EC2*

---

## <b> 2.3 </b> Snapshots

**Snapshots** là bản sao lưu tại một thời điểm của volume EBS.  
Chúng hoạt động dạng incremental nhưng vẫn chiếm dung lượng và tốn phí nếu không quản lý tốt.  
Lambda sẽ tự động tìm và xoá snapshot không còn liên kết EC2 để tối ưu chi phí.

<img src="https://miro.medium.com/v2/resize:fit:1400/1*GVeaZPArzgwRUtpvLbFELA.jpeg" alt="EBS Snapshots" style="width:70%;max-width:600px;display:block;margin:auto;" />

*EBS Snapshots – Sao lưu & Khôi phục*

---

## <b> 2.4 </b> AWS Lambda

**AWS Lambda** cho phép chạy code serverless.  
Trong workshop, Lambda chạy script Python để quét snapshot & EC2, rồi xoá snapshot không liên kết instance nào.  
Hoàn toàn không cần máy chủ — tự động hoá 100%.

<img src="https://assets.dio.me/6UJHZEQOJZcmQJ-VaiGgwlpgb_91VAJVJKBAVKe_ens/f:webp/q:80/L2FydGljbGVzL2NvdmVyL2JlYjk1NjE1LWRiYzctNGE3Ni04NmFiLTJjODM4ZDNkNzY5Mi5qcGc" alt="AWS Lambda" style="width:70%;max-width:600px;display:block;margin:auto;" />

*AWS Lambda – Tự động hoá Serverless*

---

## <b> 2.5 </b> IAM (Identity and Access Management)

**IAM** quản lý quyền truy cập dịch vụ AWS.  
Bạn sẽ tạo policy cấp quyền:
- `DescribeInstances`
- `DescribeVolumes`
- `DescribeSnapshots`
- `DeleteSnapshot`

Sau đó gán policy cho role Lambda để đảm bảo quy trình tự động chạy đúng quyền và an toàn.

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSXmo0KadhhREpXe6xuxLi36HB0YLhXWNciVhPKtyyxOmNqs-GdDgjUTzuc9XOT7M7ePe0&usqp=CAU" alt="AWS IAM" style="width:70%;max-width:600px;display:block;margin:auto;" />

*AWS IAM – Quản lý truy cập an toàn*

---

## <b> 2.6 </b> CloudWatch / EventBridge

**CloudWatch** và **EventBridge** giúp giám sát và tự động hoá.  
Bạn có thể lên lịch chạy Lambda tự động hàng giờ, hàng ngày hoặc theo sự kiện để môi trường luôn gọn gàng, không còn snapshot dư.

<img src="https://razorops.com/images/blog/amazon-cloudwatch.webp" alt="Amazon CloudWatch" style="width:70%;max-width:600px;display:block;margin:auto;" />

*Amazon CloudWatch – Giám sát & Lên lịch*
