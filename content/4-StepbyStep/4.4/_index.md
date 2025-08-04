---
title: "Step 4.4: Test Lambda with Terminated EC2 Instance"
date: 2025-07-07
weight: 4.4
chapter: false
pre: "<b>4.4 </b>"
---

# Step 4.4: Test Lambda with Terminated EC2 Instance

#### Why It Matters

Terminating the EC2 instance makes the snapshot stale (not associated with an active volume), allowing you to test the Lambda function’s ability to detect and delete such snapshots.

---

## Instructions

1. **Terminate the EC2 Instance**:
   - Navigate to the EC2 Console ( **Services** → **EC2**).
   - In the **Instances** section, select the `TestInstance` created in Step 4.1.
   - Click **Instance State** → **Terminate Instance**.
   - Confirm the termination and wait for the instance to reach the **Terminated** state.

2. **Run the Lambda Function**:
   - Return to the Lambda Console and select the `StaleSnapshotCleaner` function.
   - In the **Code** tab, click **Test** and select the `TestEvent`.
   - Execute the test and check the output. The function should detect and delete the `TestSnapshot` because its associated volume (from the terminated instance) no longer exists.

   ![Lambda Deletion Output](/images/lambda_deletion_output.png?featherlight=false&width=90pc)

---

> **Did you know?** Terminating an EC2 instance does not automatically delete its associated EBS volumes or snapshots, which can accumulate costs if not cleaned up.