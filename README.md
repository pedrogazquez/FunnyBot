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


# Licencia

Este proyecto usará una licencia GNU GENERAL PUBLIC LICENSE (Versión 3, 29 de Junio de 2007).

# Concurso de software libre

Aquí se adjunta la imagen de que este proyecto está inscrito en el concurso de software libre:

![Inscripcion software libre](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/InscripcionProyectosLibres_zps7lkqcacs.png)


# Hito 3

# Hito 4

# Hito 5




