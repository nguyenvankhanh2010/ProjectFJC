---

title: "5. Kết quả đạt được"
weight: 5
chapter: false
--------------

## Results Achieved

1. **System Architecture for Automation**:
   - The system is designed to automate EBS snapshot management using AWS services like EC2, Lambda, CloudWatch/EventBridge, IAM, and S3. The diagram below illustrates how these components interact to detect and delete unused snapshots, ensuring cost efficiency.

   ![Kiến Trúc Hệ Thống](./images/system_architecture.png?featherlight=false&width=90pc)

2. **Automated Snapshot Cleanup**:
   - The `StaleSnapshotCleaner` Lambda function was successfully implemented and tested, automatically deleting EBS snapshots not linked to active volumes, as shown in the test output.

3. **Cost Reduction on AWS**:
   - EBS snapshots are stored in Amazon S3 and incur costs based on their size. By deleting unused snapshots, you can save on storage costs, especially in AWS accounts with numerous instances and snapshots.

4. **Improved Resource Management**:
   - The CloudWatch/EventBridge automation ensures snapshots are cleaned up regularly without manual intervention, reducing the risk of accumulating unused resources.

5. **Security and Compliance**:
   - The `StaleSnapshotPolicy` IAM policy follows the principle of least privilege, granting only necessary permissions (`DescribeInstances`, `DescribeVolumes`, `DescribeSnapshots`, `DeleteSnapshot`), enhancing security.

6. **Scalability**:
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