# Deploying WordPress on Kubernetes Cluster using Helm (DigitalOcean)

Источник: https://www.digitalocean.com/community/developer-center/deploying-a-wordpress-website-on-a-kubernetes-cluster-using-helm
Дата: ~2022-2023

## Архитектура на DigitalOcean

- **DOKS** — Managed Kubernetes (3 worker nodes, 4cpu/8gb)
- **OpenEBS Dynamic NFS Provisioner** — RWX storage для WordPress (горизонтальное масштабирование)
- **DO Managed MySQL** — внешняя БД (master + slave, 2cpu/4gb)
- **Redis** — кеширование (managed или Helm chart)
- **Nginx Ingress Controller** — entry point
- **Cert-Manager** — Let's Encrypt TLS
- **WordPress Helm Chart** — Bitnami

## Storage для масштабирования

Проблема: DigitalOcean Block Storage — RWO (ReadWriteOnce).
Решение: OpenEBS Dynamic NFS Provisioner → RWX PVC.

StorageClass для RWX:
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: rwx-storage
provisioner: openebs.io/nfsrwx
reclaimPolicy: Delete
```

## WordPress + Redis

Redis для кеширования MySQL запросов.
Опции: managed DO Redis или Bitnami Helm chart.

## TLS

- Nginx Ingress Controller (Helm)
- Cert-Manager (Let's Encrypt)
- Production-ready TLS сертификаты

## Возможности

- Горизонтальное масштабирование WordPress (HPA)
- Zero-downtime upgrades через Helm
- Rollback через Helm
- Мониторинг метрик
- NitroPack plugin для оптимизации (CDN, lazy loading, caching)

## Стоимость (пример, 2022)

- DOKS: 3 nodes × 4cpu/8gb = $96-192/мес
- DO MySQL: 2 nodes × 2cpu/4gb = ~$100/мес
- DO Redis (managed): от $10/мес
