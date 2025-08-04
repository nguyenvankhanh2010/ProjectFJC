title: "Quản Lý Chi Phí AWS Hiệu Quả thông qua Phát Hiện Tài Nguyên Không Sử Dụng"
weight: 3
chapter: false
--------------

# Quản Lý Chi Phí AWS Hiệu Quả thông qua Phát Hiện Tài Nguyên Không Sử Dụng

#### Tại Sao Quan Trọng

Các tài nguyên AWS không sử dụng, như snapshot và Elastic IP, có thể âm thầm làm tăng hóa đơn AWS của bạn. Dự án này tự động hóa việc phát hiện và xóa các snapshot không sử dụng bằng hàm AWS Lambda, giúp bạn tối ưu hóa chi phí bằng cách đảm bảo chỉ các tài nguyên đang hoạt động mới phát sinh chi phí. Giải pháp này có thể được mở rộng cho các tài nguyên khác như Elastic IP không gắn kết.

---

## 1. Thiết Lập EC2 Instance và Snapshot

* Đăng nhập vào **AWS Console** và điều hướng đến **EC2 Console**.
* **Tạo một EC2 Instance**:
  * Vào **Instances** → Nhấn **Launch Instance**.
  * **Tên**: `TestInstance`.
  * **AMI**: Amazon Linux 2 hoặc Ubuntu LTS.
  * **Loại Instance**: `t2.micro`.
  * **Cặp Khóa**: Chọn hoặc tạo cặp khóa RSA PEM.
  * **Mạng**: Sử dụng VPC và subnet mặc định, bật **Auto-assign Public IPv4**.
  * **Lưu trữ**: Giữ mặc định 8 GiB gp2 volume.
  * Nhấn **Launch Instance**.
* **Tạo Snapshot**:
  * Điều hướng đến **Elastic Block Store** → **Volumes**.
  * Ghi chú volume mặc định gắn với EC2 instance của bạn.
  * Vào **Snapshots** → Nhấn **Create Snapshot**.
  * Chọn **Volume ID** của volume mặc định.
  * Cung cấp **Tên** cho snapshot (ví dụ: `TestSnapshot`).
  * Nhấn **Create Snapshot**.

![Tạo Snapshot](../images/snapshot_creation.png?featherlight=false&width=90pc)

---

## 2. Tạo và Cấu Hình Hàm Lambda

* Điều hướng đến **Lambda Console**.
* **Tạo Hàm Lambda**:
  * Nhấn **Create Function** → Chọn **Author from Scratch**.
  * **Tên Hàm**: `StaleSnapshotCleaner`.
  * **Runtime**: Python 3.9 hoặc mới hơn.
  * Nhấn **Create Function**.
* **Thêm Mã**:
  * Trong phần **Code**, thay thế mã mặc định bằng mã sau:

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