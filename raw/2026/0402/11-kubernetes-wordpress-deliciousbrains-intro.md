# Running WordPress in a Kubernetes Cluster (Delicious Brains)

Источник: https://deliciousbrains.com/running-wordpress-kubernetes-cluster/
Дата: 2017

## Введение

Статья — раннее введение в Kubernetes + WordPress, исторически значимая.

## Установка

- **Minikube** — локальный Kubernetes (VirtualBox driver)
- **kubectl** — CLI инструмент
- **Helm** — пакетный менеджер

## Процесс

1. `minikube start --vm-driver=virtualbox`
2. `brew install kubernetes-helm`
3. `helm init`
4. `helm install --namespace wordpress --name wordpress --set serviceType=NodePort stable/wordpress`

## Масштабирование

```bash
kubectl scale --replicas 2 deployments wordpress-wordpress --namespace=wordpress
```

## High Availability (Self-healing)

Kubernetes автоматически перезапускает упавшие Pods:
```bash
kubectl delete pod {POD-ID} --namespace=wordpress
# Kubernetes мгновенно создаёт замену
```

## Дальнейшие шаги

- Horizontal Pod Autoscaling
- Automated rollouts/rollbacks
- Secret management
- Self-healing через ReplicationController
