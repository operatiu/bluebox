---
title: 'Elementary - Linux OS Luna'

date:
  created: 2014-06-20

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - elementary
    - luna
    - slingshot
    - theblueblox
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/w_150/Elementary.svg_fanwua.png "elementary")

**Elementary OS Luna**, es un sistema operativo GNU/Linux ligero basado en Ubuntu, con entorno de escritorio Pantheon, lanzador de aplicaciones SlingShot (integrado en el WingPanel o panel superior) y un zócalo de aplicaciones o Dock que es Plank. En conjunto presenta un aspecto sencillo y potente muy a lo MAC OS. Hasta aquí nada que no puedas encontrar en cientos de sitios. Pero lo que me interesa explicar hoy la solución adoptada para un problema con SlingShot.

El tema es fácil, **SlingShot** muestra a todos los usuarios todos los lanzadores de aplicaciones y como habrás imaginado ya, determinados usuarios no necesitan tener ciertos iconos de acceso a aplicaciones a su alcance. Como personalizo esto?

Parece ser que en versiones Beta de elementary OS bastaba con utilizar el editor de menú, que a estas alturas para poder acceder a el necesitas instalar ‘alacarte’ y por tanto un montón más de aplicaciones, dependencias, etc que quizás no te interesan, es más en la versión final ‘Main menu’ o editor de menú no cumple con su cometido o al menos no en muchos casos.

La solución que parece que funciona es crear un directorio ‘applications’ en la home del usuario que quieres que mantenga todas las applicaciones **~/.local/share/applications/** y mover allí los lanzadores de extensión .desktop que se encuentran en /usr/share/applications/ de esta forma eliminamos el lanzador o icono de aplicación de SlingShot que sirve como panel común a todos y sólo aquellos usuarios que queremos que si les aparezca dicho icono de acceso a la aplicación, serán aquellos que lo tienen en el directorio ‘applications’ de su home.

![](https://res.cloudinary.com/dbuv9r3p5/image/upload/w_420/e-shell_mgxagt.png "e-shell")

Estaría bien alguna herramienta de control de aplicaciones por usuario, pero de momento con esto parece que salvamos el problema.

![](https://res.cloudinary.com/dbuv9r3p5/image/upload/w_420/slingshot_bzzoqd.png "slingShot")