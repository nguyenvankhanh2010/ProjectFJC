---
title: "Giới thiệu: Xây dựng hệ thống quản lý chi phí AWS hiệu quả"
weight: 1
chapter: false
---

![Quản lý chi phí AWS](https://www.prosperops.com/wp-content/uploads/2023/11/Blog-Featured-Image_8-Understanding-AWS-Cost-Management-1.jpg)

> *Hình minh hoạ quản lý chi phí AWS — Nguồn: ProsperOps*

---

## Tại sao cần Workshop này?

Trong môi trường điện toán đám mây hiện đại, kiểm soát chi phí là điều bắt buộc đối với bất kỳ nhóm hay tổ chức nào. Một nguyên nhân phổ biến dẫn đến hóa đơn AWS tăng cao là do các tài nguyên dư thừa — đặc biệt là snapshot của EC2, Elastic IP hoặc volume không còn gắn với máy chủ nhưng vẫn tồn tại, âm thầm tạo chi phí hàng tháng.

Workshop này sẽ hướng dẫn bạn một cách tiếp cận đơn giản nhưng rất thực tế để phát hiện và tự động xoá các snapshot EC2 không còn sử dụng, thông qua một **AWS Lambda Function** thông minh. Bằng cách triển khai giải pháp này, bạn có thể giảm lãng phí và nâng cao tính kỷ luật trong quản lý hạ tầng.

## Bạn sẽ học được gì?

- Hiểu cách snapshot EC2 và Elastic IP không cần thiết ảnh hưởng đến chi phí AWS của bạn.
- Sử dụng thành thạo các dịch vụ **EC2**, **Lambda**, **IAM**, **CloudWatch**.
- Tạo và cấu hình Lambda Function để tự động dò tìm và xóa snapshot dư thừa.
- Thiết lập chính sách IAM để tự động hoá an toàn.
- Sử dụng CloudWatch hoặc EventBridge để lập lịch chạy tự động.
- Kiểm thử giải pháp bằng cách tạo, xoá tài nguyên và quan sát Lambda tự động dọn dẹp.

## Dành cho ai?

Workshop này phù hợp với developer, DevOps engineer, quản trị viên đám mây hoặc bất kỳ ai muốn thực hành kiểm soát chi phí AWS bằng tự động hóa.

## Kết quả đạt được

Sau khi hoàn thành, bạn sẽ:
- Có một giải pháp dọn dẹp tự động hoạt động thực tế.
- Biết cách mở rộng logic này cho các tài nguyên AWS dư thừa khác.
- Có ý thức và kỹ năng giám sát chi phí AWS hiệu quả hơn.

Hãy bắt đầu tối ưu chi phí cho cloud của bạn ngay hôm nay!
