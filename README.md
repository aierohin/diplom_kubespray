# Установка Kubernetes с помощью kubespray

## Подготовка

Склонировать себе репозиторий:  
git clone https://github.com/kubernetes-sigs/kubespray  

Установка зависимостей  
sudo pip3 install -r requirements.txt

Копирование примера в папку со своей конфигурацией  
cp -rfp inventory/sample inventory/mycluster

## Конфигурация

Обновление Ansible inventory с помощью билдера   
declare -a IPS=(158.160.33.134 158.160.22.243 51.250.36.172)  
CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}  

10.10.1.3 10.10.1.4 10.10.1.5 - адреса серверов    
Билдер подготовит файл inventory/mycluster/hosts.yaml. Там будут прописаны адреса серверов.  

```
all:
  hosts:
    node-cp:
      ansible_host: 158.160.33.134
      ansible_user: centos
      #ip: 158.160.44.13
      #access_ip: 158.160.44.13
    node-work-1:
      ansible_host: 158.160.22.243
      ansible_user: centos
      #ip: 158.160.10.230
      #access_ip: 158.160.10.230
    node-work-2:
      ansible_host: 51.250.36.172
      ansible_user: centos
      #ip: 51.250.33.150
      #access_ip: 51.250.33.150
  children:
    kube_control_plane:
      hosts:
        node-cp:
    kube_node:
      hosts:
        node-cp:
        node-work-1:
        node-work-2:
    etcd:
      hosts:
        node-cp:
    k8s_cluster:
      children:
        kube_control_plane:
        kube_node:
    calico_rr:
      hosts: {}
```
## Установка кластера

ansible-playbook -i inventory/mycluster/hosts.yaml cluster.yml -b -v

## Проверка установки
kubectl version  
kubectl get nodes  

## Доступ к кластеру
Для доступа к кластеру извне нужно добавить параметр supplementary_addresses_in_ssl_keys: [158.160.33.134] в файл inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml Заново запустить установку кластера. После этого кластер будет доступен извне.
