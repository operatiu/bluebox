---
title: 'Enlace simbólico y Linux Phun'

date:
  created: 2010-12-08

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - link
    - phun
    - softlink
---

A diferencia de lo que ocurre en entornos Windows, dónde un enlace simbólico podríamos asemejarlo a un “acceso directo” o lo que es lo mismo: Un destino que puede ser un directorio o un archivo, representado por un icono. En entornos GNU/Linux este adquiere un mayor potencial.

El enlace o link lo llamaremos mediante el comando ln y para utilizarlo como enlace simbólico el comando será ln -s, dicha llamada quedará completada con el destino o archivo al que queremos enlazar y con el nombre que utilizaremos para llamarlo.

```
ln -s /lib/destino.txt /home/llamaDestino
```

Veamos un ejemplo real**.** Supongamos que queremos utilizar el programa [Phun](http://www.phunland.com/wiki/Download) en Ubuntu 10.04. Simplemente descomprimiremos el archivo Phun\_beta\_5\_28\_linux32.tgz por ejemplo en el directorio Phun dentro de nuestro /home/user, pero al ejecutarlo es posible que se queje de que le falta alguna librería.

```
./phun<br></br>  There are missing dependencies.<br></br>  Please make sure that all the required libraries are installed.<br></br>  Missing:<br></br>    libpng.so.3 => not found 
```

Podemos buscarla y ver que tenemos disponible

$ **find / -iname “libpng\*.so\*” 2&gt;/dev/null**

```
/lib/libpng12.so.0<br></br>/lib/libpng12.so.0.42.0<br></br>/usr/lib/compiz/libpng.so 
```

Por lo que parece tenemos instalada una librería diferente, ya que nos pide **libpng.so.3** y tenemos disponible **libpng12.so.0** así que podemos probar a utilizar esta, cómo?

Creando un enlace simbólico a nuestra librería desde la que la aplicación phun va a llamar.

```
sudo ln -s /lib/libpng12.so.0 /lib/libpng.so.3  
```

Nota: En la versión 11.04 de Ubuntu el camino de las librerías ha cambiado, ahora no es **/lib/**  sino  **/lib/i386-linux-gnu/** con lo que quedaría de la siguiente forma:

```
sudo ln -s /lib/i386-linux-gnu/libpng12.so.0 /lib/i386-linux-gnu/libpng.so.3 
```

Ahora sólo queda probar el programa llamándolo de nuevo.

$ **./phun**

Y vemos que carga perfectamente

<a href="https://res.cloudinary.com/dbuv9r3p5/image/upload/phun_i23eeq.png" target="_blank">
  <img src="https://res.cloudinary.com/dbuv9r3p5/image/upload/w_300/phun_i23eeq.png" alt="phun">
</a>

salu2