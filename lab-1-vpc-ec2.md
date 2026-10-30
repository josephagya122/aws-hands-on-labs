# AWS Cloud Labs Portfolio

This repository contains documentation and step-by-step implementation guides for various hands-on AWS labs, focusing on cloud networking, compute resources, and security fundamentals.

---

## Lab 1: Build Your VPC and Launch EC2 Instances

### Objective
To design and configure a custom Amazon Virtual Private Cloud (VPC) from scratch with public/private subnets, internet gateways, route tables, and launch Amazon EC2 instances to verify connectivity and web application responses.

### Architecture Overview
* **VPC**: Custom VPC configured with an IPv4 CIDR block.
* **Subnets**: Public and private subnets mapped across Availability Zones.
* **Networking Components**: Internet Gateway (IGW) attached to the VPC, alongside configured Route Tables for traffic routing.
* **Compute**: EC2 instances launched (such as a Bastion/Lab host and web server instances) running Amazon Linux.

---

### Step-by-Step Implementation Guide

#### Step 1: Create a Custom VPC
1. Navigate to the **VPC Console**.
2. Click **Create VPC**.
3. Select **VPC and more** (or use the VPC wizard) to automatically configure subnets, route tables, and gateways, or manually configure:
   * **Name tag**: `1lab-vpc`[cite: 4]
   * **IPv4 CIDR block**: `10.0.0.0/16`[cite: 4]
4. Click **Create VPC**[cite: 3].

#### Step 2: Configure Subnets and Internet Gateway
1. Create public and private subnets within your VPC CIDR block (e.g., `10.0.0.0/24`)[cite: 4].
2. Navigate to **Internet Gateways**, click **Create Internet Gateway**, and attach it to your custom VPC (`1-lab-vpc`)[cite: 3].
3. Create custom **Route Tables** for public subnets and add a route pointing `0.0.0.0/0` to your Internet Gateway[cite: 3].

#### Step 3: Launch EC2 Instances
1. Navigate to the **EC2 Console** and click **Launch Instance**[cite: 1].
2. Configure your instance details:
   * **Name**: `LabHost` / `Server 1`[cite: 1, 2]
   * **AMI**: Amazon Linux (e.g., `al2023-ami-...`)[cite: 7]
   * **Instance Type**: `t2.micro` or `t3.micro`[cite: 1, 2]
   * **Key Pair**: Select your designated key pair (`vockey`)[cite: 7].
   * **Network Settings**: Select your custom VPC and public subnet with auto-assign public IP enabled.
3. Review configurations and click **Launch Instance**.

#### Step 4: Verify Deployment and Application Load
1. Once the instances enter the **Running** state, verify their Public IPv4 addresses and Status Checks (`2/2 checks passed`)[cite: 1, 2].
2. Access deployed web or load testing scripts via the public DNS/IP to confirm operational status (e.g., CPU load monitoring pages)[cite: 8].
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/e0ec9f17-0990-42f7-922d-f292fe519b91" />
