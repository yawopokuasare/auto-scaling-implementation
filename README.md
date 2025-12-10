# AWS Auto Scaling Implementation

## Overview
Production-ready infrastructure deployment demonstrating automated scaling of Nginx web servers using AWS Auto Scaling Groups, Application Load Balancer, and Target Tracking policies. This project ensures high availability and cost optimization through intelligent resource management.

## Architecture
```
Internet → ALB → ASG (EC2 Instances in Private Subnets) → Nginx Servers
         ↓
    Target Groups
         ↓
    Health Checks
```

<img width="11628" height="9820" alt="Image" src="https://github.com/user-attachments/assets/a2dd2ee1-1539-4bc5-8a0f-b998b39384ee" /> 

**Key Components:**
- Custom VPC with 3 public and 3 private subnets across multiple AZs
- Application Load Balancer for traffic distribution
- Auto Scaling Group with dynamic scaling policies
- Bastion host for secure SSH access to private instances
- Target Tracking scaling (30% average vCPU utilization threshold)

## Technologies Used
- **AWS Services:** VPC, EC2, Auto Scaling Groups, Application Load Balancer, CloudWatch
- **Web Server:** Nginx
- **IaC:** User Data scripts for automated server provisioning
- **Monitoring:** CloudWatch metrics for scaling triggers

## Features
- ✅ Automatic horizontal scaling based on CPU utilization
- ✅ Health checks with automatic instance replacement
- ✅ Load balancing across multiple availability zones
- ✅ Zero-downtime deployments
- ✅ Cost optimization through intelligent scaling

## Setup Instructions

### Prerequisites
- AWS Account with appropriate IAM permissions
- AWS CLI configured
- Understanding of VPC networking

### Deployment Steps
1. **Create VPC Infrastructure**
   - Configure VPC with public and private subnets
   - Set up Internet Gateway and NAT Gateway
   - Configure route tables

2. **Launch EC2 Instances**
   - Deploy bastion host in public subnet
   - Launch instances in private subnet with Nginx user data script

3. **Configure Auto Scaling Group**
   - Create launch template with AMI and instance configuration
   - Set minimum, maximum, and desired capacity
   - Configure Target Tracking policy (30% CPU threshold)

4. **Deploy Application Load Balancer**
   - Create target groups
   - Configure health checks
   - Attach ASG to ALB

### User Data Script Example
```bash
#!/bin/bash
yum update -y
yum install -y nginx
systemctl start nginx
systemctl enable nginx
```

## Challenges Solved
**SSH Access to Private Instances:**
- Implemented bastion host architecture for secure access
- Configured security groups for proper network segmentation
- Established SSH key management for multi-layer access

## Scaling Behavior
- **Scale Out:** Triggered when average CPU > 30%
- **Scale In:** Triggered when average CPU < 30%
- **Cooldown Period:** 300 seconds between scaling activities

## Monitoring & Verification
```bash
# Check ASG status
aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names <your-asg-name>

# View ALB target health
aws elbv2 describe-target-health --target-group-arn <your-target-group-arn>

# Monitor CloudWatch metrics
aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization
```

## Cost Optimization
- Automatic scale-in during low traffic periods
- Right-sized instance types based on workload
- Spot instance integration (optional enhancement)

## Future Enhancements
- [ ] Implement Terraform/CloudFormation for full IaC
- [ ] Add CloudWatch custom metrics for application-level scaling
- [ ] Integrate with AWS Systems Manager for patch management
- [ ] Implement blue/green deployment strategy

## What I Learned
- Designing resilient, multi-AZ architectures
- Implementing secure network segmentation with bastion hosts
- Configuring predictive scaling policies for cost optimization
- Troubleshooting ALB health check failures

---

**AWS Certified Solutions Architect**
