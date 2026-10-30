# Lab 5: Deploying and Managing Applications with AWS Elastic Beanstalk

### Objective
To deploy, configure, and manage scalable web applications using **AWS Elastic Beanstalk**, upload a sample application package (`tomcat.zip`), monitor environment health and event logs, review managed platform details, and examine the underlying provisioned EC2 instances.

---

### Architecture & Key Components
* **Elastic Beanstalk Application**: Sample application running on a load-balanced web server environment.
* **Platform**: Tomcat 9 with Corretto 11 running on 64bit Amazon Linux 2023 (Platform Version `5.14.11`)[cite: 39, 42].
* **Environment Tier**: Web server environment (Load balanced).
* **Compute Resources**: Provisioned EC2 instances (`t3.micro`) running across multiple Availability Zones (`us-east-1b`, `us-east-1c`).

---

### Step-by-Step Implementation Guide

#### Step 1: Access and Inspect Elastic Beanstalk Environment
1. Open the **AWS Elastic Beanstalk Console** and navigate to your active environment (`c2377-samp-G37mvex4k0Jq`).
2. Verify the **Environment health** is reported as **Ok**.
3. Note the assigned **Domain URL** (e.g., `c2377-samp-G37mvex4k0Jq.eba-y4ke6cki.us-east-1.elasticbeanstalk.com`) used to access the web application.

#### Step 2: Upload and Deploy an Application Package
1. On the environment dashboard, click **Upload and deploy**[cite: 39, 40].
2. In the upload dialog:
   * Click **Choose file** and select your application package (`tomcat.zip`).
   * Review the auto-generated **Version label**[cite: 40].
   * Check **Deployment preferences** (configured to use the `AllAtOnce` policy across the active instances)[cite: 40].
3. Click **Deploy**[cite: 40].
4. Monitor the deployment progress via the green notification banner and the **Events** tab.

#### Step 3: Monitor Deployment Events & Health Status
1. Navigate to the **Events** tab in the Elastic Beanstalk console[cite: 41].
2. Verify event logs tracking the deployment stages[cite: 41]:
   * Successful file upload to Amazon S3[cite: 41].
   * Creation of the new application version[cite: 41].
   * Execution of instance deployment across running EC2 instances (`2 out of 2 instances completed`)[cite: 41].
   * Transition of environment health from `Info` back to `Ok` once the update completes successfully[cite: 41].

#### Step 4: Review Configuration and Underlying EC2 Instances
1. Select the **Configuration** tab in the environment dashboard to inspect platform details (Tomcat 9, Corretto 11, Amazon Linux 2023) and environment type (`Load balanced`)[cite: 42].
2. Open the **EC2 Console** and navigate to **Instances**[cite: 43].
3. Verify that Elastic Beanstalk automatically manages the underlying compute fleet, showing running `t3.micro` instances across different Availability Zones with health status checks passing (`3/3 checks passed`)[cite: 43].
