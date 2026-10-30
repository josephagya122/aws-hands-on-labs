# Lab 4: Build a Database Server

### Objective
To build a multi-tier infrastructure by configuring a secure VPC DB Subnet Group across isolated private subnets, creating a Database Security Group restricted to inbound MySQL/Aurora traffic from a Web Security Group, deploying an Amazon RDS MySQL database instance, and connecting a web application tier to the RDS database layer.

---

### Architecture & Key Components
* **VPC**: `Lab VPC` (`vpc-0d1683675a23f7564`).
* **Subnet Group**: `db-subnet-group` referencing isolated `Private Subnet 1` (`10.0.1.0/24`) and `Private Subnet 2` (`10.0.3.0/24`) across Availability Zones `us-east-1a` and `us-east-1b`.
* **Security Groups**:
  * `Web Security Group`: Attached to the EC2 web application tier.
  * `DB Security Group`: Restricted to accept **MySQL/Aurora (Port 3306)** inbound traffic sourced exclusively from `Web Security Group` (`sg-0a1073d4bae04ca47`).
* **Database Engine**: Amazon RDS MySQL (`Dev/Test` multi-AZ deployment with DB instance identifier `lab-db`).

---

### Step-by-Step Implementation Guide

#### Step 1: Create a DB Security Group
1. Open the **VPC Console** and navigate to **Security Groups**.
2. Note the ID of the existing `Web Security Group` (e.g., `sg-0a1073d4bae04ca47`).
3. Click **Create security group**:
   * **Security group name**: `DB Security Group`
   * **Description**: `Permit access from Web Security Group`
   * **VPC**: Select `Lab VPC` (`vpc-0d1683675a23f7564`)
4. Under **Inbound rules**, click **Add rule**:
   * **Type**: `MYSQL/Aurora`
   * **Port**: `3306`
   * **Source**: `Custom` -> Select `Web Security Group` (`sg-0a1073d4bae04ca47`)
5. Click **Create security group**.

#### Step 2: Create a DB Subnet Group
1. Open the **Amazon RDS Console** and select **Subnet groups** from the left navigation pane.
2. Click **Create DB subnet group**:
   * **Name**: `db-subnet-group`
   * **Description**: `DB Subnet Group`
   * **VPC**: Select `Lab VPC` (`vpc-0d1683675a23f7564`)
3. Under **Add subnets**:
   * **Availability Zones**: Choose `us-east-1a` and `us-east-1b`.
   * **Subnets**: Select `Private Subnet 1` (`10.0.1.0/24`) and `Private Subnet 2` (`10.0.3.0/24`).
4. Click **Create**.

#### Step 3: Launch an Amazon RDS MySQL Database Instance
1. In the RDS Console, navigate to **Databases** and click **Create database**.
2. **Choose a database creation method**: Select `Full configuration`.
3. **Engine options**: Select `MySQL`.
4. **Templates**: Select `Dev/Test`.
5. **Availability and durability**: Select `Multi-AZ DB instance deployment (2 instances)`.
6. **Settings**:
   * **DB instance identifier**: `lab-db`
   * **Master username**: `main`
   * **Master password**: Set your lab database credentials.
7. **Connectivity**:
   * **VPC**: Select `Lab VPC`.
   * **DB Subnet Group**: Select `db-subnet-group`.
   * **Public access**: `No`.
   * **Existing VPC security groups**: Select `DB Security Group` and remove any default security groups.
8. **Maintenance**: Leave minor version upgrades enabled and click **Create database**.

#### Step 4: Connect the Web Application to the RDS Database
1. Once the database status changes to **Available**, copy its **Endpoint** (e.g., `lab-db.cpi8oa0owp10.us-east-1.rds.amazonaws.com`).
2. Open a browser and navigate to the web server's RDS testing page (e.g., `http://<Web-Server-Public-IP>/rds.php`).
3. Enter the required connection parameters:
   * **Endpoint**: Paste the RDS DB Endpoint.
   * **Database**: `lab`
   * **Username**: `main`
   * **Password**: Enter your database master password.
4. Click **Submit** to establish connectivity and verify database access from the web application tier.


<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/3dc6fad2-654f-4cac-ac7a-a5e097541211" />
