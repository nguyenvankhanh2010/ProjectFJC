---
title: "Phần II: Các công nghệ sử dụng"
weight: 2
chapter: false
---

# ⚙️ Các công nghệ sử dụng

Hiểu rõ các dịch vụ AWS cốt lõi sẽ giúp bạn dễ dàng theo dõi và tuỳ chỉnh workshop này. Dưới đây là chi tiết từng thành phần chính.

---

## 2.1 AWS EC2

**Amazon Elastic Compute Cloud (EC2)** cung cấp khả năng tính toán linh hoạt trên cloud.  
Trong workshop này, chúng ta sẽ:
- Tạo một instance làm workload mẫu.
- Gắn EBS volume để lưu trữ dữ liệu.
- Xoá instance sau để mô phỏng tài nguyên bị bỏ quên.

---

## 2.2 EBS (Elastic Block Store)

**Elastic Block Store (EBS)** là dịch vụ lưu trữ dạng block cho EC2.
Trong workshop:
- Mỗi EC2 instance sẽ có ít nhất 1 volume EBS mặc định.
- Bạn sẽ tạo snapshot để sao lưu volume này.
- Các snapshot dư sau khi terminate là mục tiêu cần dọn dẹp để tiết kiệm chi phí.

---

## 2.3 Snapshots

**Snapshots** là bản sao lưu tại một thời điểm của EBS volume.
- Snapshot hoạt động theo kiểu incremental (bổ sung), vẫn chiếm dung lượng.
- Nếu quên xóa, chúng vẫn tính phí lưu trữ hàng tháng.
- Lambda function của chúng ta sẽ tự tìm và xoá snapshot dư.

---

## 2.4 AWS Lambda

**AWS Lambda** là dịch vụ serverless chạy code mà không cần quản lý máy chủ.
Trong workshop:
- Lambda chạy Python script kiểm tra tất cả EC2 instance & snapshot.
- Nếu phát hiện snapshot không còn liên kết instance, nó sẽ xoá snapshot đó.
- Bạn có thể chạy thủ công hoặc đặt lịch tự động.

---

## 2.5 IAM (Identity and Access Management)

**IAM** giúp quản lý truy cập dịch vụ AWS một cách an toàn.
- Chúng ta sẽ tạo IAM Policy cấp quyền: `DescribeInstances`, `DescribeVolumes`, `DescribeSnapshots`, `DeleteSnapshot`.
- Policy này được gán cho role Lambda để Lambda có quyền đọc & xoá snapshot.

---

## 2.6 CloudWatch / EventBridge

**CloudWatch** & **EventBridge** là dịch vụ giám sát & kích hoạt sự kiện.
- Trong workshop, chúng ta sẽ dùng để lên lịch chạy Lambda tự động.
- Ví dụ: chạy mỗi giờ, mỗi ngày hoặc mỗi tuần.
- Lưu ý: nếu chạy thường xuyên, chi phí Lambda có thể tăng.

---

> **Mẹo:** Thành thạo các dịch vụ này sẽ giúp bạn tự động hoá và kiểm soát chi phí AWS hiệu quả hơn.
