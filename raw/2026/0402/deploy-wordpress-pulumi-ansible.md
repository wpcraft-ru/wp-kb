---
source: https://www.pulumi.com/blog/deploy-wordpress-aws-pulumi-ansible/
fetched: 2026-05-05
---

# Deploy WordPress to AWS using Pulumi and Ansible

Источник: Pulumi Blog (2022-06-27)

## Provisioning vs Configuration Management

- **Provisioning** (Pulumi, Terraform, CloudFormation): create/update/delete infrastructure
- **Configuration Management** (Ansible, Chef, Puppet): configure existing servers

Modern trend: immutable infrastructure (cattle vs pets). But config management remains relevant for stateful workloads.

## Approach: Pulumi + Ansible Together

1. Pulumi program defines infrastructure (VPC, subnets, RDS, EC2)
2. Pulumi runs Ansible playbook via remote-exec to configure WordPress
3. Single `pulumi up` orchestrates everything

## Infrastructure Components

- VPC with public/private subnets
- Internet Gateway + route tables
- RDS MySQL database (private subnet)
- EC2 instance (public subnet) with security groups
- SSH access with key pairs

## Pulumi Configuration

Multi-language support (Python, TypeScript, Go, C#, YAML). Configuration system allows varying settings per stack/environment (dev/prod).

## Key Patterns

- Secrets management for DB passwords and SSH keys
- Dynamic AZ and AMI lookup per region
- Idempotent provisioning

## Takeaway

Provisioning and configuration management are complementary. Pulumi handles cloud resources, Ansible handles server setup. Single command deploys everything.
