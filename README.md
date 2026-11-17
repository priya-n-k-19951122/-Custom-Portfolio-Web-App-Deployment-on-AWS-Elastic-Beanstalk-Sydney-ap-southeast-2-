# Custom Portfolio Web App Deployment on AWS Elastic Beanstalk (Sydney — `ap-southeast-2`)

This repository documents the end-to-end Platform as a Service (PaaS) deployment of a custom HTML/CSS/PHP personal portfolio page using **AWS Elastic Beanstalk** in the **Asia Pacific (Sydney) — `ap-southeast-2`** region.

---

## Environment Overview & Resource Inventory

| Parameter | Configuration & Resource Details |
| :--- | :--- |
| **AWS Service** | AWS Elastic Beanstalk (Managed PaaS) |
| **AWS Region** | Asia Pacific (Sydney) — `ap-southeast-2` |
| **Application Name** | `priya-app` |
| **Environment Name** | `priya-app-env-1` |
| **Platform & Runtime** | PHP running on 64-bit Amazon Linux 2023 |
| **Web Server / Proxy** | Nginx + PHP-FPM |
| **Live Endpoint URL** | `http://elasticbeanstalk.com` |
| **Deployment Bundle** | `priya-ebs-portfolio-v2.zip` (`index.php` & `index.html`) |

---

## Repository Structure

```text
aws-elastic-beanstalk-portfolio/
├── index.php        # PHP entry point serving the portfolio page for Nginx
├── index.html       # Responsive HTML5/CSS3 personal portfolio & profile card
└── README.md        # Project documentation, deployment steps & troubleshooting
```

---

## Step-by-Step Deployment Guide

### Step 1: Package the Application Bundle
Ensure `index.php` and `index.html` are zipped directly at the root level of the archive (not nested inside an extra subfolder):
```bash
zip priya-ebs-portfolio-v2.zip index.php index.html
```

### Step 2: Create the Elastic Beanstalk Environment (`priya-app-env-1`)
1. Open the **AWS Management Console** and switch the region to **Asia Pacific (Sydney) `ap-southeast-2`**.
2. Navigate to **AWS Elastic Beanstalk** → **Create application** (or **Create new environment** inside `priya-app`).
3. Configure the environment settings:
   * **Environment tier:** `Web server environment`
   * **Environment name:** `priya-app-env-1`
   * **Platform:** `PHP` on `64bit Amazon Linux 2023`
4. Under **Application code**, select **Upload your code** → **Local file**, upload **`priya-ebs-portfolio-v2.zip`**, and click **Submit**.

### Step 3: Verify Deployment & Live Portfolio URL
1. Wait ~2 minutes until the environment initialization completes and **Health** turns **`Ok`**.
2. Open the generated endpoint in your browser:
   ```text
   http://elasticbeanstalk.com
   ```
3. Confirm the custom dark-mode portfolio card renders with profile image, bio, and AWS service badges.

---

## Key Troubleshooting Lessons & Cleanup

* **Resolving Nginx `403 Forbidden` on PHP Platform:** AWS Elastic Beanstalk's PHP platform configures Nginx to route root requests to `index.php` by default. Uploading a `.zip` containing only `index.html` triggers a `403 Forbidden` response until an `index.php` entry point is included.
* **Root-Level Zip Packaging:** Always zip application files directly from inside the project directory (`zip bundle.zip index.php index.html`) rather than zipping the parent folder, so Elastic Beanstalk finds the entry files at `/var/www/html/`.
* **Post-Lab Resource Cleanup:** After testing and capturing portfolio screenshots, open **Elastic Beanstalk → Environments**, select **`priya-app-env-1`** (and any earlier test environments), and click **Actions → Terminate environment** to stop all underlying EC2 billing.
