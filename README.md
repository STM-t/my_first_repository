# Microservice Deployment with Vagrant & Ansible

Этот Python-микросервиис собирает Prometheus метрики. Для раскатки этого сервиса используется Ansible и Vagrant. 

Гипервизор 2-го типа - virt-manager(libvirt)

ОС для ВМ - AlmaLinux 9

## Струкутра проекта

```
.
├── vagrant/
│   └── Vagrantfile
└── ansible/
    ├── ansible.cfg
    ├── inventory.ini
    ├── playbook.yml
    └── roles/
        └── microservice/
            ├── files/
            │   ├── Dockerfile
            │   ├── main.py
            │   ├── requirements.txt
            │   └── start_microservice.sh
            └── tasks/
                ├── docker-setup.yml
                └── main.yml
```

## Используемый стек

- Fedora 41
- QEMU/KVM virtualization
- virt-manager
- Vagrant
- Ansible

## Deployment Steps

### 1. Настройка ВМ с помощью Vagrant

1. Устновка virt-manager:
  
   ```
   sudo apt install virt-manager qemu-kvm libvirt-daemon-system libvirt-clients
   ```
   
3. Установка Vagrant:
  
   ```
   sudo apt install vagrant
   ```
   
5. Установка libvirt плагина для Vagrant:
  
   ```
   vagrant plugin install vagrant-libvirt
   ```
   
4. Установка AlmaLinux 9 box:
  
   ```
   vagrant box add generic/alma9
   ```
   
6. Запуск ВМ:
  
   ```
   cd vagrant
   vagrant up --provider=libvirt
   ```
   
### 2. Использование Ansible

1. Установка Ansible:
  
   ```
   sudo apt install ansible
   ```
   
3. Генерация SSH ключа:
  
   ```
   ssh-keygen
   ```
   
5. Copy SSH key to the VM (password is "password"):
  
   ```
   ssh-copy-id stm@<VM_IP>
   ```

Пароль по умолчанию:_password_, он может быть изменен в Vagranfile
   
7. Запуск Ansible playbook:
  
   ```
   cd ansible
   ansible-playbook playbook.yml -K
   ```

После запуска ввести пароль _password_. Он указывается в Vagranfile
   

### 3. Проверка метрик

1. Подключение к ВМ:
  
   ```
   ssh stm@<VM_IP>
   ```
   
3. Метрики с микросервиса:
  
   ```
   curl http://localhost:8080
   ```
   

