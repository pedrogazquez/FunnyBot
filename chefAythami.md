---
layout: index
---

# Provisionamiento con chef de Aythami

Para probar el provisionamiento de mi compañero primero he conectado con mi máquina con ssh:

```
ssh -i "pgazquezAWSKeys.pem" ubuntu@ec2-54-244-0-4.us-west-2.compute.amazonaws.com
```

Despues he clonado su repositorio:

![clonandoAythami](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/clonandoAytha_zpsfelcrsjx.png)


Luego he generado la clave pública a partir de mi clave privada para que el provisionamiento se ejecute correctamente:
```
ssh-keygen -y -f pgazquezAWSKeys.pem > awsAythae.pem.pub
```

Y lo he transferido a la máquina aws mediante sftp:

![sftpTrans](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/sftpAyth_zpsikly5jhe.png)

Lo siguiente que he hecho ha sido cambiarle el nombre al de **authorized_keys** que es el que el que buscará.

Por último he ejecutado chef-solo con el siguiente comando:

```
sudo chef-solo -c ~/DeFesti/provision/chef/solo.rb
```

Y en la siguiente imagen se puede ver que el provisionamiento que mi compañero ha hecho con chef es correcto:

![provisionamientoChefAythami](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/chefCorrectoAyth_zpsxadsphbe.png)

[Volver a inicio](index)
