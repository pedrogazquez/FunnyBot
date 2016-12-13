# Orquestación con vagrant

Para realizar la orquestación con vagrant, lo primero que he hecho ha sido instalarlo:

```
sudo apt-get install vagrant

```

También he instalado el plugin de Vagrant para aws:

```
vagrant plugin install vagrant-aws
```

Posteriormente he añadido un box de vagrant, que ha sido el box de dummy:

```
vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box
```


