# Provisionamiento con ansible de César

Para probar el provisionamiento que mi compañero César ha realizado con Ansible, primero he clonado su repositorio:

```
git clone https://github.com/cesar2/Tripbot.git 
```

Luego, usando su par de claves.pem, he ejecutado el comando para ejecutar el playbook de ansible:

```
sudo ansible-playbook -i ansible_hosts --private-key parclaves.pem -b playbook.yml

```
Y como se comprueba en la siguiente imagen se ha ejecutado correctamente:

![ansibleCesar](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/probandoAnsibleCesar_zpsaid4pfa5.png)
