---
date:
  created: 2010-12-08

title: 'Ubuntu 10.10 migración fallida'

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - migración
    - ubuntu
    - upgrade
---

Creo que es desde la versión 6.04 que he ido pasando versión tras versión en Ubuntu, actualizando sin sorpresas, pero en esta última 10.10 y después de llevar varios meses como estable intenté actualizar pero al final tuve que regresar a la 10.04

Lo primero que hice fue una actualización tradicional, cambiando el soporte de larga duración por el normal en las preferencias de actualización de orígenes de software, dentro del gestor de actualizaciones.

<a href="https://res.cloudinary.com/dbuv9r3p5/image/upload/ed-normales_cpm6zd.png" target="_blank">
  <img src="https://res.cloudinary.com/dbuv9r3p5/image/upload/w_300/ed-normales_cpm6zd.png" alt="updates">
</a>

Aparentemente todo había ido bien, bueno había que hacer algunos ajustes, pero de pronto noté algunos problemas, la mayoría relacionados con la gráfica, una Nvidia GeForce 8800 GT.
<!-- more -->

Ahora tengo una gráfica, GeForce GTS 450 de 1 GB, pero configurada a partir del driver propietario desde <http://www.nvidia.es/Download/index.aspx?lang=es> instalando previamente **build-essential** y **linux-headers** para tu kernel, por ejemplo con, **apt-get install linux-headers-$(uname -r)** y por último cerrando las ‘X’, **sudo service gdm stop** en el caso de gdm, luego accedemos a un terminal desde el que instalamos el paquete de drivers con un simple *sudo sh nvidiaDriver.run* parece que los drivers estaban dando problemas, pero con esto se acabaron.

Algún problema relacionado con K3B y la búsqueda de las unidades, etc.. pero el colmo fue cuando al cabo de cuatro o cinco horas con el equipo encendido y sin prácticamente ninguna aplicación corriendo, el rendimiento empezaba a bajar de forma alarmante, hasta el punto de que operaciones de segundos se transformaban en bastantes minutos…

El rendimiento general era pésimo, más pesado de lo normal en general y al cabo de las horas era como si el equipo estuviese exhausto, agotado.

Como Ubuntu lo tengo instalado utilizando todo un disco para el y de la forma siguiente:

- Sda1 --> /Boot
- Sda2 --> Swap
- Sda3--> /
- Sda4--> /home

Hice una instalación limpia de Ubuntu 10.10 (32 bits) seleccionando las mismas particiones para la misma finalidad y formateándo todo excepto la /home (bueno la swap si quieres que la cree de nuevo la tienes que eliminar y volver a repetir el procedimiento, aunque no es necesario). La ventaja de hacer esto es que si tu /home no la marcas para formatear y creas el mismo usuario con el mismo password, tu escritorio y tus datos apareceran tal cual, pero con una instalación nueva. Después sólo tienes que instalar aquellos paquetes que necesites de nuevo.

También existe la posibilidad de que hagamos una lista de los paquetes instalados previamente a la migración o reinstalación limpia:

[Como encontré en la siguiente fuente:](http://www.taringa.net/posts/linux/7331724/Instalar-Ubuntu-desde-cero-sin-perder-los-programas-instalad.html)

```
dpkg --get-selections | grep -v deinstall > totalPaquetes.txt
```

Nada más entrar, la primera vez que acabamos de instalar de forma limpia, hacemos un listado para obtener los paquetes iniciales de la instalación.

```
dpkg --get-selections | grep -v deinstall > inicioPaquetes.txt
```

Ahora podemos pedir la diferencia de ambas listas y sabremos que paquetes nos faltan:

```
comm -3 totalPaquetes.txt inicioPaquetes.txt > faltanPaquetes.txt
```

Para completar la instalación con los paquetes que nos faltan, agregaremos los repositorios extra, como medibuntu y algún otro que sepamos que necesitamos para algunos paquetes y haremos un **apt-get update**.

Ahora lo siguiente es:

```
sudo apt-get install dselect
dpkg --set-selections < faltanPaquetes.txt
sudo dselect install
```

En cualquier caso, yo opté por instalar a mano aquellos paquetes que necesitaba que no son tantos, pero con la instalación limpia Ubuntu 10.10 seguía perdiendo rendimiento con las horas, por tanto he vuelto a hacer lo mismo pero retrocediendo de nuevo a Lucid (Ubuntu 10.04) que funciona perfectamente bien en mi máquina. Creo que esperaré a la versión 11.04 para probar de nuevo que sucede.

salu2