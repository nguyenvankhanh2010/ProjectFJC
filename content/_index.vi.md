---

title: "Efficient AWS Cost Management"
weight: 1
chapter: false
--------------
# Xây dựng hệ thống quản lý chi phí AWS hiệu quả

Dự án này hướng dẫn triển khai một giải pháp tự động hóa để quản lý và dọn dẹp snapshot EBS trên AWS, nhằm tối ưu hóa chi phí lưu trữ S3. Bằng cách sử dụng AWS Lambda để phát hiện và xóa snapshot không liên kết với volume đang hoạt động, kết hợp với CloudWatch/EventBridge để kích hoạt định kỳ và IAM để đảm bảo bảo mật, dự án giúp giảm chi phí không cần thiết, tăng hiệu quả quản lý tài nguyên, và cung cấp nền tảng mở rộng cho các tác vụ tối ưu hóa khác trên AWS.

![Understanding AWS Cost Management](https://miro.medium.com/v2/resize:fit:800/1*sQWvAOinWtfTwqrY3QyUqw.png)
