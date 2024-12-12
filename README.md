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
#Create VPC
Navigate to VPC dashboard and click “Create VPC”. Continue by naming your VPC, then add the IPv4 CIDR block. We will be using 10.10.0.0/16, as seen below. You can now “Create VPC”.


