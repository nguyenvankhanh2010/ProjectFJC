---
title: "Section II: Technologies Used"
weight: 2
chapter: false
---

# ⚙️ Technologies Used

A good understanding of the key AWS services involved will help you follow and customize this workshop effectively. Here’s a breakdown of each core component.

---

## 2.1 AWS EC2

**Amazon Elastic Compute Cloud (EC2)** provides scalable compute capacity in the cloud.  
In this workshop, we use EC2 to:
- Launch an instance as a sample workload.
- Attach an EBS volume to store data.
- Terminate the instance later to simulate an abandoned resource.

---

## 2.2 EBS (Elastic Block Store)

**Elastic Block Store (EBS)** provides block-level storage for EC2 instances.
In this workshop:
- Each EC2 instance automatically gets at least one EBS volume.
- You will create snapshots of this volume for backup.
- Unused snapshots after termination are the main cost-saving target.

---

## 2.3 Snapshots

**Snapshots** are point-in-time backups of EBS volumes.
- They are incremental but occupy storage.
- If you forget to delete them, they keep incurring costs.
- Our Lambda function will find and delete stale snapshots.

---

## 2.4 AWS Lambda

**AWS Lambda** is a serverless compute service that runs code without provisioning servers.
In this workshop:
- Lambda runs Python code that checks all your EC2 instances and EBS snapshots.
- If it finds a snapshot not linked to any active instance, it deletes it.
- You can run it manually or schedule it.

---

## 2.5 IAM (Identity and Access Management)

**IAM** helps you manage access to AWS services securely.
- We will create an IAM Policy with permissions: `DescribeInstances`, `DescribeVolumes`, `DescribeSnapshots`, and `DeleteSnapshot`.
- We will attach this policy to the Lambda execution role so it can read & delete resources.

---

## 2.6 CloudWatch / EventBridge

**CloudWatch** and **EventBridge** are AWS monitoring and event services.
- In this workshop, they are used to schedule and trigger the Lambda function automatically.
- For example, you can set it to run every hour, day, or week.
- Be mindful: frequent execution may increase Lambda charges.

---

> **Tip:** Mastering these services makes you more effective at managing cost automation in AWS.
