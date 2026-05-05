# Kubernetes WordPress — Official Tutorial + Helm + Bitnami

Источник: web_search — "kubernetes wordpress"
Дата: 2026-05-05

## Official Kubernetes Tutorial (k8s.io)

- Minikube + Kustomize + YAML manifests
- Дефолтный single-instance WordPress + MySQL
- Secret для MySQL пароля (kustomization.yaml)
- Deployments: MySQL (mysql:8.0) + WordPress (wordpress:6.2.1-apache)
- PersistentVolumeClaims: 20Gi each
- Apply: `kubectl apply -k ./`
- Доступ: `minikube service wordpress --url`
- ⚠️ Development only, не production

## Helm Chart (Bitnami)

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-wordpress bitnami/wordpress
```

Фичи:
- MariaDB bundled
- High availability
- External databases
- Customizable persistence и ingress

## Key Components Comparison

| Component   | Purpose                       | Example Config          |
|-------------|-------------------------------|-------------------------|
| Deployment  | WordPress/MySQL pods          | Replicas: 1, Recreate   |
| Service     | Expose ports (80 WP, 3306 DB) | LoadBalancer/NodePort   |
| PVC         | Persistent storage (20Gi)     | ReadWriteOnce           |
| Secret      | DB password                   | Generated via Kustomize |
| Ingress     | External access (prod)        | Host-based routing      |

## Production considerations

- Scale with replicas + Ingress для production
- Use operators для управления БД (MySQL Operator)
- Consider managed DB services вместо in-cluster MySQL
