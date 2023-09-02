# Aws-project-used-in-production-environment

#Overview

This project demonstrates how to create a VPC that you can use for servers in a production environment. To improve resiliency, you deploy the servers in two Availability Zones, by using an Auto Scaling group and an Application Load Balancer. For additional security, you deploy the servers in private subnets. The servers receive requests through the load balancer. The servers can connect to the internet by using a NAT gateway. To improve resiliency, you deploy the NAT gateway in both Availability Zones.


![vpc-example-private-subnets](https://github.com/Suresh-28/Aws-production-environment/assets/111943013/724828d3-d010-47d3-98cd-ef36f2cb933b)

The following diagram provides an overview of the resources included in this example. The VPC has public subnets and private subnets in two Availability Zones. Each public subnet contains a NAT gateway and a load balancer node. The servers run in the private subnets, are launched and terminated by using an Auto Scaling group, and receive traffic from the load balancer. The servers can connect to the internet by using the NAT gateway. The servers can connect to Amazon S3 by using a gateway VPC endpoint

#Routing
When you create this VPC by using the Amazon VPC console, we create a route table for the public subnets with local routes and routes to the internet gateway. We also create a route table for the private subnets with local routes, and routes to the NAT gateway, egress-only internet gateway, and gateway VPC endpoint.

#Create the VPC
Use the following procedure to create a VPC with a public subnet and a private subnet in two Availability Zones, and a NAT gateway in each Availability Zone.

#To create the VPC
- Open the Amazon VPC console at https://console.aws.amazon.com/vpc/.

- On the dashboard, choose Create VPC.

- For Resources to create, choose VPC and more.

#Configure the VPC

- For Name tag auto-generation, enter a name for the VPC.

- For IPv4 CIDR block, you can keep the default suggestion, or alternatively you can enter the CIDR block required by your application or network.

- If your application communicates by using IPv6 addresses, choose IPv6 CIDR block, Amazon-provided IPv6 CIDR block.

#Configure the subnets

- For Number of Availability Zones, choose 2, so that you can launch instances in multiple Availability Zones to improve resiliency.

- For Number of public subnets, choose 2.

- For Number of private subnets, choose 2.

You can keep the default CIDR block for the public subnet, or alternatively you can expand Customize subnet CIDR blocks and enter a CIDR block. For more information, see Subnet CIDR blocks.

- For NAT gateways, choose 1 per AZ to improve resiliency.

- If your application communicates by using IPv6 addresses, for Egress only internet gateway, choose Yes.

- For VPC endpoints, if your instances must access an S3 bucket, keep the S3 Gateway default. Otherwise, instances in your private subnet can't access Amazon S3. There is no cost for 
  this option, so you can keep the default if you might use an S3 bucket in the future. If you choose None, you can always add a gateway VPC endpoint later on.

- For DNS options, clear Enable DNS hostnames.

- Choose Create VPC.

#Deploy your application
  Ideally, you've finished testing your servers in a development or test environment, and created the scripts or images that you'll use to deploy your application in production.

  You can use Amazon EC2 Auto Scaling to deploy servers in multiple Availability Zones and maintain the minimum server capacity required by your application.

- To launch instances by using an Auto Scaling group
  
  Create a launch template to specify the configuration information needed to launch your EC2 instances by using Amazon EC2 Auto Scaling. For step-by-step directions, see Create a 
  launch template for your Auto Scaling group in the Amazon EC2 Auto Scaling User Guide.

 #To create a launch template

- Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

- On the navigation pane, under Instances, choose Launch Templates.

- Choose Create launch template. Enter a name and provide a description for the initial version of the launch template.

  (Optional) Under Auto Scaling guidance, select the check box to have Amazon EC2 provide guidance to help create a template to use with Amazon EC2 Auto Scaling.

  Under Launch template contents, fill out each required field and any optional fields as needed.

- Application and OS Images (Amazon Machine Image): (Required) Choose the ID of the AMI for your instances. You can search through all available AMIs, or select an AMI from the 
  Recents or Quick Start list. If you don't see the AMI that you need, choose Browse more AMIs to browse the full AMI catalog.

- To choose a custom AMI, you must first create your AMI from a customized instance. For more information, see Create an AMI in the Amazon EC2 User Guide for Linux Instances.

- For Instance type, choose a single instance type that's compatible with the AMI that you specified.

- Alternatively, to launch an Auto Scaling group with multiple instance types, choose Advanced, Specify instance type attributes, and then specify the following options:

- Number of vCPUs: Enter the minimum and maximum number of vCPUs. To indicate no limits, enter a minimum of 0, and keep the maximum blank.

- Amount of memory (MiB): Enter the minimum and maximum amount of memory, in MiB. To indicate no limits, enter a minimum of 0, and keep the maximum blank.

- Expand Optional instance type attributes and choose Add attribute to further limit the types of instances that can be used to fulfill your desired capacity. For information about 
  each attribute, see InstanceRequirementsRequest in the Amazon EC2 API Reference.

- Resulting instance types: You can view the instance types that match the specified compute requirements, such as vCPUs, memory, and storage.

- To exclude instance types, choose Add attribute. From the Attribute list, choose Excluded instance types. From the Attribute value list, select the instance types to exclude.

- For more information, see Create an Auto Scaling group using attribute-based instance type selection.

- Key pair (login): For Key pair name, choose an existing key pair, or choose Create new key pair to create a new one. For more information, see Amazon EC2 key pairs and Linux 
  instances in the Amazon EC2 User Guide for Linux Instances.

- Network settings: For Firewall (security groups), use one or more security groups, or keep this blank and configure one or more security groups as part of the network interface. For 
  more information, see Amazon EC2 security groups for Linux instances in the Amazon EC2 User Guide for Linux Instances.

- If you don't specify any security groups in your launch template, Amazon EC2 uses the default security group for the VPC(that we created above) that your Auto Scaling group will launch instances into. By 
  default, this security group doesn't allow inbound traffic from external networks. For more information, see Default security groups for your VPCs in the Amazon VPC User Guide.

 Do one of the following:

- Change the default network interface settings. For example, you can enable or disable the public IPv4 addressing feature, which overrides the auto-assign public IPv4 addresses 
  setting on the subnet. For more information, see Change the default network interface settings.

- Skip this step to keep the default network interface settings.

Do one of the following:

- Modify the storage configuration. For more information, see Modify the storage configuration.

- Skip this step to keep the default storage configuration.

- For Resource tags, specify tags by providing key and value combinations. If you specify instance tags in your launch template and then you choose to propagate your Auto Scaling 
  group's tags to its instances, all the tags are merged. If the same tag key is specified for a tag in your launch template and a tag in your Auto Scaling group, then the tag value 
  from the group takes precedence.

- (Optional) Configure advanced settings. For more information, see Configure advanced settings for your launch template.

- When you are ready to create the launch template, choose Create launch template.

#Note: To change the default network interface settings

- Under Network settings, expand Advanced network configuration.
  
- Choose Add network interface to configure the primary network interface, paying attention to the following fields:

- Auto-assign public IP: Change whether your network interface with a device index of 0 receives a public IPv4 address. By default, instances in a default subnet receive a public IPv4 
 address, while instances in a nondefault subnet do not. Select Disable to override the subnet's default setting.





#To create an Auto Scaling group, choose Create Auto Scaling group from the confirmation page.

- Create an Auto Scaling group, which is a collection of EC2 instances with a minimum, maximum, and desired size. For step-by-step directions, see Create an Auto Scaling group using a 
  launch template in the Amazon EC2 Auto Scaling User Guide.

- To create an Auto Scaling group using a launch template (console)
  Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/, and choose Auto Scaling Groups from the navigation pane.

- On the navigation bar at the top of the screen, choose the same AWS Region that you used when you created the launch template.

- Choose Create an Auto Scaling group.

- On the Choose launch template or configuration page, do the following:

- For Auto Scaling group name, enter a name for your Auto Scaling group.

- For Launch template, choose an existing launch template(that we have created above).

- For Launch template version, choose whether the Auto Scaling group uses the default, the latest, or a specific version of the launch template when scaling out.

- Verify that your launch template supports all of the options that you are planning to use, and then choose Next.

- On the Choose instance launch options page, under Network, for VPC, choose a VPC(that we have created above). The Auto Scaling group must be created in the same VPC as the security 
  group you specified in your launch template.

- For Availability Zones and subnets, choose only private subnets.

- If you created a launch template with an instance type specified, then you can continue to the next step to create an Auto Scaling group that uses the instance type in the launch 
  template.

- Alternatively, you can choose the Override launch template option if no instance type is specified in your launch template or if you want to use multiple instance types for auto 
  scaling. For more information, see Auto Scaling groups with multiple instance types and purchase options.

- Choose Next to continue to the next step.

- Or, you can accept the rest of the defaults, and choose Skip to review.

- (Optional) On the Configure advanced options page, configure the following options, and then choose Next:

- To register your Amazon EC2 instances with a load balancer, choose an existing load balancer or create a new one. For more information, see Use Elastic Load Balancing to distribute 
  traffic across the instances in your Auto Scaling group. To create a new load balancer, follow the procedure in Configure an Application Load Balancer or Network Load Balancer from 
  the Amazon EC2 Auto Scaling console.

- (Optional) For Health checks, Additional health check types, select Turn on Elastic Load Balancing health checks.

- (Optional) For Health check grace period, enter the amount of time, in seconds. This is how long Amazon EC2 Auto Scaling needs to wait before checking the health status of an i 
  instance after it enters the InService state. For more information, see Set the health check grace period for an Auto Scaling group.

  Under Additional settings, Monitoring, choose whether to enable CloudWatch group metrics collection. These metrics provide measurements that can be indicators of a potential issue, 
  such as number of terminating instances or number of pending instances. For more information, see Monitor CloudWatch metrics for your Auto Scaling groups and instances.

  For Enable default instance warmup, select this option and choose the warm-up time for your application. If you are creating an Auto Scaling group that has a scaling policy, the 
  default instance warmup feature improves the Amazon CloudWatch metrics used for dynamic scaling. For more information, see Set the default instance warmup for an Auto Scaling group.

- On the Configure group size and scaling policies page, configure the following options, and then choose Next:

- For Desired capacity, select 2, for minimum capacity select 1, for maximum capacity select 4

- To automatically scale the size of the Auto Scaling group, choose Target tracking scaling policy and follow the directions. For more information, see Target tracking scaling 
  policies for Amazon EC2 Auto Scaling.

- Under Instance scale-in protection, choose whether to enable instance scale-in protection. For more information, see Use instance scale-in protection.

- (Optional) To receive notifications, for Add notification, configure the notification, and then choose Next. For more information, see Get Amazon SNS notifications when your Auto 
  Scaling group scales.

- (Optional) To add tags, choose Add tag, provide a tag key and value for each tag, and then choose Next. For more information, see Tag Auto Scaling groups and instances.

- On the Review page, choose Create Auto Scaling group.




Create a load balancer, which distributes traffic evenly across the instances in your Auto Scaling group, and attach the load balancer to your Auto Scaling group. For more information, see the Elastic Load Balancing User Guide and Use Elastic Load Balancing in the Amazon EC2 Auto Scaling User Guide.

Step 1: Configure your target group
To configure your target group
Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

- In the navigation pane, under Load Balancing, choose Target Groups.

- Choose Create target group.

- Under Basic configuration, keep the Target type as instance.

- For Target group name, enter a name for the new target group.

- Keep the default protocol (HTTP) and port (80).

- Select the VPC containing your instances. Keep the protocol version as HTTP1.

- For Health checks, keep the default settings.

- Choose Next.

Step 2: Choose a load balancer type

- To create a Application Load Balancer
- Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

- On the navigation bar, choose a Region for your load balancer. Be sure to choose the same Region that you used for your EC2 instances.

- In the navigation pane, under Load Balancing, choose Load Balancers.

- Choose Create Load Balancer.

- For Application Load Balancer, choose Create.

- On the Register targets page, complete the following steps. This is an optional step for creating the load balancer. However, you must register this target if you want to test your 
  load balancer and ensure that it is routing traffic to this target.

- For Available instances, select one or more instances.

- Keep the default port 80, and choose Include as pending below.

- Choose Create target group.


#Create a Bastion host

Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

Choose Launch Instance.

- Step 1: Choose an Amazon Machine Image (AMI), find an Amazon Linux 2 AMI at the top of the list and choose Select.

- Step 2: Choose an Instance Type, choose Next: Configure Instance Details.

- Step 3: Configure Instance Details, provide the following information:
  
- Choose Next: Add Storage.

- Choose Next: Add Tags.

- Name your instance and choose Next: Configure Security Group.

- In Step 4: Configure Security Group, set Assign a security group to Select an existing security group. Choose the default security group to make sure that it can access your EFS 
  file system.

- Choose Review and Launch.

- Choose Launch.

#To launch the private virtual machine in public virtual machine

- Copy the public ip adress of bostion host.
- open local terminal use shh and keypair to connect.
- once connection is successfull
- use ssh 


Test your configuration
After you've finished deploying your application, you can test it. If your application can't send or receive the traffic that you expect, you can use Reachability Analyzer to help you troubleshoot. For example, Reachability Analyzer can identify configuration issues with your route tables or security groups.
