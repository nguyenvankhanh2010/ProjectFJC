---
title: "3. Yêu cầu chuẩn bị"
date: 2025-08-04
weight: 3
chapter: false
---

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

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')

    # Get all EBS snapshots
    response = ec2.describe_snapshots(OwnerIds=['self'])

    # Iterate through each snapshot and delete if it's not attached to any volume or the volume is not attached to a running instance
    for snapshot in response['Snapshots']:
        snapshot_id = snapshot['SnapshotId']
        volume_id = snapshot.get('VolumeId')

        if not volume_id:
            # Delete the snapshot if it's not attached to any volume
            ec2.delete_snapshot(SnapshotId=snapshot_id)
            print(f"Deleted EBS snapshot {snapshot_id} as it was not attached to any volume.")
        else:
            # Check if the volume still exists
            try:
                volume_response = ec2.describe_volumes(VolumeIds=[volume_id])
                if not volume_response['Volumes'][0]['Attachments']:
                    ec2.delete_snapshot(SnapshotId=snapshot_id)
                    print(f"Deleted EBS snapshot {snapshot_id} as it was taken from a volume not attached to any running instance.")
            except ec2.exceptions.ClientError as e:
                if e.response['Error']['Code'] == 'InvalidVolume.NotFound':
                    # The volume associated with the snapshot is not found (it might have been deleted)
                    ec2.delete_snapshot(SnapshotId=snapshot_id)
                    print(f"Deleted EBS snapshot {snapshot_id} as its associated volume was not found.")
