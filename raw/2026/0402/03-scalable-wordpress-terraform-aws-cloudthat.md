# Building a Scalable WordPress Infrastructure on AWS with Terraform (CloudThat)

Источник: https://www.cloudthat.com/resources/blog/building-a-scalable-wordpress-infrastructure-on-aws-with-terraform/
Дата: ~2025

## Архитектура

- EC2 instance с Ubuntu 20.04 LTS
- RDS MySQL 8.0.21 (db.t2.micro, 20GB gp2)
- Application Load Balancer
- Target Group с health check
- VPC с public/private subnets

## Terraform ресурсы

- **Provider**: AWS (region: ap-south-1)
- **VPC**: 10.0.0.0/16
- **Subnets**: public (10.0.1.0/24), private (10.0.2.0/24)
- **Internet Gateway** + Route Table
- **Security Groups**: SSH (22) и HTTP (80)
- **EC2**: AMI Ubuntu 20.04, t2.micro
- **Provisioner "remote-exec"**: установка Apache2, MySQL client, PHP, WordPress через inline commands

## Процесс деплоя

1. `terraform init`
2. `terraform plan`
3. `terraform apply`

## Компоненты

- **VPC** с публичными и приватными подсетями
- **RDS** (MySQL, внутренний, непубличный доступ)
- **EC2** с user data / provisioner для установки WordPress
- **ALB** для распределения трафика
- **Target Group** с health check на "/"
- **Listener** на порту 80

## Преимущества подхода

- Версионирование инфраструктуры
- Автоматизация и повторяемость
- Масштабируемость через ALB + EC2 Auto Scaling
- Управляемая БД (RDS) — бэкапы, обновления, репликация

## AWS CloudFormation vs Terraform

CloudFormation также упоминается как альтернатива для AWS-only окружений.
