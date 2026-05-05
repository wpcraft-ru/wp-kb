---
source: https://oneuptime.com/blog/post/2026-02-23-how-to-build-a-wordpress-infrastructure-with-terraform/view
fetched: 2026-05-05
---

# How to Build a WordPress Infrastructure with Terraform

Источник: OneUp (2026-02-23)

## Architecture

Production-grade WordPress on AWS with Terraform. Components:
- **ECS Fargate** — WordPress containers (no servers)
- **RDS Aurora MySQL** — database with read replicas
- **EFS** — shared media uploads across containers
- **ElastiCache Redis** — object caching
- **CloudFront** — CDN and static asset caching
- **Application Load Balancer** — traffic distribution

## VPC and Networking

Multi-AZ VPC with public/private subnets using terraform-aws-modules/vpc/aws.

## RDS Aurora MySQL

- Aurora MySQL 8.0 with Serverless v2 scaling (0.5-16 capacity)
- Writer + Reader instances
- 7-day backup retention, encrypted storage

## EFS for Shared Media

- Encrypted EFS with IAM authorization
- Access point for /wordpress with www-data (uid/gid 33)
- Lifecycle policy: transition to IA after 30 days

## ElastiCache Redis

- Redis 7.1, multi-AZ with automatic failover
- cache.r6g.large, encrypted at rest and in transit

## ECS Task Definition

WordPress container with:
- DB connection to Aurora endpoint
- Redis cache via WP_REDIS constants in wp-config
- EFS mount for wp-content
- Secrets from AWS Secrets Manager

## Auto Scaling

- ECS service: 2-10 tasks
- Target tracking: 60% CPU utilization
- Scale out: 60s cooldown, scale in: 300s cooldown

## Summary

Multi-AZ Aurora + EFS + Redis + ECS Fargate auto-scaling. Handles thousands of concurrent users, cost-effective with serverless scaling.
