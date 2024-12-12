# AWS-Auto-Scaling
## Create Auto Scaling group of EC2 Instance for High Availability 
![image alt](https://github.com/Tatenda-Prince/AWS-Auto-Scaling/blob/8aa726fa1043c54f875b8fcf3ba8131f0875eb64/ASG%20image.png)

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

![image alt]()




