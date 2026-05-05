---
source: https://www.cloudthat.com/resources/blog/building-a-scalable-wordpress-infrastructure-on-aws-with-terraform/
fetched: 2026-05-05
---

# Building a Scalable WordPress Infrastructure on AWS with Terraform

Источник: CloudThat

## Overview

WordPress on AWS with Terraform: automation, repeatability, scalability.

## Architecture Steps

### Step 1: Terraform Config
```hcl
provider "aws" {
  region = "ap-south-1"
}
```

### Step 2: VPC
- VPC with CIDR 10.0.0.0/16
- Public subnet (10.0.1.0/24) for EC2
- Private subnet (10.0.2.0/24) for RDS
- Internet Gateway + Route Table

### Step 3: RDS MySQL
- mysql 8.0.21, db.t2.micro
- 20GB gp2 storage
- Not publicly accessible

### Step 4: EC2 Instance
- Ubuntu 20.04 LTS
- LEMP stack installation via remote-exec provisioner
- WordPress download and setup via shell commands

### Step 5: Application Load Balancer
- Distributes traffic to EC2
- Health check on /

### Step 6: Target Group + Listener
- Target group on port 80
- Listener forwards traffic to target group

## Benefits

- Reproducibility across environments
- Scalability via ALB
- Cost optimization
- Streamlined deployment processes
