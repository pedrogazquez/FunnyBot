---
layout: index
---

# Provisionamiento con chef

Para realizar el provisionamiento con chef primero hemos conectado con la instancia de AWS con la imagen de Ubuntu Server 14.04 LTS:
```
ssh -i "pgazquezAWSKeys.pem" ubuntu@ec2-IP.us-west-2.compute.amazonaws.com
```

Una vez dentro, instalamos chef para realizar el provisionamiento correspondiente:

```
curl -L https://www.opscode.com/chef/install.sh | sudo bash
```

Luego instalamos Git:

```
sudo apt-get install git
```

Y clonamos nuestro repositorio:

![gitAWS](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/Captura%20de%20pantalla%20de%202016-11-24%2013-55-05_zpsfjhvhz1l.png)

Por último realizados estos pasos, provisionamos con el siguiente comando:

```
sudo chef-solo -c FunnyBot/Provisionamiento/Chef/chef/solo.rb

```

Que hace referencia al archivo [solo.rb](https://github.com/pedrogazquez/FunnyBot/blob/master/Provisionamiento/Chef/chef/solo.rb) que especifica la ubicación donde se almacenarán los archivos cache, la del cookook ("libro de recetas de provisionamiento") y la del fichero de atributos en json.


En la siguiente imagen se puede ver el provisionamiento correcto de chef una vez ejecutado el comando:

![chefProvi](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/ejecutandoCHef_zpsp8iqhauj.png)


[Volver a inicio](index)
