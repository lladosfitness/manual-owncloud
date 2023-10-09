#Ahora vamos a instalar el owncloud

#Primero tenemos que crear una carpeta para nuestro vagrant

```
[alumne@elpuig ~]$ mkdir example
[alumne@elpuig ~]$ cd example/
```

#Despues subimos nuestra maquina virtual que sera jammy64

```
[alumne@elpuig example]$ vagrant init ubuntu/jammy64
```

#Lo subimos

```
[alumne@elpuig example]$ vagrant up --provider=virtualbox
```

#Hacemos ssh

```
[alumne@elpuig example]$ vagrant ssh
```

#Nos convertimos en root

```
sudo -s
```

#Actualizamos

```
apt update
apt upgrade
```

#Ahora instalamos el apache, mysql y php

```
apt install -y apache2
```

```
apt install -y mysql-server
```

```
apt install -y php libapache2-mod-php
apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
```

#Ahora reiniciamos el servidor apache

```
systemctl restart apache2
```

#Ahora configuramos el mysql

```
root@elpuig:~$ mysql
```

```
CREATE DATABASE bbdd;
```

```
CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

```
GRANT ALL ON bbdd.* to 'usuario'@'localhost';
```

#Salimos

```
exit
```

#Comprobamos conexion desde afuera

```
alumne@elpuig:~$ mysql -u usuario -p
```

#Ahora descargamos el zip de owncloud desde este link

```
https://owncloud.com/download-server/
```

#Una vez descargado movemos el archivo a la carpeta donde tenemos nuestro repositorio

Despues movemos el archvio desde vagrant a /var/www/html

```
mv /vagrant/owncloud.zip /var/www/html/
```

#Hacemos cd a /var/www/html y ponemos estos comandos

```
chmod -R 775 .
chown -R root:www-data .
```

#Le hacemos unzip al archvivo

```
unzip owncloud.zip
```

#Eliminamos los demas archivos

```
rm owncloud.zip
rm index.html
```

#Ahora eliminamos los php que queremos

```
apt-get remove php-common
```

#Ahora instalamos unos nuevos

```
add-apt-repository ppa:ondrej/php -y
```

```
apt install php7.4 php7.4-intl php7.4-mysql php7.4-mbstring \
       php7.4-imagick php7.4-igbinary php7.4-gmp php7.4-bcmath \
       php7.4-curl php7.4-gd php7.4-zip php7.4-imap php7.4-ldap \
       php7.4-bz2 php7.4-ssh2 php7.4-common php7.4-json \
       php7.4-xml php7.4-dev php7.4-apcu php7.4-redis \
       libsmbclient-dev php-pear php-phpseclib
```

#Ahora ponemos este comando y escogemos el numero 2

```
update-alternatives --config php
```

#Ahora vamos a la carpeta de vagrant y hacemos vi

```
vi vagrantfile
```

#Y los modificamos

```
# Create a forwarded port mapping which allows access to a specific port
# within the machine from a port on the host machine. In the example below,
# accessing "localhost:8080" will access port 80 on the guest machine.
# NOTE: This will enable public access to the opened port
config.vm.network "forwarded_port", guest: 80, host: 8080
```

```
# Create a public network, which generally matched to bridged network.
# Bridged networks make the machine appear as another physical device on
# your network.
config.vm.network "public_network"
```

#Salimos

```
exit
```

#Apagamos

```
vagrant halt
```

#Encendemos

```
vagrant up --provider=virtualbox
```

#Y hacemos ssh

```
vagrant ssh
```

#Y tendria que salir nuestra ip

```
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-84-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Oct  9 10:45:10 UTC 2023

  System load:  0.0               Processes:               113
  Usage of /:   7.3% of 38.70GB   Users logged in:         0
  Memory usage: 58%               IPv4 address for enp0s3: 10.0.2.15
  Swap usage:   0%                IPv4 address for enp0s8: 192.168.18.125
```

