
# AWS High-Availability Web Application Architecture

## 1. Overview

This project uses AWS networking, load balancing, and EC2 Auto Scaling components to build a web application infrastructure across two Availability Zones.

## 2. Network Design

| Resource | Configuration |
|---|---|
| VPC | `10.0.0.0/16` |
| Public subnet A | `10.0.1.0/24` |
| Public subnet B | `10.0.2.0/24` |
| Private subnet A | `10.0.11.0/24` |
| Private subnet B | `10.0.12.0/24` |

## 3. AWS Components

- **VPC:** Isolated network for project resources.
- **Public subnets:** Host the internet-facing load balancer and NAT Gateway.
- **Private subnets:** Host application EC2 instances.
- **Internet Gateway:** Enables internet connectivity for public resources.
- **NAT Gateway:** Provides outbound internet access for private instances.
- **Application Load Balancer:** Routes HTTP requests to registered targets.
- **Auto Scaling Group:** Maintains the configured EC2 capacity.
- **Launch Template:** Defines the EC2 instance configuration.
- **IAM role:** Provides permissions for Systems Manager access.
- **Security groups:** Restrict network traffic.

## 4. Traffic Flow

1. A user sends an HTTP request to the Application Load Balancer.
2. The ALB forwards requests to registered EC2 targets.
3. Nginx on an EC2 instance serves the sample web page.
4. Private instances use the NAT Gateway for outbound connectivity when needed.

## 5. Security

- ALB security group permits inbound HTTP traffic.
- EC2 security group permits HTTP traffic from the ALB security group.
- Application instances are configured in private subnets without public IP addresses.
- EC2 instances use an IAM role instead of embedded credentials.

## 6. Availability Considerations

The ALB and Auto Scaling Group are configured to use subnets in two Availability Zones. The project uses a single NAT Gateway to control costs; this introduces an outbound-connectivity and Availability Zone dependency.

## 7. Validation

The target group previously reported health-check timeouts. Confirm target health and application accessibility before claiming that the deployment is fully operational.


# AWS High-Availability Web Application Architecture

```mermaid
flowchart TB
    U([Internet Users])
    IGW[Internet Gateway]
    ALB[Application Load Balancer<br/>HTTP :80]
    TG[Target Group<br/>HTTP :80 · Health Check /]

    subgraph VPC["AWS VPC — 10.0.0.0/16"]
        subgraph AZA["Availability Zone A"]
            PA[Public Subnet<br/>10.0.1.0/24]
            PVA[Private Subnet<br/>10.0.11.0/24]
            EC2A[EC2 Instance<br/>Nginx]
            NAT[NAT Gateway]
        end

        subgraph AZB["Availability Zone B"]
            PB[Public Subnet<br/>10.0.2.0/24]
            PVB[Private Subnet<br/>10.0.12.0/24]
            EC2B[EC2 Instance<br/>Nginx]
        end

        ASG[Auto Scaling Group<br/>Min 2 · Desired 2 · Max 4]
        LT[Launch Template<br/>Amazon Linux 2023 · User Data]
    end

    U --> IGW
    IGW --> ALB
    ALB --> TG
    TG --> EC2A
    TG --> EC2B
    ASG -. manages .-> EC2A
    ASG -. manages .-> EC2B
    LT -. launches .-> ASG
    EC2A -. outbound traffic .-> NAT
    EC2B -. outbound traffic .-> NAT
    NAT --> IGW

    classDef public fill:#dbeafe,stroke:#2563eb,color:#172554
    classDef private fill:#dcfce7,stroke:#16a34a,color:#14532d
    classDef compute fill:#fef3c7,stroke:#d97706,color:#78350f

    class PA,PB,ALB,IGW public
    class PVA,PVB private
    class EC2A,EC2B,ASG,LT compute
```
