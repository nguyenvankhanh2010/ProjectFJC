---
title: "Bước 2.2: Tạo Hàm Lambda"
date: 2025-07-07
weight: 4
chapter: false
pre: " <b> 4.2 </b> "
---

# Bước 2.2: Tạo Hàm Lambda

#### Tại Sao Quan Trọng

Hàm Lambda tự động hóa việc phát hiện và xóa các snapshot không sử dụng, đảm bảo rằng các snapshot không liên kết với volume đang hoạt động được xóa để tiết kiệm chi phí.

---

## Hướng Dẫn

1. **Điều Hướng đến Lambda Console**:
   - Từ AWS Console, nhấp **Services** → **Lambda** để truy cập Lambda Console.

2. **Tạo Hàm Lambda**:
   - Nhấp **Create Function**.
   - Chọn **Author from Scratch**.
   - Cấu hình hàm:
     - **Tên Hàm**: `StaleSnapshotCleaner`.
     - **Runtime**: Chọn **Python 3.9** hoặc phiên bản Python mới nhất có sẵn.
   - Cuộn xuống và nhấp **Create Function**.

![Tạo Lambda](../images/lambda_creation.png?featherlight=false&width=90pc)

3. **Thêm Mã**:
   - Trong phần **Code**, tìm file `lambda_function.py` mặc định.
   - Xóa mã hiện có và thay thế bằng mã sau:

```python
import boto3
import json

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    
    # Lấy tất cả snapshot thuộc sở hữu của tài khoản
    snapshots = ec2.describe_snapshots(OwnerIds=['self'])['Snapshots']
    
    # Lấy tất cả volume đang hoạt động
    volumes = ec2.describe_volumes()['Volumes']
    volume_ids = [volume['VolumeId'] for volume in volumes]
    
    deleted_snapshots = []
    
    # Kiểm tra snapshot không liên kết với volume đang hoạt động
    for snapshot in snapshots:
        if snapshot['VolumeId'] not in volume_ids:
            ec2.delete_snapshot(SnapshotId=snapshot['SnapshotId'])
            deleted_snapshots.append(snapshot['SnapshotId'])
    
    return {
        'statusCode': 200,
        'body': json.dumps({
            'message': 'Đã xóa snapshot không sử dụng',
            'deleted_snapshots': deleted_snapshots
        })
    }