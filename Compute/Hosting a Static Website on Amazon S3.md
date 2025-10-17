## ☁️ Hosting a Static Website on Amazon S3

**Overview**

This project demonstrates how to deploy and manage a static website
using Amazon S3 and the AWS Command Line Interface (AWS CLI).

Throughout the process, I configured permissions, created IAM users,
uploaded web files, and automated website updates using a custom bash
script.

## 🧠 Objectives

-   By completing this project, I successfully:

-   Created an Amazon S3 bucket and configured it for static website
    hosting.

-   Created and configured an IAM user with full S3 access.

-   Uploaded and managed web content using the AWS CLI.

-   Built an automation script to update website files efficiently.

## ⚙️ Steps I Took

**Task 1: Connect to EC2 Instance via AWS Systems Manager (SSM)**

-   Connected to an Amazon Linux EC2 instance using Session Manager
    through the AWS Console.

-   Switched to the ec2-user with:

    -   *[sudo su -l ec2-user]{.underline}*

```{=html}
<!-- -->
```
-   Verified the home directory using:

    -   *[pwd]{.underline}*

**Task 2: Configure AWS CLI**

-   Configured the AWS CLI with credentials and region:

    -   aws configure

-   Entered Access Key, Secret Key, Region (us-west-2), and output
    format (json).

**Task 3: Create an Amazon S3 Bucket**

-   Created a new S3 bucket using:

    -   *[aws s3api create-bucket \--bucket \<unique-bucket-name\>
        \--region us-west-2 \--create-bucket-configuration
        LocationConstraint=us-west-2]{.underline}*

-   Verified bucket creation from the JSON output.

**Task 4: Create and Configure an IAM User**

-   Created a new IAM user:

    -   *[aws iam create-user \--user-name awsS3user]{.underline}*

-   Set a password for console login:

    -   aws iam create-login-profile \--user-name awsS3user \--password
        Training123!

-   Attached the AmazonS3FullAccess policy:

    -   aws iam attach-user-policy \--policy-arn
        arn:aws:iam::aws:policy/AmazonS3FullAccess \--user-name
        awsS3user

-   Logged in as awsS3user to verify access permissions.

**Task 5: Adjust S3 Bucket Permissions**

-   In the S3 console:

    -   Disabled Block all public access.

    -   Enabled ACLs under Object Ownership.

    -   Saved and confirmed changes to allow public website hosting.

**Task 6: Extract Website Files**

-   Extracted the provided website files:

    -   *[cd \~/sysops-activity-files]{.underline}*

    -   *[tar xvzf static-website-v2.tar.gz]{.underline}*

    -   *[cd static-website]{.underline}*

    -   *[ls]{.underline}*

-   Verified the presence of index.html, css/, and images/ folders.

**Task 7: Upload Files to S3 and Enable Website Hosting**

-   Enabled static website hosting:

    -   aws s3 website s3://\<my-bucket\>/ \--index-document index.html

-   Uploaded files with public read access:

    -   *[aws s3 cp /home/ec2-user/sysops-activity-files/static-website/
        s3://\<my-bucket\>/ \--recursive \--acl
        public-read]{.underline}*

-   Verified files uploaded successfully:

    -   *[aws s3 ls \<my-bucket\>]{.underline}*

-   Opened the Bucket Website Endpoint URL to confirm the site was live.

**Task 8: Automate Website Updates**

-   Created an update script:

    -   *[touch update-website.sh]{.underline}*

    -   *[vi update-website.sh]{.underline}*

-   Added the following content:

    -   *[#!/bin/bash]{.underline}*

    -   *[aws s3 cp /home/ec2-user/sysops-activity-files/static-website/
        s3://\<my-bucket\>/ \--recursive \--acl
        public-read]{.underline}*

-   Made the script executable:

    -   chmod +x update-website.sh

-   Updated website content locally and re-deployed with:

    -   ./update-website.sh

-   Verified changes reflected on the live website.

## Optional Challenge

-   Replaced the cp command with the more efficient sync command:

    -   *[aws s3 sync
        /home/ec2-user/sysops-activity-files/static-website/
        s3://\<my-bucket\>/ \--acl public-read]{.underline}*

-   This command only updated modified files, saving time and bandwidth.

## ✅ Outcome

**By the end of the lab:**

-   The website was successfully deployed and publicly accessible via
    the S3 static website URL.

-   I learned how to automate website updates and manage AWS resources
    securely using the CLI.

## 

## 🗂️ Files Included

-   update-website.sh --- Automation script for deploying website
    changes.

-   index.html --- Main static webpage.

-   /css --- Website styling files.

-   /images --- Media assets used in the website.

## 💡 Key AWS Services Used

-   Amazon S3 -- Static website hosting and file storage.

-   AWS IAM -- User creation and permissions management.

-   AWS CLI -- Automation and command-line resource management.

-   AWS Systems Manager (SSM) -- Secure instance access.

