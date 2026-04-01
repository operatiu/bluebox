---
date:
  created: 2009-04-29

title: 'Conectando Nokia a Parrot'

author: jmong

layout: post

categories:
    - Antigues
tags:
    - Nokia
    - Parrot
    - teléfono
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/nokia-n96_f7wfvr.png "nokia")

Hace algún tiempo cambié mi móvil por un Nokia N96 con Simbian de 3ª generación y cual fue mi sorpresa al ver que no podía vincularlo al manos libres del coche, un Parrot CK3000 Evolution con firmware 5.07A. Vacié la memoria del parrot con la secuencia “-“, “+” cinco veces, pero nada. Aproveché una revisión del coche para actualizarlo por la última versión que había en ese momento, la 5.10C, pero seguía sin poder vincularlo :(Ni Parrot ni Nokia me daban una solución satisfactoria, así que había que buscarse la vida o cambiar uno de los dos dispositivos, cosa que no estaba dispuesto a hacer. 

**La solución fue fácil.**
<!-- more -->

Se trata de enviar nuestra lista de contactos de la agenda al manos libres por bluetooth de forma que sea Parrot el que nos “invite” a establecer la conexión y luego sólo tendremos que definir dicha conexión (ya establecida) como un dispositivo fiable, de forma que las sucesivas vinculaciones se hagan de forma automática y así se logre saltar la barrera de diferencia entre protocolos que mantienen diferentes niveles de seguridad y no hacen posible la vinculación de una forma “normal”.  

1.- Vamos a nuestra agenda de contactos o menú “GUIA”. [Imagen 1](http://img529.imageshack.us/img529/3236/screenshot0001ev7.jpg)

2.- Seleccionamos en Opciones, Marcar todo (con lo que quedarán todos los contactos seleccionados. [Imagen 2](http://img139.imageshack.us/img139/7381/screenshot0002wi1.jpg)

3.- Escogemos en Opciones, Enviar tarjeta de visita. [Imagen 3](http://img139.imageshack.us/img139/7463/screenshot0003vb9.jpg)

Se nos advertirá que prepara los contactos para ser enviados (cuestión de 3 o 4 segundos)

4.- Aquí diremos que el envío será **Vía Bluetooth**. [Imagen 4](http://img139.imageshack.us/img139/2435/screenshot0004qg9.jpg)

Ahora le decimos a nuestro **Nokia N96** que busque dispositivos y nada más encontrar a **Parrot V5.10C** lo seleccionamos. Para nuestra sorpresa veremos que aparece una solicitud del código para la vinculación: Introducimos: **1234** o el que tengáis. En cuestión de unos segundos se habrá realizado la transferencia de nuestros contactos, cosa que sabremos por la voz que nos lo avisa.

Como ya tendremos los **dispositivos vinculados**, no nos quedará mas que ir al menu Bluetooth de nuestro móvil y en la pestaña central “Dispositivos vinculados“, elegimos el Parrot y en Opciones le decimos que lo definimos como dispositivo de “confianza o autorizado” (creo recordar que es así como lo llama, sino es algo parecido), con lo que esto hará que la conexión cada vez que entremos al coche se realice de forma automática. [Imagen 5](http://img139.imageshack.us/img139/4175/screenshot0005mg9.jpg)

La única pega es que si actualizamos la agenda, para tenerla actualizada en el Parrot deberemos enviar los contactos de nuevo (pero son unos segundos). Además ahora cada vez que nos llamen saldrá por los altavoces el nombre de la persona que llama.  

salu2, espero que le sirva a alguien más ![😉](https://s.w.org/images/core/emoji/17.0.2/svg/1f609.svg)   

PD: Esto no se ha conseguido ni gracias a Parrot ni a Nokia ![🙁](https://s.w.org/images/core/emoji/17.0.2/svg/1f641.svg)