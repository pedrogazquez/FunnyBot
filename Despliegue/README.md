# Despliegue

Primero, para crear los dos contenedores aún no creados, (gestión de imágenes y gestión de categorías), debemos tener instalado Docker, como ya especifiqué en el [hito de Contenedores](https://github.com/pedrogazquez/FunnyBot/tree/master/Contenedores/Docker).

Después he definido los Dockerfiles de cada servicio, que son [Dockerfile-gestionimagenes](https://github.com/pedrogazquez/FunnyBot/blob/master/Despliegue/Docker/Dockerfile-gestionimagenes) y [Dockerfile-gestioncategorias](https://github.com/pedrogazquez/FunnyBot/blob/master/Despliegue/Docker/Dockerfile-gestioncategorias).

A continuación podemos ver el Dockerfile-gestionimagenes:

```
FROM ubuntu:latest

MAINTAINER Pedro Gazquez Navarrete <pedrogazqueznavarrete@gmail.com>


# Añadiendo herramientas Python
RUN apt-get -y update
RUN apt-get -y install python-setuptools
RUN apt-get -y install python-dev
RUN apt-get -y install python-pip
RUN apt-get -y install supervisor
# Añadiendo base de datos (MongoDB)
RUN pip install mlab
# Servicio de graficos
RUN pip install plotly

#Instalando la API de BOTS de Telegram
RUN pip install pyTelegramBotAPI

#Añadiendo usuario
RUN useradd -m docker && echo "docker:docker" 

USER docker
CMD /bin/bash

```

Y hemos creado los dos contenedores junto al que tenía del hito anterior:

	+ [Contedor del bot: docker pull pedrogazquez/funnybot] (https://hub.docker.com/r/pedrogazquez/funnybot/)
	
	+ [Contenedor gestión de imágenes: docker pull pedrogazquez/funnybot-gestionimagenes] (https://hub.docker.com/r/pedrogazquez/funnybot-gestionimagenes/)
	
	+ [Contenedor gestión de categorías: docker pull pedrogazquez/funnybot-gestioncategorias](https://hub.docker.com/r/pedrogazquez/funnybot-gestioncategorias/)
	
Una vez hecho esto he procedido a definir mi Vagrantfile, con las tres instancias correspondientes de aws:
```
Vagrant.configure("2") do |config|
	
	config.vm.box = "dummy"

		
	config.vm.define :gestionImagenes do |gestionImagenes|
		gestionImagenes.vm.host_name = "gestionImagenes"
			
		gestionImagenes.vm.provider :aws do |aws, override|
			aws.access_key_id = ENV['ACCESS_KEY_ID']
			aws.secret_access_key = ENV['ACCESS_KEY_SECRET']
			aws.keypair_name = ENV['ACCESS_KEYPAIR']
			aws.ami = "ami-01f05461"
			aws.instance_type = "t2.micro"
			aws.security_groups = ["vagrant"]
			aws.region = 'us-west-2'
			aws.availability_zone = "us-west-2a" 
			aws.tags = {
				'Name' => 'Instancia FunnyBot gestion imagenes',
				'Environment' => 'vagrant'
			}
			
			override.ssh.username = "ubuntu"
			override.ssh.private_key_path = "~/Escritorio/parClavesVagrantSSH.pem"
		end
		
		gestionImagenes.vm.provision :ansible do |ansible|
			ansible.sudo = true
			ansible.playbook = "Ansible/playbook-gestionimagenes.yml"
		end
		
	end
		
	config.vm.define :gestionChistes do |gestionChistes|
		gestionChistes.vm.host_name = "gestionChistes"
			
		gestionChistes.vm.provider :aws do |aws, override|
			aws.access_key_id = ENV['ACCESS_KEY_ID']
			aws.secret_access_key = ENV['ACCESS_KEY_SECRET']
			aws.keypair_name = ENV['ACCESS_KEYPAIR']
			aws.ami = "ami-01f05461"
			aws.instance_type = "t2.micro"
			aws.security_groups = ["vagrant"]
			aws.region = 'us-west-2'
			aws.availability_zone = "us-west-2a"
			aws.tags = {
				'Name' => 'Instancia FunnyBot gestion chistes',
				'Environment' => 'vagrant'
			}
			 
			override.ssh.username = "ubuntu"
			override.ssh.private_key_path = "~/Escritorio/parClavesVagrantSSH.pem"
		end
		
		gestionChistes.vm.provision :ansible do |ansible|
			ansible.sudo = true
			ansible.playbook = "Ansible/playbook-gestioncategorias.yml"
		end
	end
	
	config.vm.define :bot do |bot|
		bot.vm.host_name = "bot"
			
		bot.vm.provider :aws do |aws, override|
			aws.access_key_id = ENV['ACCESS_KEY_ID']
			aws.secret_access_key = ENV['ACCESS_KEY_SECRET']
			aws.keypair_name = ENV['ACCESS_KEYPAIR']
			aws.ami = "ami-01f05461"
			aws.instance_type = "t2.micro"
			aws.security_groups = ["vagrant"]
			aws.region = 'us-west-2'
			aws.availability_zone = "us-west-2a"
			aws.tags = {
				'Name' => 'Instancia bot FunnyBot',
				'Environment' => 'vagrant'
			}
			 
			override.ssh.username = "ubuntu"
			override.ssh.private_key_path = "~/Escritorio/parClavesVagrantSSH.pem"
		end
		
		bot.vm.provision :ansible do |ansible|
			ansible.sudo = true
			ansible.playbook = "Ansible/playbook.yml"
		end
	end
end

```

Donde especificamos un playbook para cada servicio: [el bot de telegram](https://github.com/pedrogazquez/FunnyBot/blob/master/Despliegue/Ansible/playbook.yml), [gestión de imágenes](https://github.com/pedrogazquez/FunnyBot/blob/master/Despliegue/Ansible/playbook-gestionimagenes.yml) y [gestión de categorías](https://github.com/pedrogazquez/FunnyBot/blob/master/Despliegue/Ansible/playbook-gestioncategorias.yml)
	
Por último, como vimos en el [hito de Provisionamiento](https://github.com/pedrogazquez/FunnyBot/tree/master/Orquestacion/Vagrant) ejecutamos:

```
vagrant up --provider=aws
```

Y se ejecuta correctamente:

![VagrantUp](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/capturaEcucionPlaybooks_zpspimgb9d1.png)

Y vemos las tres instancias de aws de cada servicio ejecutándose correctamente:

![AwsBie](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/awsEjec_zpsls1q0jtt.png)
