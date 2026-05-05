---
source: https://ddpa.ru/kb/it/devops/infrastructure-as-code/
fetched: 2026-05-05
---

# Infrastructure as Code (IaC)

Infrastructure as Code (IaC) — подход к управлению IT-инфраструктурой, при котором серверы, базы данных, сети и балансировщики описываются в конфигурационных файлах и разворачиваются автоматически.

## Ключевые свойства

- **Декларативность** — описывается желаемое состояние
- **Идемпотентность** — повторное применение не ломает
- **Воспроизводимость** — dev = staging = prod
- **Контролируемость** — все изменения в Git
- **Policy-as-Code** — автоматические security-политики

## Типы IaC

### Декларативный
Инструменты: Terraform, OpenTofu, AWS CloudFormation, Crossplane.

### Императивный
Инструменты: Ansible, Chef, SaltStack.

### Программируемый
Языки: Python, TypeScript, Go. Инструменты: Pulumi, AWS CDK, CDK for Terraform.

## Основные инструменты

| Инструмент | Язык | Провайдеры | Лучше всего для |
|-----------|------|-----------|----------------|
| Terraform / OpenTofu | HCL | 4800+ | Enterprise multi-cloud |
| Pulumi | Python/TS/Go/C# | 1500+ | Команды разработчиков |
| Ansible | YAML | Широкая | Конфигурация серверов |
| Crossplane | CRD (K8s) | K8s-native | Платформенная инженерия |
| Helm | Charts | K8s | Управление K8s-приложениями |

## Российские облака

| Платформа | Инструменты |
|-----------|-------------|
| Yandex Cloud | Terraform, Pulumi, Crossplane |
| VK Cloud | Terraform, Ansible, Packer, Pulumi |
| Selectel | Terraform |
| Cloud.ru (Сбер) | Terraform |
| Timeweb Cloud | Terraform |
| K2 Cloud (КРОК) | Terraform, Ansible |

Deckhouse — российская Kubernetes-платформа с встроенным IaC и GitOps, сертификация ФСТЭК.

## Best Practices

- Весь код в Git — никаких ручных правок в проде
- Модульность — переиспользуемые компоненты
- GitOps — изменения через PR и code review
- Тестирование — Checkov для security, Terratest для интеграций
- Drift detection — проверка расхождения кода и инфраструктуры
- Минимальные привилегии

## IaC vs PaaS

| IaC | PaaS (Railway, Vercel) |
|-----|----------------------|
| Управление инфраструктурой | Управление приложением |
| Полный контроль | Ограничен платформой |
| Требует DevOps-компетенций | Минимальный порог входа |

## Ссылки

- IBM — What is Infrastructure as Code
- AWS — What is IaC
- Red Hat — What is Infrastructure as Code
- EITT Academy — Terraform vs Pulumi vs OpenTofu 2026
- Yandex Cloud — Инструменты управления облаком
