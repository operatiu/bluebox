---
title: 'Televisión en Linux'

date:
  created: 2009-04-21

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - hauppauge
    - sintonizadora
    - tv
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/television-08-icon_ge0epg.png "TV")
Hace algún tiempo instalé una sintonizadora de video en mi PC, concretamente una [Hauppauge HVR-4000](https://hauppauge.co.uk/spain/site/products/data_hvr4400.html), pero hasta hace unos días me había sido imposible hacerla funcionar en Ubuntu 8.10 o en versiones anteriores. Era un espinita clavada que se me resistía y os aseguro que había probado de todo, incluso el driver experimental v4L, hasta que di con esta dirección: <!-- more -->

<http://www.linlap.com/wiki/installing+the+latest+v4l+tv+tuner+drivers+for+ubuntu+8.10>

y volví a intentarlo.
<!-- more -->

Fue tan sencillo como situarme en **cd /usr/src/** y renombrar la carpeta v4l-dvb (por si las fly’s, en lugar de eliminarla) para generarla de nuevo con el contenido actualizado del driver:

**sudo hg clone http://linuxtv.org/hg/v4l-dvb**

Entré en la carpeta o directorio v4l-dvb, ya que estaba justo en el directorio anterior /usr/src por tanto sólo hacemos **cd v4l-dvb** suena redundante, pero no quiero que tengas dudas al leerlo 😉

Después fue tan sencillo como hacer un: **sudo make** y esperar un rato a que todo se compilase de forma correcta. Que emocionante, esta vez tenía la esperanza de que todo iba a salir bien.

Nada más acabar sólo quedaba hacer un: **sudo make install** y listo para empezar.. Reinicié el equipo y seguidamente instalé **kaffeine**, con grata sorpresa de que me había reconocido mi sintonizadora y me daba la opción de configurar el satélite y el TDT 🙂 Ahora ya puedo (si quiero) ver la TV desde GNU/Linux.

En Ubuntu 9.04 parece ser que no funciona este método (de momento), así que para más información podemos consultar en: [linuxtv.org](http://www.linuxtv.org/wiki/index.php/Hauppauge_WinTV-HVR-4000)

Bueno, después de unas horas dándole vueltas y probando distintos firmwares para ver porqué en Ubuntu 9.04 (kernel 2.6.28) no me aparecía la TV digital o TDT y sólo aparecía la opción de Televisión vía satélite, parece ser que no soporta las dos al mismo tiempo ya que en /dev/dvb/ sólo está la carpeta adapter0 que contiene frontend0, net0, dvr0 y demux0 y que evidentemente están apuntando al TV SAT en lugar del TV TDT que es el que me interesa, así que se trata de crear enlaces símbólicos que apunten al segundo.

mkdir /dev/dvb/adapter1ln -s /dev/dvb/adapter0/frontend1 /dev/dvb/adapter1/frontend0ln -s /dev/dvb/adapter0/net1 /dev/dvb/adapter1/net0ln -s /dev/dvb/adapter0/dvr1 /dev/dvb/adapter1/dvr0ln -s /dev/dvb/adapter0/demux1 /dev/dvb/adapter1/demux0

De esta forma podremos elegir la opción de TDT del módulo dvb, el problema es que o bien lo hacemos cada vez que iniciemos sesión o tendremos que hacer algún script que lo cargue al inicio.

El problema que tengo ahora, es que en mi escritorio,que es Gnome, utilizo Kaffeine para sintonizar la TV y kaffeine es el reproductor de KDE que utiliza Xine como motor de audio y video, al contrario que su homólogo en Gnome, que es Totem y utiliza Gstreamer, total que de momento con Kaffeine no tengo audio. Supongo que será una cuestión de codecs o librerias, todo se andará…

salu2