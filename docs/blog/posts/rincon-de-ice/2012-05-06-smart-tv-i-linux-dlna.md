---
date:
  created: 2012-05-06

title: 'Smart TV i Linux DLNA'

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - dlna
    - tv
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/TV-Set-Retro-icon_a4lhay.png "dlna")

Cada vez más, nos vamos acercando a una nueva forma de ver la televisión y esta como dispositivo físico está empezando a evolucionar, ya no sólo cuenta con conexiones HDMI o con resoluciones de 1080, sino que además incorporan wifi y pequeñas aplicaciones integradas de acceso a la televisión a la carta, que por que no decirlo, es una buena opción que incorporan las cadenas televisivas pero que si puedes verla desde el sofá siempre será más cómodo que desde el pc.
<!-- more -->

A partir de aquí nos encontramos con sistemas como el que ofrece LG o Samsung en sus Smart TV, (así como muchísimas más compañías), el sistema de compartición de contenidos DLNA (Digital Living Network Alliance)basado en parte, en el protocolo UPnP que nos permite hacer streaming vía wifi en nuestra red doméstica de aquellos contenidos multimedia almacenados en el pc, tablet o el móvil, hacia la TV.

Una de las aplicaciones que nos permiten conectar o acceder a estos sistemas es AllShare, en el caso de Samsung, que además ofrece un programa gratuito para realizar dicho streaming, lamentablemente sólo funciona en sistemas basados en Windows, así que estuve probando algunos media servers que fuesen libres y funcionasen en sistemas GNU/Linux, como [serviio](http://www.serviio.org/), [MediaTomb](http://mediatomb.cc/) i [Minidlna](http://sourceforge.net/projects/minidlna/).

Al final, este último fue el que me dio mejores resultados, en mi caso. Tan sólo hay que configurar el archivo /etc/minidlna.conf del que dejo mi archivo de configuración.

```
\# port for HTTP (descriptions, SOAP, media transfer) traffic  port=8200  
\# network interfaces to serve, comma delimited  
\#network\_interface=eth0  
\# set this to the directory you want scanned.  
\# \* if have multiple directories, you can have multiple media\_dir= lines  
\# \* if you want to restrict a media\_dir to a specific content type, you  
\# can prepend the type, followed by a comma, to the directory:  
\# + “A” for audio (eg. media\_dir=A,/home/jmaggard/Music)  
\# + “V” for video (eg. media\_dir=V,/home/jmaggard/Videos)  
\# + “P” for images (eg. media\_dir=P,/home/jmaggard/Pictures)  
\# Use A, P, and V to restrict media ‘type’ in directory  
media\_dir=A,/media/compartida/Musica  
media\_dir=P,/media/compartida/Imagen  
media\_dir=V,/media/compartida/Videos  
\# set this if you want to customize the name that shows up on your clients  
friendly\_name=LinuxServer

\# set this if you would like to specify the directory where you want MiniDLNA to store its database and album art cache  
db\_dir=/var/cache/minidlna  
\# set this if you would like to specify the directory where you want MiniDLNA to store its log file  
log\_dir=/var/log

\# set this to change the verbosity of the information that is logged  
\# each section can use a different level: off, fatal, error, warn, info, or debug  
\#log\_level=general,artwork,database,inotify,scanner,metadata,http,ssdp,tivo=warn

\# this should be a list of file names to check for when searching for album art  
\# note: names should be delimited with a forward slash (“/”)  
album\_art\_names=Cover.jpg/cover.jpg/AlbumArtSmall.jpg/albumartsmall.jpg/AlbumArt.jpg/albumart.jpg/Album.jpg/album.jpg/Folder.jpg/folder.jpg/Thumb.jpg/thumb.jpg

\# set this to no to disable inotify monitoring to automatically discover new files  
\# note: the default is yes  
inotify=yes

\# set this to yes to enable support for streaming .jpg and .mp3 files to a TiVo supporting HMO  
enable\_tivo=no

\# set this to strictly adhere to DLNA standards.  
\# \* This will allow server-side downscaling of very large JPEG images,  
\# which may hurt JPEG serving performance on (at least) Sony DLNA products.  
strict\_dlna=no

\# default presentation url is http address on port 80  
\#presentation\_url=http://www.mylan/index.php  
presentation\_url=http://localhost:48200

\# notify interval in seconds. default is 895 seconds.  
notify\_interval=900

\# serial and model number the daemon will report to clients  
\# in its XML description  
serial=12345678  
model\_number=1

\# specify the path to the MiniSSDPd socket  
\#minissdpdsocket=/var/run/minissdpd.sock

\# use different container as root of the tree  
\# possible values:  
\# + “.” – use standard container (this is the default)  
\# + “B” – “Browse Directory”  
\# + “M” – “Music”  
\# + “V” – “Video”  
\# + “P” – “Pictures”  
\# if you specify “B” and client device is audio-only then “Music/Folders” will be used as root  
\#root\_container=.
```

Sólo debemos tener cuidado a nivel de permisos con las carpetas y todo debería funcionar sin problemas. Para arrancarlo como servició, tenemos un script que deberíamos añadir y que lo obtenemos de la misma página del proyecto.

```
\#!/bin/sh  
\# chkconfig: 345 99 10  
\# description: Startup/shutdown script for MiniDLNA daemon  
\#  
\# Based on the MiniUPnPd script by Thomas Bernard  
\# Modified for MiniDLNA by Justin Maggard  
\# Status function added by Igor Drobot  
\#  
\### BEGIN INIT INFO  
\# Provides: minidlna  
\# Required-Start: $network $local\_fs $remote\_fs  
\# Required-Stop:: $network $local\_fs $remote\_fs  
\# Should-Start: $all  
\# Should-Stop: $all  
\# Default-Start: 2 3 4 5  
\# Default-Stop: 0 1 6  
\# Short-Description: DLNA/UPnP-AV media server  
\### END INIT INFO

MINIDLNA=/usr/sbin/minidlna  
PIDFILE=/var/run/minidlna.pid  
CONF=/etc/minidlna.conf  
ARGS=”-f $CONF”

test -f $MINIDLNA || exit 0

. /lib/lsb/init-functions

case “$1” in  
start) log\_daemon\_msg “Starting minidlna” “minidlna”  
 start-stop-daemon –start –quiet –pidfile $PIDFILE –startas $MINIDLNA — $ARGS $LSBNAMES  
 log\_end\_msg $?  
 ;;  
stop) log\_daemon\_msg “Stopping minidlna” “minidlna”  
 start-stop-daemon –stop –quiet –pidfile $PIDFILE  
 log\_end\_msg $?  
 ;;  
restart|reload|force-reload)  
 log\_daemon\_msg “Restarting minidlna” “minidlna”  
 start-stop-daemon –stop –retry 5 –quiet –pidfile $PIDFILE  
 start-stop-daemon –start –quiet –pidfile $PIDFILE –startas $MINIDLNA — $ARGS $LSBNAMES  
 log\_end\_msg $?  
 ;;  
status)  
 status\_of\_proc -p $PIDFILE $MINIDLNA minidlna &amp;&amp; exit 0 || exit $?  
 ;;  
\*) log\_action\_msg “Usage: /etc/init.d/minidlna {start|stop|restart|reload|force-reload|status}”  
 exit 2  
 ;;  
esac  
exit 0
```

Con esta versión además incluimos la opción de consulta de estado o status.  
Para añadirlo manualmente como servicio, sólo debemos incluirlo en **/etc/init.d/** y darle permisos.

`sudo chmod a+x /etc/init.d/minidlna`

Ahora hacemos que se funcione en los diferentes niveles de ejecución:

`sudo update-rc.d minidlna defaults`

La linea anterior también podría contener:

**enable | disable | remove**, para activar, desactivar o eliminar dicha ejecución del servicio.  
  
Para activar el servidor utilizaremos:

```
sudo service minidlna start
```
Siendo stop | restart | status otras de las opciones disponibles.

Y por último para actualizar la base de datos de contenidos, al añadir nuevos elementos multimedia:

```
sudo /usr/sbin/minidlna -f /etc/minidlna.conf -R -d
```

saludos