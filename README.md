# Установка Kubernetes с помощью kubespray

## Подготовка

Склонировать себе репозиторий:  
git clone https://github.com/kubernetes-sigs/kubespray  

Установка зависимостей  
sudo pip3 install -r requirements.txt

Копирование примера в папку со своей конфигурацией  
cp -rfp inventory/sample inventory/mycluster

## Конфигурация

Отредактировать файл inventory/mycluster/inventory.ini.

## Установка кластера

ansible-playbook -i inventory/mycluster/inventory.ini cluster.yml -b -v

## Проверка установки
kubectl version  
kubectl get nodes  

## Доступ к кластеру
Для доступа к кластеру извне нужно добавить параметр supplementary_addresses_in_ssl_keys: [51.250.42.98] в файл inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml Заново запустить установку кластера. После этого кластер будет доступен извне.
