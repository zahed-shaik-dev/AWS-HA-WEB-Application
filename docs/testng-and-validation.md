# Testing and Validation Report

## 1. Overview

This document defines the validation plan for the AWS High-Availability Web Application architecture. It describes the checks required to verify load balancing, instance health, automatic recovery, network security, and monitoring.

**Current infrastructure status:** AWS resources have been deleted. The tests below are documented validation procedures, not claims of a currently running deployment.

## 2. Validation Checklist

| Test | Validation procedure | Expected result |
|---|---|---|
| VPC networking | Review the VPC CIDR, subnets, and route tables | Resources use the intended network configuration |
| Application Load Balancer | Open the ALB DNS endpoint while the infrastructure is running | The application responds over HTTP |
| Target health | Inspect the target group's registered targets | Both EC2 targets report Healthy |
| Load balancing | Send multiple requests through the ALB | Requests can be served by registered healthy targets |
| Auto Scaling | Terminate a test instance during a controlled test | The Auto Scaling Group launches a replacement when configured to maintain desired capacity |
| Availability Zones | Review subnet and instance placement | Instances are distributed across the intended Availability Zones |
| Security groups | Review inbound and outbound rules | Only required traffic is permitted |
| Monitoring | Review CloudWatch metrics and configured alarms | Relevant metrics are available and configured alarms respond to their thresholds |

## 3. Load Balancer Validation

1. Confirm that the Application Load Balancer is active.
2. Verify that its listener forwards traffic to the correct target group.
3. Open the ALB DNS name in a browser.
4. Confirm that the application returns the expected response.
5. Review target health and investigate any unhealthy targets.

## 4. Auto Scaling Validation

1. Verify the Auto Scaling Group's desired, minimum, and maximum capacities.
2. Confirm that the launch template references the intended AMI and instance configuration.
3. During a controlled test deployment, terminate one instance.
4. Observe whether Auto Scaling launches a replacement.
5. Confirm that the replacement registers with the target group and becomes healthy.

Perform destructive tests only in a test environment.

## 5. Security Validation

- Confirm that administrative access is restricted to authorized sources.
- Allow HTTP or HTTPS traffic only as required by the application.
- Restrict application-instance access to the intended traffic sources.
- Review IAM permissions for least privilege.
- Never commit AWS access keys, secret keys, passwords, or other credentials to GitHub.

## 6. Monitoring Validation

Review relevant CloudWatch metrics, including:

- Application Load Balancer request count and target response time.
- Healthy and unhealthy target counts.
- EC2 CPU utilization and instance status checks.
- Auto Scaling Group capacity and scaling activities.

If alarms are configured, verify their thresholds and notification actions in a running test environment.

## 7. Results and Evidence

Record actual results here after performing the tests.

| Test | Status | Evidence |
|---|---|---|
| ALB response | Not tested | Add a genuine screenshot or result |
| Target health | Not tested | Add a genuine target-group screenshot |
| Auto Scaling replacement | Not tested | Add genuine scaling activity evidence |
| CloudWatch monitoring | Not tested | Add genuine metrics or alarm evidence |

Do not mark a test as passed until it has been performed and the result verified.

## 8. Conclusion

This report provides a repeatable validation plan for evaluating the application's high-availability design. When the infrastructure is deployed again, the recorded results and supporting evidence can be added to this document.