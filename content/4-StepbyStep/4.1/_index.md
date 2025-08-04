---
title: "Step 2.1: Launch EC2 Instance and Create Snapshot"
date: 2025-07-07
weight: 4
chapter: false
pre: " <b> 4.1 </b> "
---


# Step 2.1: Launch EC2 Instance and Create Snapshot

#### Why It Matters

This step sets up an EC2 instance and creates a snapshot to simulate a common scenario where developers create snapshots for backups but may forget to delete them after terminating the instance, leading to unnecessary costs.

---

## Instructions

1. **Log into AWS Console**:
   - Open your browser and log into the [AWS Management Console](https://aws.amazon.com/console/).
   - Ensure you have the necessary credentials and permissions to access EC2.

2. **Navigate to EC2 Console**:
   - From the AWS Console homepage, click **Services** → **EC2** to access the EC2 Console.

3. **Launch an EC2 Instance**:
   - In the **Instances** section, click **Instances** in the left sidebar.
   - Click **Launch Instance**.
   - Configure the instance:
     - **Name**: `TestInstance`.
     - **AMI**: Select **Amazon Linux 2** or **Ubuntu Server LTS**.
     - **Instance Type**: Choose `t2.micro` (free tier eligible).
     - **Key Pair**: Select an existing RSA PEM key pair or create a new one.
     - **Network Settings**: Use the default VPC and subnet, enable **Auto-assign Public IPv4**.
     - **Storage**: Keep the default 8 GiB gp2 volume.
   - Click **Launch Instance**.
   - Wait for the instance to show **2/2 checks passed** in the status.

![EC2 Instance Launch](../images/ec2_launch.png?featherlight=false&width=90pc)

4. **Create a Snapshot**:
   - Navigate to **Elastic Block Store** → **Volumes** in the EC2 Console.
   - Identify the default volume attached to `TestInstance` (created automatically during instance launch).
   - Go to **Snapshots** → Click **Create Snapshot**.
   - In the **Volume ID** section, select the volume ID of the default volume.
   - Provide a **Name** for the snapshot (e.g., `TestSnapshot`).
   - Scroll down and click **Create Snapshot**.

![Snapshot Creation](../images/CreateSnapshot1.png.png?featherlight=false&width=90pc)
![Snapshot Creation](../images/CreateSnapshot2.png.png?featherlight=false&width=90pc)


---

> **Did you know?** Snapshots are stored in Amazon S3 and incur costs based on their size. Regularly cleaning up unused snapshots can significantly reduce your AWS bill.