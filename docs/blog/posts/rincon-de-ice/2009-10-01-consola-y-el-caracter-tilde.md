---
date:
  created: 2009-10-01

title: 'Consola y el carácter tilde ~'

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - Alt126
    - consola
    - linux
    - tilde
    - virgulilla
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/locales-desktop-locale-icon_tsrb6a.png "virgulilla")

Pues resulta que hace tiempo que buscaba algo tan simple como poder escribir el caracter "~" en una consola de linux y no había forma. 

Había instalado las _locales_ eso lo podemos consultar con el comando: **locale** 

En el caso en que queramos modificar, añadiendo o eliminando alguno, podemos hacer un reconfigure.

```
sudo dpkg-reconfigure locales
```

Lo más habitual será añadir es_ES@EURO o mejor aún  **es_ES.UTF-8** y elegir una por defecto.  

Por último deberiamos ejecutar console-data o console-setup según versión y distribución.

```
sudo dpkg-reconfigure console-setup  
```

Aún así, no hubo forma..

En sistemas basados en Windows, utilizaba Alt+126 y me funcionaba bien, pero no así en una consola de linux. 

La solución es tan fácil como utilizar "**AltGr+ñ**" o simplemente la tecla "**Av Pag**" Fácil eh !

salu2