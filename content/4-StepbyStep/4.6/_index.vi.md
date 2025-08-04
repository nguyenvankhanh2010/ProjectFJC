---
title: "Bước 2.6: Ghi Chú Bổ Sung và Mở Rộng"
date: 2025-07-07
weight: 6
chapter: false
pre: " <b> 4.6 </b> "
---
# Bước 2.6: Ghi Chú Bổ Sung và Mở Rộng

#### Tại Sao Quan Trọng

Ngoài snapshot, các tài nguyên AWS khác như Elastic IP hoặc volume EBS không gắn kết cũng có thể phát sinh chi phí không cần thiết. Bước này cung cấp hướng dẫn để mở rộng giải pháp và tối ưu hóa chi phí.

---

## Hướng Dẫn

1. **Mở Rộng để Dọn Dẹp Elastic IP**:
   - Tạo hàm Lambda mới (ví dụ: `StaleElasticIPCleaner`) sử dụng logic tương tự để kiểm tra Elastic IP không liên kết với instance đang chạy.
   - Sử dụng API `ec2.describe_addresses` và `ec2.release_address` để xác định và giải phóng Elastic IP không sử dụng.
   - Mã ví dụ:

```python
import boto3
import json

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    
    # Lấy tất cả Elastic IP
    addresses = ec2.describe_addresses()['Addresses']
    
    released_ips = []
    
    # Kiểm tra Elastic IP không liên kết
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