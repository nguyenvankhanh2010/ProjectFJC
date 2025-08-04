---
title: "Step 2.6: Additional Notes and Extensions"
date: 2025-07-07
weight: 6
chapter: false
pre: " <b> 4.6 </b> "
---

# Step 2.6: Additional Notes and Extensions

#### Why It Matters

Beyond snapshots, other AWS resources like Elastic IPs or unattached EBS volumes can also incur unnecessary costs. This step provides guidance on extending the solution and optimizing costs.

---

## Instructions

1. **Extend to Elastic IP Cleanup**:
   - Create a new Lambda function (e.g., `StaleElasticIPCleaner`) using similar logic to check for Elastic IPs not associated with running instances.
   - Use the `ec2.describe_addresses` and `ec2.release_address` APIs to identify and release unused Elastic IPs.
   - Example code snippet:

```python
import boto3
import json

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    
    # Get all Elastic IPs
    addresses = ec2.describe_addresses()['Addresses']
    
    released_ips = []
    
    # Check for unassociated Elastic IPs
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