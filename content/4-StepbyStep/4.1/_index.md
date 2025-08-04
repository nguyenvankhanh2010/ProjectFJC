---
title: "Step 4.1: Launch EC2 Instance and Create Snapshot"
date: 2025-07-07
weight: 4.1
chapter: false
pre: "<b>4.1 </b>"
---

# Step 4.1: Launch EC2 Instance and Create Snapshot

#### Why It Matters

This step sets up an EC2 instance and creates a snapshot to simulate a common scenario where snapshots are created for backups but may be forgotten after instance termination, leading to unnecessary costs.

---

## Instructions

1. **Log into AWS Console**:
   - Open your browser and navigate to the [AWS Management Console](https://aws.amazon.com/console/).
   - Log in with credentials that have permissions to access EC2 services.

2. **Navigate to EC2 Console**:
   - From the AWS Console homepage, click **Services** in the top menu, then select **EC2** to access the EC2 Console.

3. **Launch an EC2 Instance**:
   - In the **Instances** section of the EC2 Console, click **Instances** in the left sidebar.
   - Click the **Launch Instance** button.
   - Configure the instance with the following settings:
     - **Name**: `TestInstance`
     - **AMI**: Select **Amazon Linux 2** or **Ubuntu Server LTS** (ensure it’s free tier eligible).
     - **Instance Type**: Choose `t2.micro` (free tier eligible).
     - **Key Pair**: Select an existing RSA PEM key pair or create a new one (e.g., `my-key-pair`).
     - **Network Settings**: Use the default VPC and subnet, enable **Auto-assign Public IPv4 address**.
     - **Storage**: Keep the default 8 GiB `gp2` volume (general-purpose SSD).
   - Review the settings, then click **Launch Instance**.
   - Wait for the instance to reach the **Running** state and show **2/2 checks passed** in the status column.

   ![EC2 Instance Launch](../images/ec2_launch.png?featherlight=false&width=90pc)

4. **Navigate to Volumes**:
   - In the EC2 Console, go to **Elastic Block Store** → **Volumes** in the left sidebar.
   - Identify the default volume attached to `TestInstance` (created automatically during instance launch). It will be listed with the instance ID and have a size of 8 GiB.

   ![Volume Selection](../images/volume_selection.png?featherlight=false&width=90pc)

5. **Create a Snapshot**:
   - Navigate to **Elastic Block Store** → **Snapshots** in the left sidebar.
   - Click the **Create Snapshot** button.
   - In the **Create Snapshot** page:
     - **Volume ID**: Select the volume ID of the default volume attached to `TestInstance`.
     - **Name**: Enter a name for the snapshot, e.g., `TestSnapshot`.
     - **Description** (optional): Add a description like "Snapshot for TestInstance volume".
   - Click **Create Snapshot** to complete the process.

   ![Snapshot Creation](../images/snapshot_creation.png?featherlight=false&width=90pc)
   ![Snapshot Confirmation](../images/snapshot_confirmation.png?featherlight=false&width=90pc)

---

> **Did you know?** Snapshots are stored in Amazon S3 and incur costs based on their size. Regularly cleaning up unused snapshots can significantly reduce your AWS bill.