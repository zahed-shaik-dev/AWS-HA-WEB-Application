\# AWS High-Availability Web Application Deployment



\## Overview



This project demonstrates how to build a highly available web application infrastructure on AWS using a load balancer, EC2 instances, and Auto Scaling across two Availability Zones.



The infrastructure was configured through the AWS Management Console, with a focus on networking, access control, automated instance setup, and health-check troubleshooting.



\## Architecture



\- \*\*VPC:\*\* Custom network with CIDR `10.0.0.0/16`

\- \*\*Public subnets:\*\* Two subnets across Availability Zones

\- \*\*Private subnets:\*\* Two subnets for application instances

\- \*\*Internet Gateway:\*\* Provides internet connectivity for public resources

\- \*\*NAT Gateway:\*\* Allows outbound internet access from private subnets

\- \*\*Application Load Balancer:\*\* Distributes incoming HTTP traffic

\- \*\*EC2 Launch Template:\*\* Uses Amazon Linux 2023 and user data to install Nginx

\- \*\*Auto Scaling Group:\*\* Configured with a desired capacity of two instances

\- \*\*Target Group:\*\* Configured with HTTP health checks

\- \*\*IAM and Security Groups:\*\* Control instance permissions and network access



## Architecture

See the [AWS architecture diagram](docs/architecture.md) for the VPC,
public and private subnets, load balancer, Auto Scaling Group,
EC2 instances, and NAT Gateway.

\## AWS Services Used



Amazon VPC, EC2, Application Load Balancer, Auto Scaling, IAM, Internet Gateway, NAT Gateway, and Security Groups.



\## Implementation Steps



1\. Created a VPC with public and private subnets across two Availability Zones.

2\. Configured route tables and internet connectivity.

3\. Created security groups for the load balancer and EC2 instances.

4\. Created an EC2 launch template with user data for Nginx installation.

5\. Configured an Application Load Balancer and target group.

6\. Created an Auto Scaling Group across private subnets.

7\. Investigated target health-check timeouts during validation.



\## Screenshots



AWS Console screenshots will be added here as the project documentation is completed.



\## Troubleshooting



During the last recorded validation, the target group reported unhealthy targets due to health-check timeouts. Further investigation is required to confirm successful application connectivity through the load balancer.



\## Key Learnings



\- AWS VPC and subnet design

\- Public and private routing

\- Load balancing and Auto Scaling concepts

\- EC2 user-data automation

\- IAM roles and security-group configuration

\- Application health

