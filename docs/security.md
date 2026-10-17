# Security Best Practices

## 1. Overview

This document describes the security principles for the AWS High-Availability Web Application architecture. It covers identity and access management, network isolation, security groups, credential protection, and monitoring.

**Infrastructure status:** The AWS resources for this project have been deleted. This document describes the intended security design and recommended practices; it does not claim that these controls are currently deployed or verified.

## 2. IAM Least Privilege

AWS Identity and Access Management (IAM) controls who can access AWS resources and which actions they can perform.

Security recommendations:

- Grant users and roles only the permissions required for their responsibilities.
- Prefer IAM roles and temporary credentials over long-lived access keys.
- Attach an appropriate IAM instance profile to EC2 instances when AWS API access is required.
- Avoid using the AWS account root user for routine administration.
- Enable multi-factor authentication (MFA) for privileged identities.
- Review IAM policies periodically and remove unnecessary permissions.
- Never store access keys or secrets in application source code.

## 3. Security Groups

Security groups act as stateful virtual firewalls for supported AWS resources.

Recommended rules for this architecture:

| Resource | Inbound traffic | Recommended source |
|---|---|---|
| Application Load Balancer | HTTP (port 80) or HTTPS (port 443), as required | Approved client networks or the internet for a public website |
| EC2 application instances | Application traffic on the configured port | The Application Load Balancer's security group |
| Administrative access | SSH (port 22), only if required | An approved administrator IP or a secure management service |

Additional recommendations:

- Avoid exposing SSH directly to the entire internet.
- Do not open all ports or use unnecessarily broad rules.
- Keep inbound rules limited to the minimum required traffic.
- Review both inbound and outbound rules.
- Validate rules after changes to ensure that only intended traffic is allowed.

## 4. Public and Private Subnets

Separating public-facing resources from application instances reduces direct exposure.

**Public subnets**

- Host internet-facing resources such as a public Application Load Balancer.
- Use a route to an Internet Gateway when internet connectivity is required.
- Should not contain application instances that need to remain inaccessible directly from the internet.

**Private subnets**

- Host application EC2 instances that receive user traffic through the load balancer.
- Should not have a direct route from the internet to their instances.
- May use a NAT Gateway or another approved network design for outbound connectivity when required.
- Should allow application traffic only from intended sources, such as the load balancer.

Subnet placement alone does not provide complete isolation. Route tables, security groups, network ACLs, and resource configuration must work together.

## 5. Credential and Secret Management

Credentials must be protected throughout development, deployment, and maintenance.

- Never commit AWS access keys, secret keys, passwords, private keys, or tokens to GitHub.
- Use IAM roles and temporary credentials wherever practical.
- Store application secrets in an appropriate secrets-management service when required.
- Add local environment files and sensitive artifacts to `.gitignore`.
- Do not include credentials in README files, screenshots, terminal output, or architecture diagrams.
- If a credential is accidentally exposed, revoke or rotate it immediately. Removing it from a commit does not guarantee that it is no longer accessible.
- Review repository history if sensitive information has been committed.

## 6. Application Load Balancer Security

- Use HTTPS with a valid TLS certificate when serving production traffic.
- Redirect HTTP requests to HTTPS when appropriate.
- Configure listeners to forward traffic only to the intended target group.
- Restrict access to application instances so clients cannot bypass the load balancer.
- Use health checks to detect unavailable application targets.
- Review access logs and relevant monitoring data when investigating suspicious activity.

## 7. Monitoring and Auditing

Recommended AWS security and monitoring practices include:

- Use AWS CloudTrail to record supported AWS API activity.
- Review CloudWatch metrics and alarms for operational issues.
- Monitor unexpected changes to IAM policies and security groups.
- Review failed administrative access attempts where relevant logs are available.
- Protect logs from unauthorized access and retain them according to operational requirements.
- Use AWS Config or other approved configuration-auditing tools when needed.

These services and controls may incur charges depending on configuration and usage.

## 8. Security Validation Checklist

Use this checklist when the infrastructure is deployed again.

- [ ] IAM policies follow least-privilege principles.
- [ ] Privileged identities use MFA where appropriate.
- [ ] EC2 instances are placed in private subnets when direct internet access is unnecessary.
- [ ] Application instances accept traffic only from approved sources.
- [ ] Administrative access is restricted.
- [ ] HTTPS and TLS certificates are configured where required.
- [ ] No credentials or secrets are present in tracked repository files.
- [ ] Logging and monitoring are configured according to project requirements.
- [ ] Security group rules and route tables have been reviewed.

Do not mark a control as verified until it has actually been checked.

## 9. Conclusion

Applying least privilege, network segmentation, restrictive security group rules, secure credential handling, and appropriate monitoring helps reduce the attack surface of a high-availability AWS application.

This document serves as a security reference for the project and can be updated with verified implementation details and evidence when the infrastructure is deployed again.