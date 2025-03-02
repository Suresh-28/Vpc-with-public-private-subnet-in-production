# Aws-project-used-in-production-environment

#Overview

This project demonstrates how to create a VPC that you can use for servers in a production environment. To improve resiliency, you deploy the servers in two Availability Zones, by using an Auto Scaling group and an Application Load Balancer. For additional security, you deploy the servers in private subnets. The servers receive requests through the load balancer. The servers can connect to the internet by using a NAT gateway. To improve resiliency, you deploy the NAT gateway in both Availability Zones.


![vpc-example-private-subnets](https://github.com/Suresh-28/Aws-production-environment/assets/111943013/724828d3-d010-47d3-98cd-ef36f2cb933b)

The following diagram provides an overview of the resources included in this example. The VPC has public subnets and private subnets in two Availability Zones. Each public subnet contains a NAT gateway and a load balancer node. The servers run in the private subnets, are launched and terminated by using an Auto Scaling group, and receive traffic from the load balancer. The servers can connect to the internet by using the NAT gateway. The servers can connect to Amazon S3 by using a gateway VPC endpoint


# Create the VPC
Use the following procedure to create a VPC with a public subnet and a private subnet in two Availability Zones, and a NAT gateway in each Availability Zone. [use documentation](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/gsg_create_vpc.html)

# Create Auto Scaling group
  
  Create a launch template to specify the configuration information needed to launch your EC2 instances by using Amazon EC2 Auto Scaling. For step-by-step directions, see Create a 
  launch template for your Auto Scaling group in the Amazon EC2 Auto Scaling User Guide.[use documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-auto-scaling-groups-launch-template.html)

# Create a launch template
- Auto-assign public IP: Change whether your network interface with a device index of 0 receives a public IPv4 address. By default, instances in a default subnet receive a public IPv4 
 address, while instances in a nondefault subnet do not. Select --Disable-- to override the subnet's default setting.

- For Launch template, choose an existing launch template(that we have created above).

- On the Choose instance launch options page, under Network, for VPC, choose a VPC(that we have created above). The Auto Scaling group must be created in the same VPC as the security 
  group you specified in your launch template.

- For Availability Zones and subnets, choose only private subnets.
- On the Configure group size and scaling policies page, configure the following options, and then choose Next:

- For Desired capacity, select 2, for minimum capacity select 1, for maximum capacity select 4.[use documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-asg-launch-template.html)

# Create Loadbalancer
Create a load balancer, which distributes traffic evenly across the instances in your Auto Scaling group, and attach the load balancer to your Auto Scaling group. For more information, see the Elastic Load Balancing User Guide and Use Elastic Load Balancing in the Amazon EC2 Auto Scaling User Guide.[use documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-application-load-balancer.html)

# Create target group
Target groups route requests to one or more registered targets, such as EC2 instances, using the protocol and port number that you specify.[use documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-target-group.html)

# Create a Bastion host(EC2 instance)
A bastion host is a server whose purpose is to provide access to a private network from an external network, such as the Internet.[use documentation](https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html)

# To launch the private virtual machine in public virtual machine
- Copy the public ip adress of bostion host.
- use scp -i <pemfile path> <pemfile path> ubuntu@<ip>:/home/ubuntu
- open local terminal use shh and keypair to connect.
- once connection is successfull
- copy the private ip address of one ec2 instance
- use ssh -i file.pem ubuntu@<privateIP>
- create a html page vim index.html
- run it python3 -m http.server:8000
- Goto the load balancer
- 
AWS Production Environment Deployment
☁ AWS Production Environment Deployment (EC2, VPC, Load Balancer, Security):
Designed, implemented, and managed a secure and scalable production environment using AWS cloud services. Configured key AWS infrastructure components, including EC2 instances, Virtual Private Cloud (VPC), Load Balancers, IAM policies, ensuring high availability, security, and performance.

🔹 Infrastructure Setup & EC2 Configuration:
- Deployed AWS EC2 instances (Linux/Windows) for hosting applications with optimized CPU, memory, and storage configurations.
- Configured*Elastic Block Store (EBS) volumes for persistent data storage and attached them to EC2 instances.
- Automated EC2 instance creation using AWS Auto Scaling Groups to dynamically adjust resources based on demand.

🔹 Virtual Private Cloud (VPC) & Networking:
- Designed a highly available and secure VPC architecture with public and private subnets.
- Configured Internet Gateway (IGW) and NAT Gateway for controlled internet access in private subnets.
- Implemented VPC Peering and AWS Transit Gateway to enable secure cross-VPC communication.
- Optimized network performance using AWS Route 53 for DNS management and CloudFront CDN for faster content delivery.

🔹 Load Balancing & High Availability:
- Configured an Elastic Load Balancer (ELB) (Application Load Balancer) to distribute incoming traffic across multiple EC2 instances.
- Implemented AWS Auto Scaling policies to handle sudden traffic surges and maintain uptime.
- Utilized AWS RDS Multi-AZ deployment for database redundancy and failover support.

🔹 Security & Compliance:
- Enforced IAM roles and policies to provide least-privilege access control for resources.
- Configured Security Groups and Network ACLs to restrict unauthorized access.
- Implemented AWS WAF (Web Application Firewall) to protect against common web exploits and threats.

🔹 Outcome & Impact:
- Ensured 99.99% uptime through auto-scaling, load balancing, and high-availability architectures.
- Reduced infrastructure costs by 30% using reserved instances and cost-optimized storage solutions.
- Enhanced security posture, achieving compliance with industry best practices (ISO 27001, SOC 2, GDPR).

- copy the dns name and browse it, tada.... we have successfully launch a web page in production lelvel.




