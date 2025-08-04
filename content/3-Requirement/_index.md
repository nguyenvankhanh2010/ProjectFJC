---
title: "Phần III: Các công nghệ sử dụng"
date: 2025-08-04
weight: 3
chapter: false
---

# Efficient AWS Cost Management through Stale Resource Detection

#### Why It Matters

Unused AWS resources, such as snapshots and Elastic IPs, can silently increase your AWS bill. This project automates the detection and deletion of stale snapshots using an AWS Lambda function, helping you optimize costs by ensuring only active resources incur charges. The solution can be extended to other resources like unattached Elastic IPs.

---

## 1. Set Up EC2 Instance and Snapshot

* Log into **AWS Console** and navigate to **EC2 Console**.
* **Launch an EC2 Instance**:
  * Go to **Instances** → Click **Launch Instance**.
  * **Name**: `TestInstance`.
  * **AMI**: Amazon Linux 2 or Ubuntu LTS.
  * **Instance Type**: `t2.micro`.
  * **Key Pair**: Select or create an RSA PEM key pair.
  * **Network**: Use default VPC and subnet, enable **Auto-assign Public IPv4**.
  * **Storage**: Keep default 8 GiB gp2 volume.
  * Click **Launch Instance**.
* **Create a Snapshot**:
  * Navigate to **Elastic Block Store** → **Volumes**.
  * Note the default volume attached to your EC2 instance.
  * Go to **Snapshots** → Click **Create Snapshot**.
  * Select the **Volume ID** of the default volume.
  * Provide a **Name** for the snapshot (e.g., `TestSnapshot`).
  * Click **Create Snapshot**.

![Snapshot Creation](../images/snapshot_creation.png?featherlight=false&width=90pc)

---

## 2. Create and Configure Lambda Function

* Navigate to **Lambda Console**.
* **Create a Lambda Function**:
  * Click **Create Function** → Select **Author from Scratch**.
  * **Function Name**: `StaleSnapshotCleaner`.
  * **Runtime**: Python 3.9 or later.
  * Click **Create Function**.
* **Add Code**:
  * In the **Code** section, replace the default code with the following:

```python
import boto3
import json

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    
    # Get all snapshots owned by the account
    snapshots = ec2.describe_snapshots(OwnerIds=['self'])['Snapshots']
    
    # Get all active volumes
    volumes = ec2.describe_volumes()['Volumes']
    volume_ids = [volume['VolumeId'] for volume in volumes]
    
    deleted_snapshots = []
    
    # Check for snapshots not linked to active volumes
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