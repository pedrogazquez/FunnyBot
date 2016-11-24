---
layout: index
---

# Ansible

Para realizar el provisionamiento con ansible primero he creado una instancia de AWS con la imagen de Ubuntu Server 14.04 LTS. Conecto con ella con el comando ssh siguiente:

```
ssh -i "pgazquezAWSKeys.pem" ubuntu@ec2-IP.us-west-2.compute.amazonaws.com
```

Para realizar el provisionamento con ansible, primero instalamos este:

```
sudo apt-get install ansible

```

Instalamos también la herramienta de línea de comandos de AWS para operar con este mediante el terminal:

```
sudo pip install awscli

```

Lo siguiente que haremos, será definir el fichero [playbook.yml](https://github.com/pedrogazquez/FunnyBot/blob/master/Provisionamiento/Ansible/playbook.yml) que contendrá las tareas que se ejecutarán en la máquina:

```
- hosts: all
  sudo: true
  tasks:
    - name: Actualizamos apt
      apt: update_cache=yes
    - name: Instalamos pip
      apt: name=python-setuptools state=present
      apt: name=python-dev state=present 
      apt: name=python-pip state=present
    - name: Instalamos supervisor
      apt: name=supervisor state=present
    - name: Instalamos la api pyTelegramBotAPI
      pip: name=pyTelegramBotAPI
    - name: Creamos superusuario con contraseña
      user: name=user shell=/bin/bash group=admin password="passwd"
```

Por útlimo antes de ejecutar el playbook con ansible, definiremos un fichero denominado [ansible_hosts](https://github.com/pedrogazquez/FunnyBot/blob/master/Provisionamiento/Ansible/ansible_hosts) que contiene la dirección IP de la máquina de AWS con la que conectará:

```
[aws]
54.244.0.4 ansible_ssh_user='ubuntu'
```

Una vez hemos acabado de realizar esto, procederemos a ejecutar las tareas con ansible, las ejecutaremos con el siguiente comando:

```
ansible-playbook -i ansible_hosts --private-key pgazquezAWSKeys.pem -b playbook.yml
```

Este comando ejecuta ansible, en las máquinas definidas en ansible_hosts (en este caso solo una), a la que conectará usando el par de claves definidas en el archivo pgazquezAWSKeys.pem y ejecutará en ella las tareas especificadas en el playbook definido.

Aquí el resultado de la ejecución correcta de ansible:

![ansibleEJECUCION](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/EjecutandoANsible2_zpscin3gjan.png)
