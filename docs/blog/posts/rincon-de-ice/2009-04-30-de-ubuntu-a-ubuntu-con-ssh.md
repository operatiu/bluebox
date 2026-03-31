---
date:
  created: 2009-04-30

title: 'De Ubuntu a Ubuntu con SSH'

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - compartir
    - conexión
    - openssh
    - servidor
    - share
    - SSH
    - ubuntu
---
![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/ssh-icon_kfahra.png "ssh")

Una tarea típica con la que nos solemos encontrar es la de acceder de un equipo a otro (dentro de una red local (Lan) o una red externa (Wan), sea por cable o por wifi) para acceder a determinados archivos, por poner un ejemplo.

Supongamos que en casa tenemos un PC de sobremesa con GNU/Linux (por ejemplo Ubuntu) y un portátil también con Ubuntu. Si queremos acceder desde el portátil (con dirección IP: 192.168.1.3) al PC de sobremesa (con dirección IP: 192.168.1.2), una manera sencilla es la siguiente: En el PC de sobremesa comprobaremos si tenemos instalado openssh-server, en caso contrario lo instalaremos de la forma habitual, ya sea desde cónsola con la órden:
<!-- more -->

`# apt-get install openssh-server`

o utilizando algún gestor de paquetes como aptitude o synaptic.

Seguidamente desde el portátil iremos al menu Lugares / Conectar con el servidor y allí seleccionaremos la opción SSH, más abajo indicaremos la dirección IP de nuestro PC de sobremesa y inmediatamente nos pedirá que nos identifiquemos con nuestro usuario y contraseña, si todo es correcto ya estaremos dentro.

Otra forma, desde el portátil, sería abrir una ventana del explorador de archivos, por ejemplo Nautilus y escribir en la barra de direcciones lo siguiente:

sftp://IPdelPCdeSobremesa  o lo que es lo mismo:  sftp://192.168.1.2

Haríamos el login y ya está.