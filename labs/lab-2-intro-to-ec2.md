# Lab 2: Introduction to Amazon EC2

### Objective
To gain hands-on experience launching and configuring Amazon EC2 instances, deploying an Apache Web Server using User Data scripts, modifying Security Group inbound rules for HTTP traffic, and managing instance lifecycle states (stop, monitor, volume inspection).

---

### Key Services & Components
* **Amazon EC2**: Compute instances (`t3.micro`) running Amazon Linux.
* **EC2 User Data**: Automated shell script execution at boot to install and configure Apache (`httpd`).
* **Security Groups**: Stateful virtual firewalls managing SSH (Port 22) and HTTP (Port 80) inbound access.
* **Amazon EBS**: Elastic Block Store volume management attached to EC2 instances.

---

### Step-by-Step Implementation Guide

#### Step 1: Launch the EC2 Web Server Instance
1. In the AWS Management Console, navigate to the **EC2 Console** and click **Launch Instance**.
2. **Name**: `Web Server`
3. **Instance Type**: Select `t3.micro`.
4. **Network Settings**:
   * **VPC**: Select `Lab VPC` (`10.0.0.0/16`).
   * **Subnet**: Select `PublicSubnet1`.
   * **Auto-assign Public IP**: `Enable`.
5. **Firewall (Security Groups)**:
   * Select **Create security group**.
   * **Security group name**: `Web Server security group`
   * **Description**: `Security group for my web server`
   * **Inbound Rules**: Allow `SSH` (Port 22) from `0.0.0.0/0` (Anywhere-IPv4).

#### Step 2: Configure Web Server Automation (User Data)
1. Expand the **Advanced details** section during instance setup.
2. Scroll to the **User data** field and enter the shell script below to automate package installation and index page creation:

```bash
#!/bin/bash
dnf install -y httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
