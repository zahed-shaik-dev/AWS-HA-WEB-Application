
# Deployment Guide

## Prerequisites

- An AWS account
- Access to the AWS Management Console
- AWS region: Mumbai (`ap-south-1`)
- Basic knowledge of VPC networking and EC2

## Deployment Steps

### 1. Create the VPC

Create a VPC named `ha-webapp-vpc` using CIDR `10.0.0.0/16`.

### 2. Create Subnets

Create two public and two private subnets across two Availability Zones.

### 3. Configure Networking

- Attach an Internet Gateway.
- Configure the public route table.
- Create a NAT Gateway in a public subnet.
- Configure private-subnet outbound routing through the NAT Gateway.

### 4. Configure Security

Create separate security groups for the ALB and EC2 instances. Permit HTTP traffic to EC2 only from the ALB security group.

### 5. Configure EC2

Create an IAM role, then create a launch template using Amazon Linux 2023. Configure user data to install and start Nginx.

### 6. Configure the Load Balancer

Create a target group using HTTP port 80 and health-check path `/`. Create an internet-facing Application Load Balancer in both public subnets.

### 7. Configure Auto Scaling

Create an Auto Scaling Group using the launch template and both private subnets. Attach the target group and configure the desired, minimum, and maximum capacity.

### 8. Validate the Deployment

- Check EC2 instance status.
- Check target group health.
- Inspect Nginx startup and system logs if health checks fail.
- Access the application through the ALB DNS name after targets become healthy.
- Verify the Auto Scaling configuration.

## Cleanup

Delete unused project resources when finished. Check dependencies before deletion and confirm that no resources are still required. NAT Gateways, load balancers, and EC2 instances can incur charges.
