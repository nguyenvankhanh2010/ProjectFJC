title: "Step 2.4: Test Lambda with Terminated EC2 Instance"
weight: 2.4
chapter: false
--------------

# Step 2.4: Test Lambda with Terminated EC2 Instance

#### Why It Matters

Terminating the EC2 instance creates a scenario where the snapshot becomes stale (not associated with an active volume). This step tests the Lambda function’s ability to detect and delete such snapshots, confirming its cost-saving functionality.

---

## Instructions

1. **Terminate the EC2 Instance**:
   - Navigate to **EC2 Console** → **Instances**.
   - Select `TestInstance` → Click **Instance State** → **Terminate Instance**.
   - Confirm the termination and wait for the instance status to show **Terminated**.

![EC2 Termination](../images/ec2_termination.png?featherlight=false&width=90pc)

2. **Run Lambda Function**:
   - Navigate to **Lambda Console** → Select `StaleSnapshotCleaner` function.
   - In the **Code** section, click **Test** to run the function with the `TestEvent`.
   - Check the output, which should indicate that the snapshot (`TestSnapshot`) was deleted because its associated volume no longer exists.

![Lambda Deletion Output](../images/lambda_deletion_output.png?featherlight=false&width=90pc)

---

> **Did you know?** Terminating an EC2 instance does not automatically delete its associated EBS volume or snapshots, which is why manual or automated cleanup is essential for cost management.