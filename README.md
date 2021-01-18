# ansible

Для проверки дз необходимо запустить Vagrantfile, который поднимет 2 машины.

На одной из них будет развернут ansible, вторая для развертывания nginx с помощью
ansible-playbook. Необходимо чтобы скрипты ansible.sh и nginx.sh (эти скрипты
разворачивают ansible и прокидывают ssh ключи) лежали в 
одной директории с Vagranfile. После поднятия виртуальных машин, заходим на 
машину с ansible:
vagrant ssh ansible

Там выполняем:
ansible-playbook /vagrant/nginx.yml
И проверяем что на второй машине развернулся поднялся nginx
curl 192.168.50.11:8080
Или можно зайти на вторую машину:
vagrant ssh nginx
И там выполнить:
curl localhost:8080

```
---
- name: NGINX | Install and configure NGINX #имя задачи для Ansible
  hosts: nginx # имя хоста
  become: true
  vars:
    nginx_listen_port: 8080 #добавление переменой

  tasks: #задача
    - name: NGINX | Install EPEL Repo package from standart repo
      yum:  #установка epel release на хосте nginx
        name: epel-release
        state: present
      tags:
        - epel-package
        - packages

    - name: NGINX | Install NGINX package from EPEL Repo
      yum:  #установка nginx
        name: nginx
        state: latest
      notify:
        - restart nginx #Рестарт nginx
      tags:
        - nginx-package
        - packages

    - name: NGINX | Create NGINX config file from template
      template:
        src: /vagrant/nginx.conf.j2 #копирование конфига src
        dest: /etc/nginx/nginx.conf #dst
      notify:
        - reload nginx
      tags:
        - nginx-configuration

  handlers:
    - name: restart nginx
      systemd:
        name: nginx
        state: restarted
        enabled: yes

    - name: reload nginx
      systemd:
        name: nginx
        state: reloaded
 ```
 
 
