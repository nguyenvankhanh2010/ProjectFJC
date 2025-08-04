---
title: "Step 4.2: Create Lambda Function"
date: 2025-07-07
weight: 4.2
chapter: false
pre: "<b>4.2 </b>"
---

# Step 4.2: Create Lambda Function

#### Why It Matters

This step creates a Lambda function to automate the detection and deletion of stale EBS snapshots, reducing costs by removing snapshots not associated with active volumes.

---

## Instructions

1. **Navigate to Lambda Console**:
   - From the AWS Console homepage, click **Services** → **Lambda** to access the Lambda Console.

   ![Lambda Console](../images/lambda_console.png?featherlight=false&width=90pc)

2. **Create a Lambda Function**:
   - In the Lambda Console, click **Functions** in the left sidebar.
   - Click the **Create Function** button.
   - Select **Author from Scratch** and configure:
     - **Function Name**: `StaleSnapshotCleaner`
     - **Runtime**: Choose **Python 3.9** or the latest available Python version.
     - **Architecture**: Select `x86_64` (default).
     - **Permissions**: Use the default execution role (AWS will create a role with basic Lambda permissions).
   - Scroll down and click **Create Function**.

   ![Lambda Function Created](../images/lambda_function_created.png?featherlight=false&width=90pc)

3. **Add Code to the Lambda Function**:
   - In the **Code** tab of the Lambda function page, locate the `lambda_function.py` file in the code editor.
   - Clear the existing code and replace it with the following Python script to identify and delete stale snapshots:

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
            'message': 'Stale snapshots deleted',
            'deleted_snapshots': deleted_snapshots
        })
    }