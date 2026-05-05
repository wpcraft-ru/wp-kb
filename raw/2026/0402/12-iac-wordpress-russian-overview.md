# Infrastructure as Code для WordPress — русскоязычные материалы

Источник: web_search — "инфраструктура как код WordPress развертывание"
Дата: 2026-05-05

## Основные инструменты IaC для WordPress

- **Terraform** — провайдер-агностик, HCL, AWS/GCP/Azure
- **Ansible** — управление конфигурацией, автоматизация установки WP
- **AWS CloudFormation** — JSON/YAML шаблоны для AWS
- **Google Cloud Deployment Manager** — YAML/Python/Jinja2 для GCP
- **Pulumi** — мультиязычный провижининг (+ Ansible)

## Архитектуры AWS

- EC2 + RDS (managed MySQL)
- EFS для общего хранилища
- Load balancers + Auto-scaling

## Архитектуры Kubernetes

- Containerized WordPress
- Persistent volumes
- Managed DB services (like MySQL Database Service)
- Load balancers
- Storage classes → NFS

## Преимущества IaC

- Масштабируемость — быстрый деплой без ручных процессов
- Версионирование — конфиги в Git
- Повторяемость — staging = production
- Автоматические обновления + rollback
