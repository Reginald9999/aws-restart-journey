##  Creating Amazon EC2 Instances --- AWS Lab

**Overview**

This lab demonstrates how to launch and configure Amazon EC2 instances
using both the AWS Management Console and the AWS Command Line Interface
(CLI).

You create a bastion host to securely access the environment, and then
launch a web server instance from the CLI.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2345).png) 

## Steps Taken

**Task 1 --- Launch EC2 Instance (Bastion Host) using AWS Management
Console**

-   Opened the AWS Management Console and navigated to the EC2 service.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2346).png)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2348).png) 

-   Selected Launch Instance and entered the name Bastion host.

-   Chose Amazon Linux 2 AMI (HVM) as the operating system.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2350).png)

-   Selected t3.micro as the instance type.

-   Proceeded without a key pair, since EC2 Instance Connect was used
    for access.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2351).png)

-   Configured network settings:

-   Selected Lab VPC and Public Subnet.

-   Created a new Security Group named Bastion security group to allow
    SSH (port 22).
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2352).png)

-   Kept the default storage of 8 GiB.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2353).png)

-   Assigned the IAM role Bastion-Role to allow CLI access to EC2.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2354).png)

-   Launched the instance and verified it appeared in the Instances
    list.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2355).png)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2357).png)


## Task 2 --- Connect to Bastion Host

-   Selected the Bastion host instance in the EC2 console.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2357).png)
-   Chose Connect → EC2 Instance Connect.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2358).png)
-   Successfully connected to the instance shell without using a key
    pair.
-   Verified access and AWS CLI readiness for further commands.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2359).png)


## Task 3 --- Launch EC2 Instance (Web Server) using AWS CLI

-   Retrieved the latest Amazon Linux 2 AMI:

    -   AZ=\`curl -s
        <http://169.254.169.254/latest/meta-data/placement/availability-zone%60>
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2360).png)

    -   Export AWS_DEFAULT_REGION=\${AZ::-1}
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2362).png)

    -   AMI=\$(aws ssm get-parameters --names
        /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2
        --query 'Parameters\[0\].\[Value\]' --output text)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2365).png)

-   Retrieved the Public Subnet ID:

    -   SUBNET=\$(aws ec2 describe-subnets --filters
        'Name=tag:Name,Values=Public Subnet' --query
        Subnets\[\].SubnetId --output text)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2368).png)

-   Retrieved the Web Security Group ID:

    -   SG=\$(aws ec2 describe-security-groups --filters
        Name=group-name,Values=WebSecurityGroup --query
        SecurityGroups\[\].GroupId --output text)
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2372).png)


-   Downloaded and reviewed the user data script to install Apache and
    deploy a web app:

    -   Wget
        <https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/.../UserData.txt>
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2375).png)

       -   Cat UserData.txt
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2377).png)


-   Launched the Web Server instance:

> INSTANCE=\$(
>
> Aws ec2 run-instances \\
>
> \--image-id \$AMI \\
>
> \--subnet-id \$SUBNET \\
>
> \--security-group-ids \$SG \\
>
> \--user-data [file:///home/ec2-user/UserData.txt
> \\](file:///home/ec2-user/UserData.txt%20\)
>
> \--instance-type t3.micro \\
>
> \--tag-specifications
> 'ResourceType=instance,Tags=\[{Key=Name,Value=Web Server}\]' \\
>
> \--query 'Instances\[\*\].InstanceId' \\
>
> \--output text
>
> )

![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2380).png)

-   Monitored the instance status until it was running:

    -   Aws ec2 describe-instances --instance-ids \$INSTANCE --query
        'Reservations\[\].Instances\[\].State.Name' --output text
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2382).png)

-   Tested the web server by retrieving its public DNS name:

    -   Aws ec2 describe-instances --instance-ids \$INSTANCE --query
        'Reservations\[\].Instances\[\].PublicDnsName' --output text

-   Opened the DNS in a browser and confirmed that the web page loaded
    successfully.
![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2386).png)

## Optional Challenges (If Completed)

-   **Challenge 1**: Diagnosed and corrected SSH connection issues with
    a misconfigured instance.

  ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2389).png)  
  ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2390).png)
  ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2391).png)
  ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2394).png)
  ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2404).png)
  ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2405).png)
  ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2406).png)
  ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2407).png)

-   **Challenge 2**: Troubleshot and fixed web server installation
    issues on a faulty EC2 instance.
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2410).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2415).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2412).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2413).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2414).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2416).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2417).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2418).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2419).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2420).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2421).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2422).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2423).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2424).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2425).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2426).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2427).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2428).png)
     ![image alt](https://github.com/Reginald9999/aws-restart-journey/blob/83ca3e22b3e1e7535826a7ef865eb264c827fc5a/Images/Lab%20Images/Compute/Hosting%20a%20Static%20Website%20on%20Amazon%20S3/Screenshot%20(2429).png)

## ✅ Results

-   Successfully launched and configured a bastion host and web server.

-   Verified secure access via EC2 Instance Connect and AWS CLI.

-   Demonstrated how to automate EC2 provisioning using scripts.

##  Key Takeaways

-   EC2 instances can be launched using multiple methods (Console, CLI,
    or CloudFormation).

-   Using AWS CLI enables automation, consistency, and reduced human
    error.

-   IAM roles simplify permissions and secure access without keys.

-   Security Groups act as firewalls controlling inbound/outbound
    traffic.
