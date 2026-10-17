
# Troubleshooting Notes

## Issue: ALB Targets Unhealthy

### Observed Behavior

The target group reported health-check failures with timeout details during validation.

### Troubleshooting Checklist

1. Confirm that the EC2 instances are running and pass their system status checks.
2. Verify that Nginx installed successfully through EC2 user data.
3. Confirm that Nginx is running and listening on port 80.
4. Verify that the EC2 security group allows inbound TCP port 80 from the ALB security group.
5. Verify that the ALB security group allows inbound HTTP traffic from the intended clients.
6. Check that the target group uses the correct protocol, port, and health-check path.
7. Confirm that the target instances are registered with the target group.
8. Review route tables and network ACLs if connectivity is still failing.
9. Inspect cloud-init logs and Nginx logs on the instance.

### Useful Log Files

- `/var/log/cloud-init-output.log`
- `/var/log/nginx/error.log`
- `/var/log/nginx/access.log`

### Resolution Status

The last recorded result was a target health-check timeout. The root cause has not yet been confirmed.

Update this section after identifying the cause and successfully retesting the application.

## Lessons Learned

- Security groups must allow traffic between the ALB and application instances.
- User-data scripts must complete successfully.
- Target health checks are essential for verifying application connectivity.
- Infrastructure creation alone does not prove that an application is healthy.
