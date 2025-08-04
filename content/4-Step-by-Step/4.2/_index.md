title: "Step 2.2: Create Lambda Function"
weight: 2.2
chapter: false
--------------

# Step 2.2: Create Lambda Function

#### Why It Matters

The Lambda function automates the detection and deletion of stale snapshots, ensuring that snapshots not associated with active volumes are removed to save costs.

---

## Instructions

1. **Navigate to Lambda Console**:
   - From the AWS Console, click **Services** → **Lambda** to access the Lambda Console.

2. **Create a Lambda Function**:
   - Click **Create Function**.
   - Select **Author from Scratch**.
   - Configure the function:
     - **Function Name**: `StaleSnapshotCleaner`.
     - **Runtime**: Select **Python 3.9** or the latest available Python version.
   - Scroll down and click **Create Function**.

![Lambda Creation](../images/lambda_creation.png?featherlight=false&width=90pc)

3. **Add Code**:
   - In the **Code** section, locate the default `lambda_function.py`.
   - Clear the existing code and replace it with the following:

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