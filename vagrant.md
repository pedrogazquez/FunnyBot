---
layout: index
---

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

Y he definido mi [Vagrantfile](https://github.com/pedrogazquez/FunnyBot/blob/master/Orquestacion/Vagrant/Vagrantfile):

```
Vagrant.configure("2") do |config|
	
	config.vm.box = "dummy"

		
	config.vm.define :gestionImagenes do |gestionImagenes|
		gestionImagenes.vm.host_name = "gestionImagenes"
			
		gestionImagenes.vm.provider :aws do |aws, override|
			aws.access_key_id = [$CLAVE_AWS]
			aws.secret_access_key = [Clave o variable de entorno]
			aws.keypair_name = "parVagrantSSH"
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
			override.ssh.private_key_path = "~/Escritorio/parVagrantSSH.pem"
		end
		
		gestionImagenes.vm.provision :ansible do |ansible|
			ansible.sudo = true
			ansible.playbook = "../../Provisionamiento/Ansible/playbook.yml"
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
			override.ssh.private_key_path = ENV['PRIVATE_KEY']
		end
		
		gestionChistes.vm.provision :ansible do |ansible|
			ansible.sudo = true
			ansible.playbook = "../../Provisionamiento/Ansible/playbook.yml"
		end
	end
end


```

Lo siguiente que hacemos es ejecutar vagrant con:

```
vagrant up --provider=aws
```

Y se crean correctamente las dos instancias:

![capturaVagrant1](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/dosMaquinasFunc_zpsr2c9irsq.png)

Y también se provisiona cada una con Ansible:

![capturaVagrant2](http://i1042.photobucket.com/albums/b422/Pedro_Gazquez_Navarrete/ejecuPlayBooks_zps9wwxjjxd.png)
