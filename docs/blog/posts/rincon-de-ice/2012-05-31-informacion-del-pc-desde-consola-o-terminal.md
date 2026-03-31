---
title: 'Información del PC desde consola o terminal'

date:
  created: 2012-05-31

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - df
    - du
    - lshw
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/Terminal-icon_kqgazm.png "shell")

Existen una serie de comandos muy interesantes y a veces poco conocidos para obtener información de nuestro equipo, directamente desde la línea de órdenes. Para empezar abrimos un terminal Ctrl+T.
<!-- more -->

- **curl ifconfig.me**

Utilizando esta secuencia del paquete curl, obtendremos nuestra **IP pública** sin necesidad de acceder a páginas como [www.cualesmiip.es](http://www.cualesmiip.es/) o [www.whatismyip.com](http://www.whatismyip.com/)

- **df**

Para conocer el espacio en disco en cada partición, mejor con la opción para seres humanos: **df -h**. o podemos indicar también el disco o la partición: df -h /dev/sda1 etc.. También podemos utilizar un **fdisk -l** pero con permisos de superusuario.  
  
Un equivalente gráfico podría ser **gparted**.

- **du**

Si lo que queremos es conocer la capacidad de un directorio o carpeta podemos utilizar **du -sh** **/path/carpeta** , donde (s) indica el total i (h) indica como en el caso anterior la opción de magnitud para seres humanos, osea {.. Kb, Mb, Gb ..}.

- **free**

Devuelve la cantidad de memoria del sistema, la utilizada y la libre o disponible.

- **lscpu**

Información relativa a nuestro procesador o procesadores.

- **import**

Con **import nomArchivo.ext** podemos obtener una captura de pantalla, directamente desde el escritorio, por ejemplo: **import captura.png**. Esta es la forma básica, pero existen más opciones que puedes consultar en man.

- **lspci**

Información relativa a los dispositivos PCI de la máquina.

- **lsusb**

Para los puertos USB y los dispositivos conectados a ellos.

- **lshw**

Para un listado completo de nuestro hardware, mejor aún si podemos ejecutarlo con privilegios de administrador, ya que la información será más completa. Tenemos además la opción de generar un archivo .xml o .html entre otros, por ejemplo utilizando la sentencia de la siguiente forma: **sudo lshw -html &gt; informe.html** o pedir un resumen de características con: **lshw -short**.  
  
Algunos equivalentes gráficos serian **sysinfo** y **hardinfo**.   
  
Aprovechando que hemos generado un archivo .html, cómo podemos ejecutarlo con su aplicación asociada desde el terminal? Pues con la órden, **gnome-open** o **kde-open**, según tengamos instalado uno u otro escritorio (o ambos).   
  
Por ejemplo, para el caso anterior sería: **gnome-open informe.html &amp;**

- **top**

Muestra los procesos que se están utilizando, así como los recursos que utilizan sobre el uso de la cpu y la memoria ram, entre otros.

- **script**

A menudo puede resultar interesante guardar todo el historial de una sesión de terminal o al menos decidir cuando empezar a guardar y cuando acabar, para eso tenemos script:**script nombrearchivo** esto empezará a guardar todo lo que hagamos en el archivo indicado y acabará de hacerlo cuando lo indiquemos con **exit**. Cuando queramos ver que habíamos hecho, editamos el archivo (nano nombrearchivo) y listo.   
Atención por que hacer un **cat** del archivo puede no listarnos su contenido real.

Localizar archivos: **find**

Tenemos muchas opciones en función de cómo queramos afinar en nuestra búsqueda y por tanto, necesitaríamos un artículo entero para dicho comando. En su forma más simple, encontrar un archivo o carpeta determinada, podemos hacerlo como:

```
find ruta -name elementoAbuscar
```

aunque seguramente si salimos del dominio del usuario obtendremos un montón de advertencias, por lo que podemos combinarlo con sudo:  
**sudo find / -name vlc**

- **locate**
Localizar archivos:  Con locate obtendremos una rápida y potente búsqueda y sin necesidad de escalar privilegios: **locate vlc**

Necesitaremos instalar **mlocate** que genera una base de datos indexada */var/lib/mlocate/mlocate.db* con toda la información del sistema y que por defecto se actualiza cada 24h aproximadamente mediante un cron, pero de la que podemos forzar una actualización en cualquier momento con **sudo updatedb**

El archivo de configuración se encuentra en: **/etc/updatedb.conf**

Localizar la ruta de un ejecutable: **which**

Siguiendo con el ejemplo de vlc: **which vlc**  
*/usr/bin/vlc*

- **xkill**

Eliminar un proceso de forma gráfica, simplemente señalando la ventana de ejecución (por ejemplo de Firefox o Nautilus).