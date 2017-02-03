#FunnyBot

# Uso correcto de Git y GitHub.

Repositorio para el proyecto que se realizará para la asignatura del Máster en Ingeniería Informática Cloud Computing de la ETSIIT de Granada.


# Estructuración del proyecto.

## Introducción
En este proyecto se realizará el despliegue de un Bot de Telegram. Telegram es una aplicación de mensajería gratuita y multiplataforma. La mayor parte de Telegram es de código libre. 

Este Bot se realiza para que los usuarios de Telegram tenga acceso a multitud de chistes de varias categorías y extensión para compartirlos con sus contactos y así pasar un rato de diversión y risas.

La necesidad que cubre este Bot no es otra que la del entretenimiento, que hoy día ocupa gran parte de las aplicaciones desarrolladas en la red.

## Tecnología

Para la realización de este proyecto se usará [Telegram Bot API](https://github.com/eternnoir/pyTelegramBotAPI). Esta API es una interfaz basada en HTTP que se creó para ayudar a los desarrolladores interesados en los Bots de Telegram en la construcción de estos.

### Arquitectura

La arquitectura que seguirá este proyecto será una basada en microservicios. Esta arquitectura se basa en distintos módulos independientes que juntos dan lugar a la aplicación final. 

Un bot de Telegram se comporta como una cuenta de usuario, y para interaccionar con él, el usaurio deberá hablar con este.

### Microservicios

Para empezar, este proyecto contará con tres microservicios:

* Gestión de imágenes de chistes: en este microservicio (con su propia base de datos) se gestionarán las imágenes de los chistes. De esta forma si se cae este microservicio el usuario podrá seguir consultando chistes con el unico incoveniente de que no verá las imágenes.

* Gestión de chistes por contenido: en este microservicio se realizará la organización de chistes por categorías, siendo cada categoría un mini microservicio dentro de este microservicio, de esta forma si falla alguna categoría las otras estarán disponibles para los usuarios.

* Servicio de añadir nuevos chistes: se dará la oportunidad al usuario de añadir nuevos chistes al Bot. Este microservicio proporcionará al usuario la opción de añadir un chiste en la categoría deseada. De esta forma una vez que lo añada y sea comprobado correctamente el chiste quedará añadido a la base de datos correspondiente.

# Provisionamento

Para la realización de este hito se han elegido dos sistemas de provisionamiento (Ansible y Chef). Se provisionará una instancia de Amazon Web Services con Ubuntu Server.

## Ansible

Se ha elegido Ansible porque es uno de los sistemas de provisionamento más conocidos y por su flexibilidad y multitud de opciones. 

[Ir a provisionamiento con ansible](https://github.com/pedrogazquez/FunnyBot/blob/master/Provisionamiento/Ansible/README.md)

## Chef

También se ha realizado el aprovisionamiento con chef. Esta librería resuelve el problema de la creación repetitiva al crear máquinas e infraestructuras virtuales. Tiene un plugin que permite escribir "recetas de provisionamento" para cualquier infraestructura virtual. En este caso la he usado con una máquina de EC2 que tiene Ubuntu Server 14.04.

[Ir a provisionamiento con chef](https://github.com/pedrogazquez/FunnyBot/blob/master/Provisionamiento/Chef/README.md)


## Probando el provisionamiento de los compañeros

El primero que he probado ha sido el provisionamento que ha realizado mi compañero [Aythami Estévez](https://aythae.github.io/DeFesti/chef) con Chef y he comprobado que funciona correctamente.

[Probando el provisionamiento con chef de Aythami](https://github.com/pedrogazquez/FunnyBot/blob/master/Provisionamiento/Chef/ChefAythami.md)


Adicionalmente he probado el provisionamiento que mi compañero [César Albusac](https://github.com/cesar2/Tripbot/) ha realizado con Ansible.

[Probando el provisionamiento con ansible de César](https://github.com/pedrogazquez/FunnyBot/blob/master/Provisionamiento/Ansible/AnsibleCesar.md)


# Orquestación

Para la orquestación de mi aplicación (hito 3) he elegido Vagrant.

## Vagrant

Vagrant es una herramienta para la creación y configuración de entornos de desarrollo virtualizados. Se ha usado con la virtualización de AWS y con el sistema de provisionamiento de Anisble.

[Ir a orquestación con Vagrant](https://github.com/pedrogazquez/FunnyBot/tree/master/Orquestacion/Vagrant)

## Probando el provisionamiento de los compañeros

He probado la orquestación que ha realizado mi compañero [César Albusac](https://github.com/cesar2/Tripbot/) con Vagrant

[Probando la orquestación con vagrant de César](https://github.com/pedrogazquez/FunnyBot/blob/master/Orquestacion/Vagrant/VagrantCesar.md)

# Contenedores

Para la creación y configuración de entornos de desarrollo virtualizados he usado Dcoker, ya que es capaz de trabajar con múltiples proveedores virtuales.

## Docker

Docker es una herramienta para la configuración y creación de entornos de desarrollo virtualizados que originalmente se desarrolló para VirtualBox y sistemas de configuración tales como Chef, Salt y Puppet.

[Ir a gestión de contenedores con docker](https://github.com/pedrogazquez/FunnyBot/blob/master/Contenedores/Docker/README.md)

## Probando el contenedor de los compañeros

He probado el contenedor que ha realizado mi compañero [Alejandro Casado](https://github.com/acasadoquijada/MyStudentBot)

[Probando el contenedor docker de Alex](https://github.com/pedrogazquez/FunnyBot/blob/master/Contenedores/Docker/DockerAlex.md)


# Despliegue

Para el despliegue de este bot de telegram, he definido dos microservicios y el propio Bot:

	El propio bot, definido anteriormente con su correspondiente [contenedor docker](https://hub.docker.com/r/pedrogazquez/funnybot/) que es el servicio que ejecutará el Bot de Telegram.
	
	Un servicio que gestiona las imágenes de los chistes, con su propia base de datos de imágenes y sistema de log. [Ir a contenedor docker](https://hub.docker.com/r/pedrogazquez/funnybot-gestionimagenes/).
	
	Por último un servicio de gestión de categorías de chistes, que gestiona tanto las categorías de estos, como la adicción y eliminado de los chistes. [Ir a contenedor docker de gestión de categorías](https://hub.docker.com/r/pedrogazquez/funnybot-gestioncategorias/)

Para la gestión de la base de datos, se ha seleccionó MongoDB como ya se dijo en hitos anteriores. Para la administración de la base de datos en la nube, he elegido [MLab](https://mlab.com/), ya que se ejecuta en AWS también. Además tiene un plan llamado Sandbox que nos proporciona una base de datos con 0,5 GB de almacenamiento en una base de datos compartida, no tiene fecha de caducidad, tiene copias de seguridad, soporte de dase de datos de expertos, restauraciones fáciles, etc.

![capturaMlab](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/Captura%20de%20pantalla%20de%202017-02-03%2014-24-13_zpsenszxitg.png)

Para la gestión de logs se ha elegido [papertrail](https://papertrailapp.com/) ya que tiene una visibilidad instantánea de los logs. Es muy flexible y además proporciona un seguimiento de los problemas de los clientes, mensajes de error, etc.

También porque tiene un plan gratuito muy completo y es fácil de usar en tres sencillos pasos. [Ir a setup papertrail](https://papertrailapp.com/systems/setup)



[Ir a despliegue](https://github.com/pedrogazquez/FunnyBot/blob/master/Despliegue/README.md)


# Licencia

Este proyecto usará una licencia GNU GENERAL PUBLIC LICENSE (Versión 3, 29 de Junio de 2007).

# Concurso de software libre

Aquí se adjunta la imagen de que este proyecto está inscrito en el concurso de software libre:

![Inscripcion software libre](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/InscripcionProyectosLibres_zps7lkqcacs.png)

# Hito 4

# Hito 5






