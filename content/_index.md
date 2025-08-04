---
title: "Host Web A.I ReactJS & NestJS on AWS"
weight: 1
chapter: false
---

# Introduction

#### Overview  
Welcome to this hands‑on lab where you will learn to deploy a complete, highly available, and secure full‑stack AI web application on AWS. Over the course of this exercise you will:

1. **Design and build a resilient network**  
   Create a multi‑AZ Virtual Private Cloud (VPC) with three public subnets and one private subnet. You’ll attach an Internet Gateway, define custom route tables, and lock down traffic with security groups—laying the foundation for both public‑facing and private compute resources.

2. **Containerize and host your AI service**  
   Launch a t2.medium EC2 instance in a public subnet, install Docker, clone your AI code repository, and spin up your AI model inside containers. You’ll verify external connectivity and integrate the AI endpoint into your backend.

3. **Deploy your Node.js (NestJS) backend**  
   Provision a t2.micro EC2 instance, install Node.js and PostgreSQL, synchronize your code via `rsync`, configure Prisma for database migrations, and run your backend in production mode. You’ll learn how to securely connect to both your containerized AI service and your database.

4. **Publish a ReactJS frontend to S3**  
   Build your React app, configure an S3 bucket for static website hosting, apply a bucket policy for public reads, and upload your build artifacts. Your frontend will then serve as the user interface, dynamically fetching data from your NestJS GraphQL API.

5. **Provision a managed PostgreSQL database on RDS**  
   Create an RDS PostgreSQL instance within your VPC, secure it with a dedicated security group, and connect via pgAdmin. You will execute Prisma migrations to initialize your schema and seed data, demonstrating how to integrate a managed database service into your stack.

6. **Clean up and cost‑control**  
   Learn best practices for resource teardown: empty and delete your S3 bucket, terminate EC2 instances, and remove your RDS instance to avoid ongoing charges.

#### Why This Matters  
By completing these steps, you will gain practical experience with core AWS services—VPC, EC2, S3, and RDS—and see firsthand how they integrate to support modern AI‑driven web applications. You’ll become familiar with infrastructure as code concepts, container orchestration with Docker, secure networking, and managed database workflows. This lab not only equips you with deployable artifacts but also instills best practices for security, high availability, and cost management in the cloud.

---

#### Lab Sections

1. [VPC Networking & Security (Section I)](1-VPC-networking/)  
   Build your VPC with public and private subnets, Internet Gateway, route tables, and security groups.

2. [AI Service on EC2 (Section II)](2-ai-on-ec2/)  
   Containerize and deploy your AI model in Docker on an EC2 instance.

3. [NestJS Backend on EC2 (Section III)](3-backend-NestJS-on-ec2/)  
   Launch a Node.js backend, sync code with `rsync`, configure Prisma, and connect to PostgreSQL.

4. [ReactJS Frontend on S3 (Section IV)](4-frontend-ReactJS-on-s3/)  
   Build and host your React application as a static website on S3 with public access.

5. [PostgreSQL on RDS (Section V)](5-postgresql-on-rds/)  
   Provision, secure, and connect to an Amazon RDS PostgreSQL database, then run migrations.

6. [Resource Cleanup (Section VI)](6-cleanup/)  
   Tear down services and delete resources to maintain a cost‑efficient AWS environment.

---

Ready? Let’s get started with Section I and lay the networking groundwork for your AI‑powered web application!  
