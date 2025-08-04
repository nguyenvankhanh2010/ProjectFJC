---
title: "Step 4.3: Configure IAM Permissions for Lambda"
date: 2025-07-07
weight: 4.3
chapter: false
pre: "<b>4.3 </b>"
---

# Step 4.3: Configure IAM Permissions for Lambda

#### Why It Matters

The Lambda function requires permissions to describe EC2 instances, volumes, and snapshots, and to delete snapshots. This step creates and attaches an IAM policy to grant these permissions.

---

## Instructions

1. **Navigate to IAM Console**:
   - From the AWS Console homepage, click **Services** → **IAM** to access the Identity and Access Management Console.

   ![IAM Policy Creation](../images/iam_policy_creation.png?featherlight=false&width=90pc)

2. **Create a Policy**:
   - In the IAM Console, click **Policies** in the left sidebar, then click **Create Policy**.
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