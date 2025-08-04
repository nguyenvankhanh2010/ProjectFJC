---
title: "Step 4.5: Automate with CloudWatch/EventBridge"
date: 2025-07-07
weight: 4.5
chapter: false
pre: "<b>4.5 </b>"
---

# Step 4.5: Automate with CloudWatch/EventBridge

#### Why It Matters

Automating the Lambda function with CloudWatch/EventBridge ensures regular cleanup of stale snapshots, but frequent invocations may increase costs. This step sets up a schedule to trigger the function periodically.

---

## Instructions

1. **Navigate to CloudWatch Console**:
   - From the AWS Console homepage, click **Services** → **CloudWatch** to access the CloudWatch Console.

   ![CloudWatch Console](../images/cloudwatch_console.png?featherlight=false&width=90pc)

2. **Create a Rule**:
   - In the CloudWatch Console, click **Rules** in the left sidebar under **Events**.
   - Click **Create Rule**.
   - Select **Schedule** as the event source.
   - Configure the schedule:
     - Choose **Fixed rate** and set it to `1 hour` (e.g., `rate(1 hour)`).
   - Click **Add Target** and select **Lambda function**.
   - In the **Function** dropdown, choose `StaleSnapshotCleaner`.

   ![CloudWatch Rules](../images/cloudwatch_rules.png?featherlight=false&width=90pc)
   ![CloudWatch Schedule](../images/cloudwatch_schedule.png?featherlight=false&width=90pc)
   ![CloudWatch Target](../images/cloudwatch_target.png?featherlight=false&width=90pc)
   ![CloudWatch Schedule Config](../images/cloudwatch_schedule_config.png?featherlight=false&width=90pc)

3. **Configure Rule Details**:
   - Click **Next**.
   - In the **Targets** section, ensure the `StaleSnapshotCleaner` function is selected.
   - Click **Next** again.
   - For **Action after Schedule**, select **None**.

   ![CloudWatch Next](../images/cloudwatch_next.png?featherlight=false&width=90pc)
   ![CloudWatch Target Selection](../images/cloudwatch_target_selection.png?featherlight=false&width=90pc)
   ![CloudWatch Action None](../images/cloudwatch_action_none.png?featherlight=false&width=90pc)

4. **Finalize the Rule**:
   - Set the **Rule Name** to `HourlySnapshotCleanup`.
   - Click **Create Rule** to activate the schedule.

   ![CloudWatch Rule Name](../images/cloudwatch_rule_name.png?featherlight=false&width=90pc)
   ![CloudWatch Rule Created](../images/cloudwatch_rule_created.png?featherlight=false&width=90pc)

5. **Monitor Costs**:
   - The schedule triggers the Lambda function every hour, which may increase costs due to frequent executions.
   - Consider adjusting to a daily or weekly schedule (e.g., `rate(1 day)`) or triggering manually to optimize costs.

---

> **Did you know?** CloudWatch/EventBridge schedules can be customized to run at specific times or days, offering flexibility for cost management.