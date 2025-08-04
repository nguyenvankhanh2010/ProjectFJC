---
title: "Step 2.3: Configure IAM Permissions for Lambda"
date: 2025-07-07
weight: 4
chapter: false
pre: " <b> 4.3 </b> "
---

# Step 2.3: Configure IAM Permissions for Lambda

#### Why It Matters

The Lambda function requires specific permissions to describe EC2 instances, volumes, and snapshots, and to delete stale snapshots. This step ensures the function has the necessary IAM policy to perform these actions.

---

## Instructions

1. **Navigate to IAM Console**:
   - From the AWS Console, click **Services** → **IAM** to access the Identity and Access Management Console.

2. **Create a Policy**:
   - Go to **Policies** → Click **Create Policy**.
   - Select the **JSON** tab and paste the following policy:

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