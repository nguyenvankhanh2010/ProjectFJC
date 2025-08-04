---
title: "Section II: Technologies Used"
date: 2025-08-04
weight: 2
chapter: false
---

# ⚙️ Section II: Technologies Used

In this section, you’ll explore each AWS service involved in the stale resource detection solution. Each plays a specific role in managing storage costs efficiently.

---

## <b> 2.1 </b> AWS EC2

**Amazon Elastic Compute Cloud (EC2)** provides scalable virtual servers.  
In this workshop, EC2 acts as the workload that generates volumes and snapshots.  
You’ll launch an instance, attach storage, and terminate it to see how leftover snapshots waste money.

<img src="https://raw.githubusercontent.com/phamr39/ezidev-imagestorage/master/aws-saa-c03/aws-5-gioi-thieu-ve-aws-ec2/Amazon-EC2.jpg" alt="Amazon EC2" style="width:70%;max-width:600px;display:block;margin:auto;" />

*Amazon EC2 – Virtual Servers in the Cloud*

---

## <b> 2.2 </b> EBS (Elastic Block Store)

**Amazon EBS** provides block-level storage for EC2 instances.  
When you launch an instance, a default EBS volume is attached.  
You’ll create a snapshot of this volume to simulate a backup — and see how it incurs cost if not deleted.

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQfYFsIM-aH7AtvdQrrthx4tfDrJzz1Cj6QvQ&s" alt="Amazon EBS" style="width:70%;max-width:600px;display:block;margin:auto;" />

*Amazon EBS – Block Storage for EC2*

---

## <b> 2.3 </b> Snapshots

**Snapshots** are point-in-time backups of EBS volumes.  
They’re incremental, but they accumulate storage costs if forgotten.  
Our Lambda function finds and deletes stale snapshots to keep your AWS bill under control.

<img src="https://miro.medium.com/v2/resize:fit:1400/1*GVeaZPArzgwRUtpvLbFELA.jpeg" alt="EBS Snapshots" style="width:70%;max-width:600px;display:block;margin:auto;" />

*EBS Snapshots – Backup & Restore*

---

## <b> 2.4 </b> AWS Lambda

**AWS Lambda** lets you run serverless code.  
In this project, Lambda runs Python code to scan snapshots and EC2 instances, then deletes any snapshot not linked to an active instance.  
No servers, pure automation.

<img src="https://assets.dio.me/6UJHZEQOJZcmQJ-VaiGgwlpgb_91VAJVJKBAVKe_ens/f:webp/q:80/L2FydGljbGVzL2NvdmVyL2JlYjk1NjE1LWRiYzctNGE3Ni04NmFiLTJjODM4ZDNkNzY5Mi5qcGc" alt="AWS Lambda" style="width:70%;max-width:600px;display:block;margin:auto;" />

*AWS Lambda – Serverless Automation*

---

## <b> 2.5 </b> IAM (Identity and Access Management)

**IAM** controls who can do what in your AWS account.  
You’ll create a policy with permissions to:
- `DescribeInstances`
- `DescribeVolumes`
- `DescribeSnapshots`
- `DeleteSnapshot`

Then, you’ll attach this policy to the Lambda execution role to enable safe, controlled automation.

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSXmo0KadhhREpXe6xuxLi36HB0YLhXWNciVhPKtyyxOmNqs-GdDgjUTzuc9XOT7M7ePe0&usqp=CAU" alt="AWS IAM" style="width:70%;max-width:600px;display:block;margin:auto;" />

*AWS IAM – Secure Access Management*

---

## <b> 2.6 </b> CloudWatch / EventBridge

**CloudWatch** and **EventBridge** help automate and monitor your environment.  
You can schedule the Lambda to run hourly, daily, or on specific events to ensure stale snapshots are detected and removed continuously.

<img src="https://razorops.com/images/blog/amazon-cloudwatch.webp" alt="Amazon CloudWatch" style="width:70%;max-width:600px;display:block;margin:auto;" />

*Amazon CloudWatch – Monitoring & Scheduling*
