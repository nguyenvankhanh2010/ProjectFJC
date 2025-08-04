---
title: "5. Results achieved"
weight: 5
chapter: false
--------------

## Kết Quả Đạt Được

1. **Kiến Trúc Hệ Thống Tự Động Hóa**:
   - Hệ thống được thiết kế để tự động hóa việc quản lý snapshot EBS, sử dụng các dịch vụ AWS như EC2, Lambda, CloudWatch/EventBridge, IAM, và S3. Hình ảnh dưới đây mô tả cách các thành phần này tương tác để phát hiện và xóa snapshot không sử dụng, đảm bảo hiệu quả chi phí.

   ![Kiến Trúc Hệ Thống](./images/system_architecture.png?featherlight=false&width=90pc)

2. **Tự Động Hóa Dọn Dẹp Snapshot**:
   - Hàm Lambda `StaleSnapshotCleaner` đã được triển khai và kiểm tra thành công, tự động xóa các snapshot EBS không liên kết với volume đang hoạt động, như được hiển thị trong đầu ra kiểm tra.


3. **Giảm Chi Phí AWS**:
   - Snapshot EBS được lưu trữ trên Amazon S3 và phát sinh chi phí dựa trên kích thước. Bằng cách xóa snapshot không sử dụng, bạn có thể tiết kiệm chi phí, đặc biệt trong các tài khoản AWS với nhiều instance và snapshot.

4. **Tăng Cường Quản Lý Tài Nguyên**:
   - Quy trình tự động hóa với CloudWatch/EventBridge đảm bảo snapshot được dọn dẹp định kỳ mà không cần can thiệp thủ công, giảm nguy cơ tích lũy tài nguyên không sử dụng.


5. **Bảo Mật và Tuân Thủ**:
   - Chính sách IAM `StaleSnapshotPolicy` được cấu hình theo nguyên tắc quyền hạn tối thiểu, chỉ cấp các quyền cần thiết (`DescribeInstances`, `DescribeVolumes`, `DescribeSnapshots`, `DeleteSnapshot`), tăng cường bảo mật.

6. **Khả Năng Mở Rộng**:
   - Giải pháp có thể được mở rộng để quản lý các tài nguyên AWS khác, như Elastic IP hoặc volume EBS không gắn kết, giúp tối ưu hóa chi phí toàn diện.

## Lợi Ích Cụ Thể

- **Tiết kiệm chi phí**: Xóa snapshot không sử dụng giúp giảm chi phí lưu trữ S3, đặc biệt quan trọng trong môi trường sản xuất với hàng trăm snapshot.
- **Tự động hóa**: Lịch trình CloudWatch/EventBridge loại bỏ nhu cầu kiểm tra thủ công, tiết kiệm thời gian và giảm lỗi con người.
- **Tính linh hoạt**: Lịch trình có thể được điều chỉnh (hàng giờ, hàng ngày, hoặc hàng tuần) để cân bằng giữa chi phí Lambda và hiệu quả dọn dẹp.
- **Khả năng tái sử dụng**: Mã Lambda có thể được sửa đổi để quản lý các tài nguyên khác, như Elastic IP hoặc volume EBS.

## Đề Xuất Mở Rộng

Để tối ưu hóa chi phí AWS hơn nữa, bạn có thể:

1. **Quản Lý Elastic IP**:
   - Tạo một hàm Lambda mới để phát hiện và giải phóng các Elastic IP không gắn kết với instance. Ví dụ mã:

```python
import boto3
import json

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    addresses = ec2.describe_addresses()['Addresses']
    released_ips = []
    for address in addresses:
        if 'InstanceId' not in address:
            ec2.release_address(AllocationId=address['AllocationId'])
            released_ips.append(address['AllocationId'])
    return {
        'statusCode': 200,
        'body': json.dumps({
            'message': 'Đã giải phóng Elastic IP không sử dụng',
            'released_ips': released_ips
        })
    }