# ☁️ Hosting a Static Website on Amazon S3

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

![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d246fc15eb86b5e57ffbad82ccaa6f49a3be2ce8/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2274).png)

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
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2275).png)

**Task 2: Configure AWS CLI**

-   Configured the AWS CLI with credentials and region:

    -   aws configure

-   Entered Access Key, Secret Key, Region (us-west-2), and output
    format (json).
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2276).png)
**Task 3: Create an Amazon S3 Bucket**

-   Created a new S3 bucket using:

    -   *[aws s3api create-bucket \--bucket \rpolelo999
        \--region us-west-2 \--create-bucket-configuration
        LocationConstraint=us-west-2]{.underline}*
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2277).png)
-   Verified bucket creation from the JSON output.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2278).png)

**Task 4: Create and Configure an IAM User**

-   Created a new IAM user:

    -   *[aws iam create-user \--user-name awsS3user]{.underline}*
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2280).png)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2281).png)
-   Set a password for console login:

    -   aws iam create-login-profile \--user-name awsS3user \--password
        Training123!
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2282).png)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2283).png)
-   Attached the AmazonS3FullAccess policy:

    -   aws iam attach-user-policy \--policy-arn
        arn:aws:iam::aws:policy/AmazonS3FullAccess \--user-name
        awsS3user
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2284).png)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2285).png)
-   Logged in as awsS3user to verify access permissions.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2288).png)

**Task 5: Adjust S3 Bucket Permissions**

-   In the S3 console:

    -   Disabled Block all public access.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2290).png) ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2292).png) ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2294).png)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2295).png) ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/d5918507cb159d3235f004c7cd11adabfb3d08d4/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2296).png) 
    -   Enabled ACLs under Object Ownership.
![image alt]() ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/f3cc7d978cbd3561d901885d2ee58243815c5b91/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2297).png)
![image alt]() ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/f3cc7d978cbd3561d901885d2ee58243815c5b91/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2300).png)
![image alt]() ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/f3cc7d978cbd3561d901885d2ee58243815c5b91/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2301).png)
    -   Saved and confirmed changes to allow public website hosting.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/f3cc7d978cbd3561d901885d2ee58243815c5b91/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2302).png)



**Task 6: Extract Website Files**

-   Extracted the provided website files:

    -   *[cd \~/sysops-activity-files]{.underline}*

    -   *[tar xvzf static-website-v2.tar.gz]{.underline}*

    -   *[cd static-website]{.underline}*

    -   *[ls]{.underline}*

-   Verified the presence of index.html, css/, and images/ folders.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/f3cc7d978cbd3561d901885d2ee58243815c5b91/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2304).png) 

**Task 7: Upload Files to S3 and Enable Website Hosting**

-   Enabled static website hosting:

    -   aws s3 website s3://\<my-bucket\>/ \--index-document index.html

-   Uploaded files with public read access:

    -   *[aws s3 cp /home/ec2-user/sysops-activity-files/static-website/
        s3://\<my-bucket\>/ \--recursive \--acl
        public-read]{.underline}*

-   Verified files uploaded successfully:

    -   *[aws s3 ls \<my-bucket\>]{.underline}*
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/f3cc7d978cbd3561d901885d2ee58243815c5b91/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2309).png)
-   Opened the Bucket Website Endpoint URL to confirm the site was live.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/f3cc7d978cbd3561d901885d2ee58243815c5b91/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2313).png)


**Task 8: Automate Website Updates**

-   Created an update script:

    -   *[touch update-website.sh]{.underline}*

    -   *[vi update-website.sh]{.underline}*
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/f3cc7d978cbd3561d901885d2ee58243815c5b91/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2318).png) 
-   Added the following content:

    -   *[#!/bin/bash]{.underline}*

    -   *[aws s3 cp /home/ec2-user/sysops-activity-files/static-website/
        s3://\<my-bucket\>/ \--recursive \--acl
        public-read]{.underline}*
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/f3cc7d978cbd3561d901885d2ee58243815c5b91/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2320).png) 
-   Made the script executable:

    -   chmod +x update-website.sh

-   Updated website content locally and re-deployed with:

    -   ./update-website.sh

-   Verified changes reflected on the live website.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/f3cc7d978cbd3561d901885d2ee58243815c5b91/Images/Lab%20Images/Compute/Creating%20Amazon%20EC2%20Instance/Screenshot%20(2323).png) 


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

