##  Creating Amazon EC2 Instances --- AWS Lab

**Overview**

This lab demonstrates how to launch and configure Amazon EC2 instances
using both the AWS Management Console and the AWS Command Line Interface
(CLI).

You create a bastion host to securely access the environment, and then
launch a web server instance from the CLI.

## Steps Taken

**Task 1 --- Launch EC2 Instance (Bastion Host) using AWS Management
Console**

-   Opened the AWS Management Console and navigated to the EC2 service.

-   Selected Launch Instance and entered the name Bastion host.

-   Chose Amazon Linux 2 AMI (HVM) as the operating system.

-   Selected t3.micro as the instance type.

-   Proceeded without a key pair, since EC2 Instance Connect was used
    for access.

-   Configured network settings:

-   Selected Lab VPC and Public Subnet.

-   Created a new Security Group named Bastion security group to allow
    SSH (port 22).

-   Kept the default storage of 8 GiB.

-   Assigned the IAM role Bastion-Role to allow CLI access to EC2.

-   Launched the instance and verified it appeared in the Instances
    list.

## Task 2 --- Connect to Bastion Host

-   Selected the Bastion host instance in the EC2 console.

-   Chose Connect → EC2 Instance Connect.

-   Successfully connected to the instance shell without using a key
    pair.

-   Verified access and AWS CLI readiness for further commands.

## Task 3 --- Launch EC2 Instance (Web Server) using AWS CLI

-   Retrieved the latest Amazon Linux 2 AMI:

    -   AZ=\`curl -s
        <http://169.254.169.254/latest/meta-data/placement/availability-zone%60>

    -   Export AWS_DEFAULT_REGION=\${AZ::-1}

    -   AMI=\$(aws ssm get-parameters --names
        /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2
        --query 'Parameters\[0\].\[Value\]' --output text)

```{=html}
<!-- -->
```
-   Retrieved the Public Subnet ID:

    -   SUBNET=\$(aws ec2 describe-subnets --filters
        'Name=tag:Name,Values=Public Subnet' --query
        Subnets\[\].SubnetId --output text)

```{=html}
<!-- -->
```
-   Retrieved the Web Security Group ID:

    -   SG=\$(aws ec2 describe-security-groups --filters
        Name=group-name,Values=WebSecurityGroup --query
        SecurityGroups\[\].GroupId --output text)

```{=html}
<!-- -->
```
-   Downloaded and reviewed the user data script to install Apache and
    deploy a web app:

    -   Wget
        <https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/.../UserData.txt>

    -   Cat UserData.txt

```{=html}
<!-- -->
```
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

-   Monitored the instance status until it was running:

    -   Aws ec2 describe-instances --instance-ids \$INSTANCE --query
        'Reservations\[\].Instances\[\].State.Name' --output text

-   Tested the web server by retrieving its public DNS name:

    -   Aws ec2 describe-instances --instance-ids \$INSTANCE --query
        'Reservations\[\].Instances\[\].PublicDnsName' --output text

-   Opened the DNS in a browser and confirmed that the web page loaded
    successfully.

## Optional Challenges (If Completed)

-   **Challenge 1**: Diagnosed and corrected SSH connection issues with
    a misconfigured instance.

-   **Challenge 2**: Troubleshot and fixed web server installation
    issues on a faulty EC2 instance.

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
