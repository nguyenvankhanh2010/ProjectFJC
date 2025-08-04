---
title: "3. Preparation Required"
date: 2025-08-04
weight: 3
chapter: false
---

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