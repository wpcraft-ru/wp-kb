# How to Build a WordPress Infrastructure with Terraform (OneUptime — 2026)

Источник: https://oneuptime.com/blog/post/2026-02-23-how-to-build-a-wordpress-infrastructure-with-terraform/view
Дата: 2026-02-23

## Архитектура production-grade на AWS

- **ECS Fargate** — WordPress контейнеры (без управления серверами)
- **RDS Aurora MySQL** — база данных с read replicas
- **EFS** — общее файловое хранилище для медиа-загрузок
- **ElastiCache Redis** — объектное кеширование
- **CloudFront** — CDN для статических ассетов
- **Application Load Balancer** — распределение трафика

## VPC и сеть

- Private subnets для БД и кеша, public subnets для ALB
- 3 availability zones
- NAT Gateway
- DNS hostnames/support включены

## RDS Aurora MySQL

- Serverless v2 (0.5–16 ACU)
- Writer + Reader instance (HA)
- 7 дней retention бэкапов
- Storage encrypted

## EFS для общего хранилища

- Encrypted, KMS
- Общая файловая система через EFS Access Points
- POSIX user 33 (www-data)
- Throughput mode: bursting

## ElastiCache Redis

- 2 ноды, multi-AZ
- Redis 7.1
- At-rest и transit encryption
- Automatic failover

## ECS Task Definition

- Fargate launch type
- WordPress 6.4 + PHP 8.2 FPM
- EFS монтирование в /var/www/html/wp-content
- AWS CloudWatch Logs
- Secrets Manager для пароля БД

## Auto Scaling (ECS)

- Desired count: 2, min: 2, max: 10
- CPU-based target tracking (60%)
- Scale-in cooldown: 300s, scale-out: 60s

## CloudFront

- Перед ALB для глобального кеширования статики

## Вывод

architecture handles high availability with multi-AZ Aurora, shared storage with EFS, performance caching with Redis, and auto-scaling with ECS Fargate. Setup can handle thousands of concurrent users while remaining cost-effective during quiet periods.
