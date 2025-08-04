---
title: "Bước 4.3: Cấu Hình Quyền IAM cho Lambda"
date: 2025-07-07
weight: 4.3
chapter: false
pre: "<b>4.3 </b>"
---

# Bước 4.3: Cấu Hình Quyền IAM cho Lambda

#### Tại Sao Quan Trọng

Hàm Lambda cần quyền để mô tả các instance, volume, snapshot EC2 và xóa snapshot. Bước này tạo và gắn chính sách IAM để cấp các quyền này.

---

## Hướng Dẫn

1. **Điều Hướng đến IAM Console**:
   - Từ trang chủ AWS Console, nhấp **Services** → **IAM** để vào Identity and Access Management Console.

   ![Tạo Chính Sách IAM](/images/iam_policy_creation.png?featherlight=false&width=90pc)

2. **Tạo Chính Sách**:
   - Trong IAM Console, nhấp **Policies** ở thanh bên trái, sau đó nhấp **Create Policy**.
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