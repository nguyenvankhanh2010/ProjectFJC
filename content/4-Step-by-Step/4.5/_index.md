---
title: "Step 2.5: Automate with CloudWatch/EventBridge"
date: 2025-07-07
weight: 4
chapter: false
pre: " <b> 4.5 </b> "
---


# Step 2.5: Automate with CloudWatch/EventBridge

#### Why It Matters

Automating the Lambda function with CloudWatch/EventBridge ensures regular checks for stale snapshots, reducing manual effort. However, this increases costs due to frequent Lambda invocations, so consider manual triggering for cost efficiency.

---

## Instructions

1. **Navigate to CloudWatch Console**:
   - From the AWS Console, click **Services** → **CloudWatch** to access the CloudWatch Console.

2. **Create a Rule**:
   - Go to **Rules** → Click **Create Rule**.
   - Select **Schedule** as the event source.
   - Set the schedule to run every hour (e.g., enter `rate(1 hour)`).
   - Add a **Target**:
     - Choose **Lambda function**.
     - Select `StaleSnapshotCleaner` from the dropdown.
   - Click **Next** → Choose **None** for **Action after Schedule**.
   - Provide a **Rule Name** (e.g., `HourlySnapshotCleanup`) → Click **Create Rule**.

![CloudWatch Schedule Creation](../images/cloudwatch_schedule.png?featherlight=false&width=90pc)

3. **Monitor Costs**:
   - Be aware that frequent Lambda invocations (e.g., hourly) will incur additional costs.
   - To optimize costs, consider adjusting the schedule to daily or weekly, or trigger the function manually when needed.

---

> **Did you know?** CloudWatch/EventBridge schedules can be customized to run at specific times or days, offering flexibility for balancing automation and cost.