# Lab 3: Working with Amazon Elastic Block Store (EBS)

### Objective
To gain hands-on experience managing persistent block storage in AWS by creating an Amazon EBS volume, attaching it to an EC2 instance, formatting and mounting a filesystem, taking a point-in-time snapshot, restoring that snapshot to a new EBS volume, and verifying data persistence across volumes using AWS Systems Manager Session Manager.

---

### Architecture & Key Components
* **EC2 Instance**: `Lab` (`i-00dad0be864fe324e`) running in availability zone `us-east-1a`.
* **Original EBS Volume**: `My Volume` (`vol-049fa827d0d7535fa`) - 100 GiB `gp2` block volume.
* **EBS Snapshot**: `My Snapshot` (`snap-0cdb374372b5caf24`) - Point-in-time backup of `My Volume`.
* **Restored EBS Volume**: `Restored Volume` (`vol-0b80ab0725e867f41`) - 100 GiB `gp3` volume restored from `My Snapshot`.
* **Mount Points**: `/mnt/data-store` (Original volume) and `/mnt/data-store2` (Restored volume).

---

### Step-by-Step Implementation Guide

#### Step 1: Create and Attach an EBS Volume
1. In the EC2 Management Console, select **Volumes** under **Elastic Block Store** in the left menu.
2. Click **Create volume**:
   * **Volume Type**: `General Purpose SSD (gp2)`
   * **Size**: `100 GiB`
   * **Availability Zone**: `us-east-1a` (must match the AZ of the `Lab` instance)
   * **Tag**: Key `Name` with Value `My Volume`
3. Click **Create volume**.
4. Select `My Volume` (`vol-049fa827d0d7535fa`), click **Actions** -> **Attach volume**.
5. Select instance `Lab` (`i-00dad0be864fe324e`) and set device name `/dev/sdb`. Click **Attach volume**.

#### Step 2: Initialize, Mount, and Populate the Volume via SSM Session Manager
1. Select the `Lab` instance and connect via **AWS Systems Manager Session Manager**.
2. Format the new block device with an `ext3` filesystem:
   ```bash
   sudo mkfs -t ext3 /dev/sdb
