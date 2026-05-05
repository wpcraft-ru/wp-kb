---
source: web_search synthesis
fetched: 2026-05-05
---

# Kubernetes WordPress CI/CD Pipeline: Search Results Summary

## Key Findings

### CI/CD Pipeline для WordPress

Типичный pipeline:
1. Checkout кода (wp-content/themes, plugins)
2. Build Docker image (extends official WordPress image)
3. Push в registry (Docker Hub/GCR)
4. Apply K8s manifests (kubectl apply или Helm)
5. Optional: WP-CLI для миграций/cache flush

### Инструменты

| Tool | Use |
|------|-----|
| GitHub Actions | Build/push image + deploy on push to main |
| GitLab CI | GKE integration, GCR registry |
| Jenkins | Declarative pipeline, multi-app deploy |

### K8s Architecture

- WordPress Deployment + MySQL Deployment (separate pods)
- PersistentVolumeClaim для wp-content/uploads и MySQL data
- Secrets для DB credentials
- Ingress для HTTP routing
- Rolling updates для zero-downtime

### GitHub Actions Example

```yaml
name: Deploy to Kubernetes
on: [push]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Build and push
      uses: docker/build-push-action@v4
    - name: Deploy
      run: kubectl apply -f k8s/
```

### Docker + Terraform + WooCommerce

- Custom WordPress image с предустановленным WooCommerce
- Docker Compose: WordPress + MariaDB + Nginx
- Terraform: VPC, EC2/ASG, EFS, ALB
- EFS критичен для shared persistent volumes при scaling

### Sources

- Bitpoke App for WordPress (GitLab CI integration)
- kubernetes.io MySQL+WordPress tutorial
- devtron.ai GitHub Actions + K8s guide
- wpclimastery.com WordPress CI/CD with GitHub Actions + WP-CLI
