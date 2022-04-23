# session-manager


**Going Bastion-less: Accessing Private EC2 instance with Session Manager**

**Why use Session Manager?**

It is well known that we can not directly connect to a private EC2 instance unless there is VPN Connectivity or Direct Connect or other network connectivity source with the VPC. A common approach to connect to an EC2 instance on a private subnet of your VPC is to use a Bastion Host.
A Bastion Host is a server whose purpose is to provide access to a private network from an external network (such as the Internet). Because of its exposure to potential attacks, a bastion host must minimize the chances of penetrations. When using a bastion host, you log into the bastion host first, and then into your target private instance. With this approach, only the bastion host will have an external IP address.

However, there are some drawbacks:

    You will need to allow SSH inbound rule at your bastion
    You need to open ports on your private EC2 instance in order to connect it to your bastion
    You will need to manage the SSH key credentials of your users: You will need to generate an ssh key pair for each user or get a copy of the same SSH key for your users
    Cost: The bastion host also has a cost associated with it as it is a running EC2 instance. Even a t2.micro costs about $10/month.


Session Manager can be used to access instances within private subnets that allow no ingress from the internet. AWS SSM provides the ability to establish a shell on your systems through its native service, or by using it as a tunnel for other protocols, such as Secure Shell (SSH). 

Advantages:

 - It will log the commands issued during the session, as well as the results. You can save the logs in s3 if you wish.
 - Shell access is completely contained within Identity and Access Management (IAM) policies, you won’t need to manage SSH keys
 - The user does not need to use a bastion host and Public IPs.
 - No need to open the ports in the security groups
