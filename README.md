# session-manager


**Going Bastion-less: Accessing Private EC2 instance with Session Manager**

**Why use Session Manager?**

It is well known that we can not directly connect to a private EC2 instance unless there is VPN Connectivity or Direct Connect or other network connectivity source with the VPC. A common approach to connect to an EC2 instance on a private subnet of your VPC is to use a Bastion Host.

A Bastion Host is a server whose purpose is to provide access to a private network from an external network (such as the Internet). Because of its exposure to potential attacks, a bastion host must minimize the chances of penetrations. When using a bastion host, you log into the bastion host first, and then into your target private instance. With this approach, only the bastion host will have an external IP address.

However, there are some drawbacks:

 - You will need to allow SSH inbound rule at your bastion
 - You need to open ports on your private EC2 instance in order to connect it to your bastion
 - You will need to manage the SSH key credentials of your users: You will need to generate an ssh key pair for each user or get a copy of the same SSH key for your users
 - Cost: The bastion host also has a cost associated with it as it is a running EC2 instance. Even a t2.micro costs about $10/month.


Session Manager can be used to access instances within private subnets that allow no ingress from the internet. AWS SSM provides the ability to establish a shell on your systems through its native service, or by using it as a tunnel for other protocols, such as Secure Shell (SSH). 

Advantages:

 - It will log the commands issued during the session, as well as the results. You can save the logs in s3 if you wish.
 - Shell access is completely contained within Identity and Access Management (IAM) policies, you won’t need to manage SSH keys
 - The user does not need to use a bastion host and Public IPs.
 - No need to open the ports in the security groups

Network configuration used:

![image](https://user-images.githubusercontent.com/74225291/164887297-cbe60bbd-561f-4c58-9679-c181307f9aa4.png)

For SSM manager to send its logs to S3 bucket and Cloudwatch logs. We will create S3 bukcer (ssm-demo) and cloudwatch loggroup (ssmdemo). 
Now goto session manager and from Preferneces enable s3 logging and cloudwatch log.

S3 bucket:

<img width="1166" alt="image" src="https://user-images.githubusercontent.com/74225291/164887506-47624f65-b00b-432c-a2fa-76d574917c9c.png">

Cloudwatch Log:

<img width="1220" alt="image" src="https://user-images.githubusercontent.com/74225291/164887521-be78716f-7e8e-488f-90fe-c1e1e8c36c92.png">

Session manager Sessiongs:

<img width="1237" alt="image" src="https://user-images.githubusercontent.com/74225291/164887423-fbd13dbf-783a-48f9-a59e-800db7014745.png">
<img width="1172" alt="image" src="https://user-images.githubusercontent.com/74225291/164887473-ad23e812-3391-411a-a993-3682fcf85499.png">

<img width="1237" alt="image" src="https://user-images.githubusercontent.com/74225291/164887423-fbd13dbf-783a-48f9-a59e-800db7014745.png">

Now to be able to access the EC2 instnace using session manager create ssm-role and attached AmazonSSMManagedInstanceCore, AmazonS3FullAccess and CloudWatchLogsFullAccess.

<img width="1177" alt="image" src="https://user-images.githubusercontent.com/74225291/164887602-d32346b6-7e05-49fc-80b6-18d59deaaa35.png">

Now cerate a EC2 instance in private subnet and attached ssm-role to it.

<img width="1267" alt="image" src="https://user-images.githubusercontent.com/74225291/164888867-925fe46f-c39f-4c4d-9c37-9945b8850bd2.png">

Connect to instance using session manager:

<img width="965" alt="image" src="https://user-images.githubusercontent.com/74225291/164888886-fca33ebb-0af2-46a8-8412-16714ba5316b.png">


<img width="1449" alt="image" src="https://user-images.githubusercontent.com/74225291/164888830-f128f836-76a8-41d0-ae0b-306f3c9af827.png">


Session logs uplocaded in s3 bucket:

<img width="1243" alt="image" src="https://user-images.githubusercontent.com/74225291/164888763-9e6c48c6-74d0-4460-bf94-9bb3db7a75cf.png">

Session logs uploaded in cloudwatch log:

<img width="1232" alt="image" src="https://user-images.githubusercontent.com/74225291/164888802-e6d0d782-894e-46ad-a651-f1bb15846b5f.png">


