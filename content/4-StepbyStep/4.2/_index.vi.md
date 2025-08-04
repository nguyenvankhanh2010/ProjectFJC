---
title: "Bước 4.2: Tạo Hàm Lambda"
date: 2025-07-07
weight: 4.2
chapter: false
pre: "<b>4.2 </b>"
---

# Bước 4.2: Tạo Hàm Lambda

#### Tại Sao Quan Trọng

Bước này tạo một hàm Lambda để tự động phát hiện và xóa các snapshot EBS không sử dụng, giúp giảm chi phí bằng cách loại bỏ snapshot không liên kết với volume đang hoạt động.

---

## Hướng Dẫn

1. **Điều Hướng đến Lambda Console**:
   - Từ trang chủ AWS Console, nhấp **Services** → **Lambda** để vào Lambda Console.

   ![Lambda Console](../images/lambda_console.png?featherlight=false&width=90pc)

2. **Tạo Hàm Lambda**:
   - Trong Lambda Console, nhấp **Functions** ở thanh bên trái.
   - Nhấp nút **Create Function**.
   - Chọn **Author from Scratch** và cấu hình:
     - **Tên Hàm**: `StaleSnapshotCleaner`
     - **Runtime**: Chọn **Python 3.9** hoặc phiên bản Python mới nhất.
     - **Kiến Trúc**: Chọn `x86_64` (mặc định).
     - **Quyền**: Sử dụng vai trò thực thi mặc định (AWS sẽ tạo vai trò với quyền Lambda cơ bản).
   - Cuộn xuống và nhấp **Create Function**.

   ![Hàm Lambda Đã Tạo](../images/lambda_function_created.png?featherlight=false&width=90pc)

3. **Thêm Mã vào Hàm Lambda**:
   - Trong tab **Code** của trang hàm Lambda, tìm file `lambda_function.py` trong trình chỉnh sửa mã.
   - Xóa mã hiện có và thay bằng đoạn mã Python sau để xác định và xóa snapshot không sử dụng:

```python
import boto3
import json

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    snapshots = ec2.describe_snapshots(OwnerIds=['self'])['Snapshots']
    volumes = ec2.describe_volumes()['Volumes']
    volume_ids = [volume['VolumeId'] for volume in volumes]
    deleted_snapshots = []
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