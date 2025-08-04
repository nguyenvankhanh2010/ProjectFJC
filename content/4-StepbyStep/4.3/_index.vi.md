---
title: "Bước 2.3: Cấu Hình Quyền IAM cho Lambda"
date: 2025-07-07
weight: 4
chapter: false
pre: " <b> 4.3 </b> "
---


# Bước 2.3: Cấu Hình Quyền IAM cho Lambda

#### Tại Sao Quan Trọng

Hàm Lambda cần các quyền cụ thể để mô tả EC2 instance, volume, snapshot và xóa snapshot không sử dụng. Bước này đảm bảo hàm có chính sách IAM cần thiết để thực hiện các hành động này.

---

## Hướng Dẫn

1. **Điều Hướng đến IAM Console**:
   - Từ AWS Console, nhấp **Services** → **IAM** để truy cập Identity and Access Management Console.

2. **Tạo Chính Sách**:
   - Vào **Policies** → Nhấp **Create Policy**.
   - Chọn tab **JSON** và dán chính sách sau:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:DescribeVolumes",
                "ec2:DescribeSnapshots",
                "ec2:DeleteSnapshot"
            ],
            "Resource": "*"
        }
    ]
}