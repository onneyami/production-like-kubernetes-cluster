# DevOps Project: Kubernetes Cluster Setup with Ansible (Infrastructure as Code)

Этот проект представляет собой набор Ansible-ролей, плейбуков и Kubernetes-манифестов для автоматизированного развёртывания production-подобного кластера Kubernetes с помощью `kubeadm`. Проект создан для демонстрации навыков написания инфраструктурного кода и не требует фактического запуска виртуальных машин (хотя при желании его можно развернуть с помощью Vagrant).

## Что внутри?

- **Vagrantfile** – описание трёх виртуальных машин (Ubuntu 22.04) для тестового окружения.
- **Ansible** – полный набор плейбуков и ролей:
  - `common` – базовая подготовка узлов (отключение swap, настройка containerd, sysctl).
  - `kubernetes` – установка kubeadm, kubelet, kubectl.
  - Плейбуки: подготовка узлов, установка kubeadm, инициализация control-plane, присоединение workers, установка Calico, деплой тестового приложения.
- **Kubernetes манифесты** – для сетевого плагина Calico и тестового Nginx-приложения с Ingress.

## Технологии

- Ansible
- Kubernetes (kubeadm)
- Docker/containerd
- Linux (Ubuntu)
- Git

## Как использовать (теоретически)

1. Установите Vagrant и VirtualBox (опционально).
2. Склонируйте репозиторий.
3. Запустите `vagrant up` для создания трёх виртуальных машин.
4. Установите Ansible (если не установлен).
5. Выполните плейбуки последовательно:
   ```bash
   ansible-playbook -i ansible/inventory/production/hosts.ini ansible/playbooks/01-prepare-nodes.yml
   ansible-playbook -i ansible/inventory/production/hosts.ini ansible/playbooks/02-install-kubeadm.yml
   ansible-playbook -i ansible/inventory/production/hosts.ini ansible/playbooks/03-init-cluster.yml
   ansible-playbook -i ansible/inventory/production/hosts.ini ansible/playbooks/04-install-calico.yml
   ansible-playbook -i ansible/inventory/production/hosts.ini ansible/playbooks/05-deploy-test-app.yml
   ```
