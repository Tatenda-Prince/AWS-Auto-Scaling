# AWS-Auto-Scaling
## Create Auto Scaling group of EC2 Instance for High Availability 
![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/8aa726fa1043c54f875b8fcf3ba8131f0875eb64/ASG%20image.png)

#Intro
Together, we will “Scale the Clouds” by following the steps to creating an Auto Scaling Group (ASG) of Amazon EC2 Instances for high availability. We will literally be building a cloud infrastructure from ground up, so expect the use of many different AWS resources.

## Background 
# Virtual Private Cloud (VPC)
Virtual Private Cloud is a service on AWS that offers a virtual private network, logically isolated from other virtual networks on the platform.

# Subnet
Subnets are a set of IP address ranges in your VPC.

# Internet Gateway
Internet Gateways enable AWS resources in public subnets with public IP addresses, to be able to connect to the internet.

# Elastic Load Balancers
Elastic Load Balancers automatically distribute incoming traffic across multiple targets such as Amazon EC2 Instances.

# Route Table
A Route Table contains a set of rules, called routes, that determine the direction of network traffic from subnets or an internet gateway.

# Launch Template 
Launch Templates contain pre-configuration information used to launch resources such as Amazon EC2 Instance.

# High Availability
High availability refers to the elimination of single points of failure to enable infrastructures to continue operating, even if a component it depends on fails.

## Use Case 
You are a Cloud Engineer for Tatenda Tech, tasked to build a highly available Apache Web Servers cloud infrastructure. The infrastructure needs to have a minimum of 2 servers running at all times. Additionally, it must have the capability to scale out and increase to a maximum of 5 servers when the average CPU utilization increases to 50%.

## Step 1: Setup VPC and subnets
Navigate to VPC dashboard and click “Create VPC”. Continue by naming your VPC, then add the IPv4 CIDR block. We will be using 10.10.0.0/16, as seen below. You can now “Create VPC”.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/092fe7f219f885431c71d04e3895077f9e4fe832/Screenshot%202024-11-11%20183616.png)

## Create Subnets
Navigate to “Subnets” in the left pane of the VPC dashboard. Click “Create subnets”, then select our newly created VPC from the list. Name your subnet, then choose the Availability Zone (AZ) and assigned IPv4 CIDR block. For the first subnet, we will use AZ us-east-1a and CIDR block 10.10.1.0/24. For the second subnet, use AZ us-east-1b and CIDR block 10.10.2.0/24. Lastly, for subnet three, use AZ us-east-1c and CIDR block 10.10.3.0/24.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/6351f7adc17cbf49304aaa08608f5cc0d3500a61/Screenshot%202024-11-11%20183848.png)

After configuring all three subnets, click “Create subnets”. You should be able to see all 3 subnets created and “Available”, as seen below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/894ae903d2c93520df67a12055a33a708089cf15/Screenshot%202024-11-11%20184149.png)

We need to enable each subnet to automatically receive a public IP address so we can send and receive traffic over the internet. To accomplish this, select one of your subnets, then navigate to “Actions” and click “Edit subnet settings”. Enable “Auto-assign public address”, then save the changes. Make sure to do this with all 3 subnets.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/d3b8ab56dfbcd659cf09db95cb2b6e12d6735547/Screenshot%202024-11-11%20184223.png)

Now that we’ve created and configured our VPC and 3 subnets, we can proceed to Step 2 — Internet Gateway and Route Tables!

## Step 2: launch Internet gateway and configure route tables

Create Internet Gateway and attach to VPC
Navigate to your VPC dashboard, select “Internet Gateway”, then “Create a new gateway”. Name the Internet Gateway and proceed to “Create internet gateway”.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/f2ef534710b604cadff424962bfc99cd255d849a/Screenshot%202024-11-11%20184410.png)

On the Internet Gateway dashboard, select your Internet Gateway, click “Actions”, then “Attach to VPC”. Select your VPC, then attach the Internet Gateway, a shown below.

![images alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/1109475b29cf0dcfb322972755f6f75a603f37a6/Screenshot%202024-11-11%20184437.png)

Your Internet Gateway’s state should now read “Attached” with the VPC ID of our previously created VPC, as shown below

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/8d5970df94d42f8df4f2e2243ed4c2af9dd09153/Screenshot%202024-11-11%20184455.png)

Configure route tables
Navigate to “Route tables” and “Create route table”. Name the route table, choose the VPC, then “Create route table”.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/5edcba747c22eb2f4d9017e9e0a32c959a135a21/Screenshot%202024-11-11%20184557.png)

In your Route table dashboard, select the “Subnet associations” tab, then edit the explicit subnet associations —

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/611fc04a701cce3ccfd2782cae8a752cebb246b6/Screenshot%202024-11-11%20184803.png)

Add all three subnets to the associations, then save the associations.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/46daab140a8fdcf8011cfaa1cc90c22d99c8e1fb/Screenshot%202024-11-11%20184823.png)

Let’s now edit the route table to have access to the internet though our Internet Gateway. Click on the “Routes” tab, “Edit routes”, then “Add route”. We will add a route that will direct all other addresses destined outside our network, to our newly created Internet Gateway through to the internet, as seen below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/c86f8448c0dc7770e59c772af5ae4e6d6ba549cf/Screenshot%202024-11-11%20185006.png)

We now have an Internet Gateway and a configured Route Table. Let’s proceed to Step 3— Application Load Balancer!

## Step 3: Launch a configure application load balancer
Configure network mappings
Head over to your EC2 dashboard, navigate to the left side, then scroll down. Select “Load Balancers”, then “Create load balancer”.

We will choose to create an Application Load Balancer. Name the Load Balancer and keep the rest of the default basic configurations. Continue on to Network mappings, choose your VPC then select the three AZ’s with your three subnets, as shown below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/e9e3b6108215bb1f1ab0cf35cb209e7814da200d/Screenshot%202024-11-11%20185401.png)

Create security group
Proceed to the Security group setting, then create a new security group. Name your security group and make sure your previously created VPC is selected. Add an inbound rule to allow HTTP traffic from Anywhere (0.0.0.0/0), as seem below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/6ad05e90205b8a92dd4078ca16a436a65f6c1ab3/Screenshot%202024-11-11%20190035.png)

Additionally, add another rule of type “SSH” with source from “Anywhere”. Note — this is security risk, however for this article we will allow it for demonstration sake.

We can now proceed to “Create security group”.

Select the newly created security group from the list, as seen below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/84dcb5512d68d9e474faf1614a5fae617fdf8596/Screenshot%202024-11-11%20190102.png)

Continue to “Listeners and routing”. Make sure the listener Protocol is set to HTTP and the Port to 80. We will now “Create target group” to be configured for our EC2 Instances. Choose “Instances” for a target type and name your target group. Choose HTTP for the protocol and 80 for the Port. Select our new VPC, click “Next”, then “Create target group”.

![iamge alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/9c5aae88c99d27feb093e81c6d1ce51d8c4f6307/Screenshot%202024-11-11%20190213.png)

We can now select our newly created target group, as shown below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/cc41554fefb1dd6c17a33dcf646f122b10340807/Screenshot%202024-11-11%20190323.png)

Scroll to the bottom, review the summary then “Create load balancer”. Now let’s continue to Step 4 — Launch Template!

## Step 4: Create Launch Template
Configure Launch Template
Navigate to your EC2 dashboard, on the left, click “Launch Templates”, then “Create launch template”.

Name the launch template, select the Amazon Linux AMI that’s part of AWS free tier by clicking the “Quick Start” tab, then choosing the AMI. Choose the “t2.micro” instance type, as shown below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/fa9cb9cd99ee99444b846628731c93eb542d24ba/Screenshot%202024-11-11%20190714.png)

Proceed to create a new key pair to enable you to authenticate via SSH into our EC2 Instances. Save the key pair “.pm” file to your local machine in a safe location. Remember, you can also use a previously created key pair.

Continue to “Network settings” and select our previously created load balancer security group, as shown below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/acadfcd4bb7451fa148f982043abf1452855c201/Screenshot%202024-11-11%20190849.png)

Bootstrap EC2 Instance to launch with Apache Web Server
Continue to the “Advanced details”. Expand the settings, then scroll down to add the user data to install, start and enable the Apache Web Server. We will also install other software to enable us to stress test an EC2 Instance.

Copy the bash script below, paste it in the user data field, then “create launch template” 

## Step 5: Create an Autoscaling Group (ASG)
Configure new ASG launch options
After creating the launch template, in the following window, click “Create Auto Scaling group”, as show below. Scroll down, select “Auto scaling groups” in the left pane, then “Create auto scaling group”.

![iamge alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/4013148a7cbbadb46afb7680659b5a7d6c1f73ad/Screenshot%202024-11-11%20191112.png)

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/1dca7164cefa10265f958ef90f0f33bd538bfe0a/Screenshot%202024-11-11%20191244.png)

Name the Auto scaling group, select the launch template previously created, then click “Next”.

Select our previously created VPC, add the three of our subnets, as seen below, then click “Next”.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/93d59e1d91243c4a979c92b57862c4a1c3023137/Screenshot%202024-11-11%20191602.png)

Configure load balancing options
Select “Attached to and exiting load balancer”, then choose our previously created load balancer’s target group, as shown below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/237052db629fcbf1bfc2c6eaf23d63a5a128a723/Screenshot%202024-11-11%20191707.png)

Proceed to health checks and change the grace period to 120 seconds (2 minutes), as seen below. With health checks enabled, the load balancer is able to periodically send request to check the status of our EC2 Instances and determine whether an instance is capable of performing work successfully.

In additional settings, “enable group metric collection within CloudWatch”.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/8356e53dbee210a28b335fe0fe8c41496e74277d/Screenshot%202024-11-11%20191831.png)

Configure ASG Group size and CloudWatch Monitoring
As our use case states, we will set our desired and minimum capacity to 2. Our maximum capacity will be set to 5. Select “Target scaling policy” and make sure the metric type is “Average CPU utilization” and “Target value” is set to 50, then click “Next”.

![images alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/c8f24a708f7826e4249b12e41c0e38e750947a31/Screenshot%202024-11-11%20191959.png)

Continue until you get to the review page. Review the final configurations, then “Create auto scaling group”. You should be able to see your ASG with a status of “updating capacity..” as it launches your EC2 Instances according to the pre-set configurations.

Navigate to your EC2 dashboard and verify that there are two running EC2 Instances created by your ASG.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/b2b2c96631696eb9fdba45554b0ef208bb080143/Screenshot%202024-11-11%20192447.png)

## Step 6: Connect to Servers running Apache Web Server
Retrieve the public IP Address of each of the EC2 Instance from the Amazon EC2 dashboard “Networking” tab, copy and paste it in the address bar of your preferred browser. Your browser should display the Apache Web Server default Webpage, as seen below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/51b7786d25233a0fead6b71a6e033f5963f4723b/Screenshot%202024-11-12%20192719.png)

You should also be able to connect to one of your instances through the application load balancer’s domain name. You can retrieve this by navigating to your previously created load balancer, copying the DNS name, then pasting it in your browser’s address bar, as seen below.

![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/157cadbdd3f2e87d7870be7db0d4d2b7b3132740/Screenshot%202024-11-12%20192819.png)

Congratulations!
You’ve just successfully completed “Scaling The Clouds”. You’ve created an Auto Scaling Group of EC2 Web Servers for high availability!


















