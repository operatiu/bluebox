---
date:
  created: 2010-03-29

title: 'Ubuntu 9.10 y Java-plugin 1.6'

author: jmong

layout: post

categories:
    - Antigues
tags:
    - java
    - plugin
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/Ubuntu_9.10_zlig5c.png "Ubuntu_9.10")

Hola, si has llegado hasta aquí posiblemente te suceda lo que a mi me pasó con java. Resulta que con la última actualización de java la 1.6.0.15 dejé de ver las páginas web que cargaban algún applet de java, como por ejemplo esta: [calculadora wiris](http://cv.uoc.edu/webapps/calculadora/es/index.html)

Buscando información por la red, di con lo que parecía la solución, que consistía en crear un enlace simbólico a la carpeta plugins del navegador (en mi caso firefox) apuntando a la ubicación del nuevo archivo del plugin de java, osea ejecutamos en una consola lo siguiente (asegurándonos primero que las rutas y versiones son las correctas):
<!-- more -->


```
cd /usr/lib/firefox-3.6/pluginssudo ln -s /usr/lib/jvm/java-6-sun-1.6.0.15/jre/lib/i386/libnpjp2.so 
```

Pues bien, todo parecía estar correctamente, pero el plugin no mostraba lo que debía mostrar. Desde la barra de direcciones de firefox, tecleando: **about\:plugins** podemos ver que el java-plugin está cargado y funcionando. ¿Qué pasaba entonces?

Pues nada más sencillo. Tiempo atrás tuve que modificar el archivo **/etc/environment** y añadir la siguiente línea:

 **AWT\_TOOLKIT=”MToolkit”**

Esto solucionaba el refresco y la presentación correcta del plugin de java cuando compiz estaba activo y ahora era lo que precisamente me ocasionaba los problemas. Por tanto la solución está en **quitar o comentar dicha línea** y listos.

Tan simple y la de vueltas que he tenido que dar.

Espero que le sirva a alguien más.

salu2