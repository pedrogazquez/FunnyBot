# Gestión de contenedores con docker

Para realizar la gestión y creación de contenedores con Docker, lo primero que he hecho ha sido instalar este, con un script que he definido:

```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | sudo tee /etc/apt/sources.list.d/docker.list
sudo apt-get update
apt-cache policy docker-engine
sudo apt-get update
sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
sudo apt-get update
sudo apt-get install docker-engine
sudo service docker start

```

Lo siguiente ha sido en la [página web de docker](https://hub.docker.com/) Hacer clic en Create, y seleccionar "Create Automated Build" y seleccionar el repositorio de GitHub del cual vamos a crear el contenedor.

Una vez hecho esto, he definido mi archivo [DockerFile](https://github.com/pedrogazquez/FunnyBot/blob/master/Dockerfile):

```
FROM ubuntu:latest

MAINTAINER Pedro Gazquez Navarrete <pedrogazqueznavarrete@gmail.com>



# Añadiendo herramientas Python
RUN apt-get -y update
RUN apt-get -y install python-setuptools
RUN apt-get -y install python-dev
RUN apt-get -y install python-pip
RUN apt-get -y install supervisor

#Instalando la API de BOTS de Telegram
RUN pip install pyTelegramBotAPI

#Añadiendo usuario
RUN useradd -m docker && echo "docker:docker" 

USER docker
CMD /bin/bash
```
Lo siguiente ha sido ejecutar el comando:

```
sudo docker pull pedrogazquez/funnybot
```

Para crear el contenedor, este comando se ejcuta dentro del repositorio (que anteriormente hemos clonado):

![dockerPull](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/dockerpull_zpscowuxd08.png)

Ahora arrancaremos el contenedor con el comando:

```
sudo docker run -it pedrogazquez/funnybot bash
```

Y como podemos ver a continuación se arranca correctamente y tiene instalado todo lo defnido en el Dockerfile:

![dockerRun](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/dcokerrun_zpsguihjgxb.png)
