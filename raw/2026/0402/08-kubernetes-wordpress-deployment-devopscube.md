# How to Deploy WordPress on Kubernetes with Nginx and MySQL (DevOpsCube)

Источник: https://devopscube.com/deploy-wordpress-on-kubernetes/
Дата: ~2025

## Архитектура

Используемые Kubernetes объекты:
- **Namespace** — изоляция ресурсов
- **Secret** — MySQL credentials (base64 encoded)
- **ConfigMap** — Nginx конфиг + MySQL init script
- **PersistentVolumeClaim (PVC)** — 2Gi для WP и MySQL
- **StatefulSet** — MySQL БД
- **Deployment** — WordPress приложение
- **Services** — ClusterIP (MySQL) + NodePort (WordPress)
- **NetworkPolicy** — ограничение трафика к MySQL

## Custom Docker Image

Базовый образ: ubuntu:24.04
- Nginx вместо Apache
- PHP 8.2 FPM
- WordPress latest
- Startup script с sed-заменами для wp-config.php
- Health checks: liveness + readiness

## Шаги деплоя

1. Build custom Docker image (Nginx + PHP + WordPress)
2. Push to DockerHub
3. Create namespace (`kubectl create ns wordpress`)
4. Create Secret с MySQL credentials
5. Create ConfigMaps (nginx-cm.yaml, mysql-cm.yaml)
6. Deploy MySQL (StatefulSet + Service)
7. Deploy WordPress (Deployment + Service + PVC)
8. Create NetworkPolicy
9. Login to WordPress UI

## Health checks

- MySQL: livenessProbe (TCP :3306), readinessProbe (TCP :3306)
- WordPress: livenessProbe (GET /wp-admin/install.php), readinessProbe (GET /wp-login.php)

## WordPress Service

Type: NodePort, Port 30004
(Можно заменить на Ingress Controller вместо NodePort)

## Resource limits

WordPress: requests 256Mi/100m, limits 512Mi/200m
MySQL: requests 256Mi/100m, limits 512Mi/200m

## Cleanup

```bash
kubectl delete -f .
```

## Репозиторий манифестов

- https://github.com/techiescamp/kubernetes-projects.git
