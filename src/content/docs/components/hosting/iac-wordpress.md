---
title: "Infrastructure as Code для WordPress"
description: "IaC для WordPress: Terraform, Pulumi, Ansible, Docker, Kubernetes — обзор инструментов, архитектурных шаблонов и российских облаков."
---

## Что такое Infrastructure as Code (IaC)

Подход, при котором серверы, базы данных, сети и балансировщики описываются в конфигурационных файлах и разворачиваются автоматически. Для WordPress: инфраструктура под сайт создаётся одной командой.

**Ключевые свойства:** декларативность, идемпотентность, воспроизводимость (dev = staging = prod), весь код в Git.

## Типы IaC

| Тип | Суть | Инструменты |
|-----|------|------------|
| **Декларативный** | Описывается желаемое состояние | Terraform, OpenTofu, CloudFormation, Crossplane |
| **Императивный** | Пошаговые инструкции | Ansible, Chef, SaltStack |
| **Программируемый** | На Python, TS, Go | Pulumi, AWS CDK, CDK for Terraform |

## Инструменты для WordPress

| Инструмент | Язык | Для WordPress |
|-----------|------|---------------|
| **Terraform / OpenTofu** | HCL | AWS (EC2+RDS+EFS+Redis+CloudFront), Yandex Cloud, VK Cloud |
| **Pulumi** | Python/TS/Go/C# | Команды разработчиков, мульти-стек |
| **Ansible** | YAML | Установка WP, PHP, Nginx на готовых VM |
| **Docker Compose** | YAML | Локальная разработка и простой продакшен |
| **Helm / kubectl** | Charts / YAML | WordPress в Kubernetes |

## Terraform на AWS: архитектура (тезисно)

Production-grade схема для тысяч одновременных пользователей:

```
CloudFront → ALB → ECS Fargate (WordPress, Auto Scaling 2–10)
                    ├── EFS (общее wp-content)
                    ├── RDS Aurora MySQL Serverless v2
                    └── ElastiCache Redis
```

**Ключевые компоненты:**
- **VPC** — 3 AZ, public (ALB) + private (приложение, БД, кеш) подсети
- **RDS Aurora Serverless v2** — 0.5–16 ACU, writer + reader, 7 дней бэкапов, encrypted
- **EFS** — общее хранилище `wp-content/uploads` для всех контейнеров (иначе загрузка на одном не видна на других)
- **ElastiCache Redis** — 2 ноды multi-AZ, объектное кеширование (ускорение ×10–100)
- **ECS Fargate** — WordPress 6.4 + PHP 8.2 FPM, Secrets Manager для паролей
- **Auto Scaling** — по CPU (60%), 2–10 контейнеров
- **CloudFront** — перед ALB, не кеширует `/wp-admin/*`

**Стоимость (ориентир):** ~$300–400/мес. Упрощённый вариант — один EC2 + RDS MySQL (не Aurora) — ~$55/мес.

## Docker Compose: локально и продакшен

**Локальная разработка:** MySQL + WordPress (официальные образы), `/wp-content` в локальный volume, `WP_DEBUG=true`.

**Production-архитектура:** Nginx reverse proxy (единственная точка входа) → PHP-FPM WordPress + MySQL + Redis. Приватная сеть — только proxy наружу.

**Обязательно:** HTTPS (Let's Encrypt + Certbot), `.env` в `.gitignore`, named volumes для БД и wp-content, health checks, resource limits, конкретные теги образов (не `latest`).

**Pre-production чек-лист:**
- HTTPS активен, только 80/443 публично
- Persistent volumes, автоматические бэкапы (внешнее S3)
- Staging = production, план отката задокументирован

## Kubernetes: когда и как

**Когда оправдан:** десятки тысяч посетителей/день, горизонтальное масштабирование, zero-downtime деплой. Для типовых сайтов — избыточен.

**Архитектура:** Ingress (Nginx) → Service → Deployment (WordPress pods, 2–10) + PVC (RWX через OpenEBS NFS) ← MySQL StatefulSet + Redis StatefulSet.

**Ключевые объекты:** Namespace, Secret, ConfigMap, PVC, StatefulSet (MySQL), Deployment (WP), Service, Ingress + Cert-Manager.

**Проблема RWX:** несколько реплик WordPress должны видеть одни `wp-content/uploads`. Стандартное блочное хранилище (RWO) монтируется только к одному поду. Решение: OpenEBS NFS Provisioner или AWS EFS CSI driver.

**Helm (Bitnami):** `helm install my-wp bitnami/wordpress --set externalDatabase.host=... --set persistence.storageClass=rwx-storage`

**Стоимость (DigitalOcean):** ~$220–315/мес (DOKS 3×4cpu/8gb + Managed MySQL + Redis + LB).

## Связка Pulumi + Ansible

Pulumi создаёт облачную инфраструктуру (VPC, RDS, EC2), Ansible настраивает серверы (установка WP, PHP, Nginx). Всё одной командой `pulumi up`.

## Российские облака

| Платформа | Terraform | Pulumi | Ansible | Особенности |
|-----------|:---------:|:------:|:-------:|------------|
| **Yandex Cloud** | ✓ | ✓ | ✓ | Crossplane |
| **VK Cloud** | ✓ | ✓ | ✓ | Packer, зрелая документация |
| **Selectel** | ✓ | — | ✓ | Выделенные серверы |
| **Cloud.ru (Сбер)** | ✓ | — | — | ФСТЭК |
| **Timeweb Cloud** | ✓ | — | — | Простой вход |
| **Deckhouse (Флант)** | K8s-native | — | — | Российская K8s, GitOps, ФСТЭК |

## Best Practices

- Весь код в Git, изменения через PR
- Модульность, переиспользуемые компоненты
- GitOps, тестирование (Checkov, Terratest)
- Drift detection, минимальные IAM-привилегии

## Когда IaC, а когда PaaS

| IaC | PaaS (Managed WP) |
|-----|-------------------|
| Полный контроль | Управление приложением |
| Нужны DevOps-компетенции | Минимальный порог входа |
| High-load, compliance | Типовые сайты |

Для большинства проектов на старте PaaS практичнее. IaC оправдан при: 5+ серверов, high-load, мульти-региональный деплой, compliance.

## Материалы и источники

- [DDpa: Infrastructure as Code](https://ddpa.ru/kb/it/devops/infrastructure-as-code/)
- [OneUptime: WordPress Infrastructure with Terraform on AWS](https://oneuptime.com/blog/post/2026-02-23-how-to-build-a-wordpress-infrastructure-with-terraform/view)
- [CloudThat: Scalable WordPress on AWS with Terraform](https://www.cloudthat.com/resources/blog/building-a-scalable-wordpress-infrastructure-on-aws-with-terraform/)
- [Pulumi: Deploy WordPress to AWS](https://www.pulumi.com/blog/deploy-wordpress-aws-pulumi-ansible/)
- [DevOpsCube: Deploy WordPress on Kubernetes](https://devopscube.com/deploy-wordpress-on-kubernetes/)
- [DigitalOcean: WordPress on K8s with Helm](https://www.digitalocean.com/community/developer-center/deploying-a-wordpress-website-on-a-kubernetes-cluster-using-helm)
- [Automattic: DevOps for WordPress](https://automattic.com/for-agencies/blog/wordpress-devops/)
