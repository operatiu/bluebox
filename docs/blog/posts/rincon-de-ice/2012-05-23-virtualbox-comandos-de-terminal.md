---
date:
  created: 2012-05-23

title: 'VirtualBox, comandos de terminal'

author: jmong

layout: post

categories:
    - Antigues
tags:
    - terminal
    - VirtualBox
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/VirtualBox-icon_ant9gc.png "VirtualBox")

Es habitual tener instalado VirtualBox en alguna distribución GNU/Linux, por ejemplo Ubuntu o Debian, que son las que suelo utilizar y necesitar hacer alguna cosa utilizando un terminal. Para ello tenemos diferentes comandos que pueden ser útiles:
<!-- more -->

**1.- <u>vboxdrv</u>**

A veces sucede que después de una actualización del kernel, pueda saltar un error que imposibilita el acceso a nuestra máquina virtual. Si tenemos el módulo “dkms” instalado tan sólo necesitaremos abrir un terminal (Ctrl+Alt T) y ejecutar:

```
sudo /etc/init.d/vboxdrv setup
```

**2.- <u>VBoxManage</u>**

Con VBoxManage vamos a ver un par de comandos útiles.

a) Clonar máquina virtual:

Abrimos terminal y ejecutamos:

```
VBoxManage clonevdi /pathOrigen/nomMáquinaOrigen.vdi /pathDestino/nomMáquinaDestino.vdi
```

b) Iniciar máquina desde terminal:

```
VBoxManage startvm “nomMáquina”
```

También podemos crear un pequeño script o un simple lanzador que se ejecute al inicio y entraríamos directamente desde una sesión a un máquina virtual determinada. Si además queremos que el terminal se cierre, añadiremos al final de la comanda: **&amp;&amp; exit** y listo.

c) Cambiar UUID del disco .vdi de la máquina virtual

```
VBoxManage internalcommands sethduuid “nomDisc.vdi”
```

Al ser VirtualBox multiplataforma, esto lo podemos hacer desde cualquier sistema.

En un MAC, por ejemplo, podemos acceder a su carpeta:

```
cd /Applications/VirtualBox.app/Contents/MacOS
```

Y desde aquí ver los diferentes paquetes de que disponemos, además de VBoxManage, aunque no es obligatorio estar en dicha ruta.

También podemos ejecutar las órdenes desde aquí o incluso ver que opciones tenemos con cada una, por ejemplo con VBoxManage:

```
VBoxManage --help
```

Nos mostrará las diferentes opciones disponibles y algunos ejemplos a seguir o a tener en cuenta.