---
title: "5. Results achieved"
weight: 5
chapter: false
--------------

## Results Achieved

1. **Automated Snapshot Cleanup**:
   - The `StaleSnapshotCleaner` Lambda function was successfully implemented and tested, automatically deleting EBS snapshots not linked to active volumes, as shown in the test output.

   ![Lambda Deletion Output]/images/lambda_deletion_output.png?featherlight=false&width=90pc)

2. **Cost Reduction on AWS**:
   - EBS snapshots are stored in Amazon S3 and incur costs based on their size. By deleting unused snapshots, you can save on storage costs, especially in AWS accounts with numerous instances and snapshots.

3. **Improved Resource Management**:
   - The CloudWatch/EventBridge automation ensures snapshots are cleaned up regularly without manual intervention, reducing the risk of accumulating unused resources.

   ![CloudWatch Rule Created]/images/cloudwatch_rule_created.png?featherlight=false&width=90pc)

4. **Security and Compliance**:
   - The `StaleSnapshotPolicy` IAM policy follows the principle of least privilege, granting only necessary permissions (`DescribeInstances`, `DescribeVolumes`, `DescribeSnapshots`, `DeleteSnapshot`), enhancing security.

5. **Scalability**:
   - The solution can be extended to manage other AWS resources, such as Elastic IPs or unattached EBS volumes, for comprehensive cost optimization.

## Specific Benefits

- **Cost Savings**: Deleting unused snapshots reduces S3 storage costs, critical in production environments with hundreds of snapshots.
- **Automation**: The CloudWatch/EventBridge schedule eliminates manual checks, saving time and reducing human error.
- **Flexibility**: The schedule can be adjusted (hourly, daily, or weekly) to balance Lambda costs and cleanup efficiency.
- **Reusability**: The Lambda code can be modified to manage other resources, such as Elastic IPs or EBS volumes.

## Recommendations for Extension

To further optimize AWS costs, consider:

1. **Managing Elastic IPs**:
   - Create a new Lambda function to detect and release unattached Elastic IPs. Example code:

```python
import boto3
import json

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    addresses = ec2.describe_addresses()['Addresses']
    released_ips = []
    for address in addresses:
        if 'InstanceId' not in address:
            ec2.release_address(AllocationId=address['AllocationId'])
            released_ips.append(address['AllocationId'])
    return {
        'statusCode': 200,
        'body': json.dumps({
            'message': 'Unused Elastic IPs released',
            'released_ips': released_ips
        })
    }