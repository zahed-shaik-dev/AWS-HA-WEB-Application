# AWS High-Availability Web Application Deployment

## Overview

This project demonstrates the design and configuration of a highly available web application infrastructure on AWS. The architecture uses an Application Load Balancer, Amazon EC2, and an Auto Scaling Group distributed across two Availability Zones.

The infrastructure was configured through the AWS Management Console, with a focus on network architecture, access control, automated instance configuration, and application health-check troubleshooting.

## Architecture


## Architecture

<img width="1536" height="1024" alt="AWS-HA-Application" src="https://github.com/user-attachments/assets/8a754922-5a8d-4bb0-9fc6-dbaf19f47c0e" />


See [Architecture Documentation](docs/architecture.md) for implementation details.


For the detailed architecture documentation, see [Architecture Documentation](docs/architecture.md).


### Architecture Components

- **Amazon VPC:** Custom network using CIDR `10.0.0.0/16`.
- **Public Subnets:** Host public-facing infrastructure across two Availability Zones.
- **Private Subnets:** Intended to host application instances without direct public access.
- **Internet Gateway:** Provides internet connectivity for public resources.
- **NAT Gateway:** Enables outbound internet access from appropriately routed private subnets.
- **Application Load Balancer:** Distributes incoming HTTP traffic to registered targets.
- **EC2 Launch Template:** Defines instance configuration using Amazon Linux 2023 and user data for Nginx installation.
- **Auto Scaling Group:** Configured with a desired capacity of two instances.
- **Target Group:** Uses HTTP health checks to monitor registered targets.
- **IAM and Security Groups:** Control AWS permissions and network traffic.

## AWS Services Used

Amazon VPC, Amazon EC2, Application Load Balancer, Auto Scaling, IAM, Internet Gateway, NAT Gateway, and Security Groups.

## Implementation Steps

1. Created a VPC with public and private subnets across two Availability Zones.
2. Configured route tables and internet connectivity.
3. Created security groups for the load balancer and EC2 instances.
4. Created an EC2 launch template with user data to install Nginx.
5. Configured an Application Load Balancer and target group.
6. Created an Auto Scaling Group across private subnets.
7. Investigated target health-check timeouts during validation.

## Screenshots

Add screenshots of your actual AWS environment here as they become available.

Recommended evidence includes:
- VPC and subnet configuration
- Application Load Balancer listeners and target health
- EC2 instances across Availability Zones
- Auto Scaling Group configuration
- Successful application response through the load balancer

## Troubleshooting

During the last recorded validation, the target group reported unhealthy targets due to health-check timeouts. Further investigation is required to confirm successful application connectivity through the load balancer.

## Key Learnings

- AWS VPC and subnet design
- Public and private routing
- Load balancing and Auto Scaling
- EC2 user-data automation
- IAM roles and security-group configuration
- Application health checks and troubleshooting

## Security and Cost Considerations

- Restrict inbound traffic using security groups and least-privilege access.
- Do not commit AWS credentials, private keys, or sensitive Terraform state.
- Review active AWS resources and delete unused resources to avoid unnecessary charges.
- Verify target health and end-to-end application connectivity before describing the deployment as fully operational.

## Project Status

**Status:** Infrastructure deployment completed as a hands-on learning exercise; AWS resources have since been deleted.

This project demonstrates the design of a highly available web application architecture on AWS, including networking, load balancing, compute scaling, and security fundamentals.

### Validation Status

- [ ] Project documentation added
- [ ] AWS architecture diagram included
- [ ] Security documentation added
- [ ] Testing and validation plan documented
- [ ] GitHub Actions workflow added for Markdown validation
- [ ] Live application health checks
- [ ] Load balancer and target health verification
- [ ] Auto Scaling behavior verification
- [ ] Monitoring and alert validation

*Live infrastructure checks are not currently performed because the AWS resources have been deleted.*

### Documentation

- [Architecture](docs/architecture.md)
- [Security](docs/security.md)
- [Testing and Validation](docs/testing-and-validation.md)

### Continuous Integration

A GitHub Actions workflow checks Markdown files on pushes and pull requests targeting the `main` branch.
