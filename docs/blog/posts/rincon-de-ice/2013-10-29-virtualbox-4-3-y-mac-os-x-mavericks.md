---
title: 'VirtualBox 4.3 y MAC OS X Mavericks'

date:
  created: 2013-10-29

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - 'ExtensionPack'
    - Mavericks
    - theblueblox
    - VBox
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/X-Circle-Green-icon_n39uto.png "mavericks")

Esta vez va del error de kernel que devuelve VirtualBox 4.2.18, al ejecutarse sobre la nueva versión de OS X, Mavericks.

Al principio parecía que con sólo instalar la versión 4.3.0 de VirtualBox ya estaba todo solucionado, pero lo cierto es que al cerrar y volver a abrir VBox cualquier màquina virtual que no usara NAT, por ejemplo las configuradas con Adaptador puente o Bridge, volvían a dar el mensaje de error y no se dejaban abrir.
<!-- more -->

La solución ha sido simple, se trata de eliminar los archivos que empiezan con el nombre VBox… y que se encuentran en el directorio: **/Library/Extensions/** estos son:

**VBoxDrv.kext, VBoxNetAdp.kext, VBoxNextFlt.kext y VBoxUSB.kext** (aunque es posible que tengas alguno más o menos).

Primero eliminamos VirtualBox, entrando en el directorio Aplicaciones y enviandolo a la papelera.

Segundo eliminamos los archivos antes mencionados, desde Finder o desde el terminal.

```
# rm /Lybrary/Extensions/VBox*   
```

Ahora sólo hay que instalar VirtualBox 4.3.0 o superior (si ya ha salido), después puedes instalar las VirtualBox Extension Pack para la versión 4.3.0 o la que has instalado de VirtualBox y con esto debería desaparecer el problema. Volverás a poder acceder a todas tus màquinas virtuales sin problemas y con nuevas funcionalidades.

Si te aparece algunas advertencia, como por ejemplo la de profundidad de color (32 bits) puedes omitirla si no quieres verla cada vez. Esto pasa con sistemas a los que has dado suficiente memoria gràfica pero no lo tienes configurado para utilizarla, por ejemplo un Debian o un server de cualquier distribución, sin pretensiones gráficas que quizás no necesitas y no te has molestado en configurar. Sin problemas.

saludos