---
date: 
    created: 2018-08-31
    
title: 'Ubuntu 18.04 arranque dual y modo seguro'

author: jmong

layout: post

categories:
    - Antigues
tags:
    - bios
    - hp
    - ideapad
    - lenovo
    - mok
    - secure
    - theblueblox
    - uefi
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/secrecy-icon_b6jqe7.png "boot")

Hace unos días tuve que instalar Ubuntu en un portátil nuevo (HP 15-DA0113NS) que venía con un Windows 10.

Para aprovechar todo el potencial, redimensioné la partición de Windows con Gparted utilizando un live multisistema o un (gparted live simplemente, pero de los últimos). A MS Windows lo dejé con unos 100GB y el resto del disco SSD de 256 (excepto las particiones de recuperación y demás) lo dejé libre para Linux, utilizando el sistema de particionado GPT.
<!-- more -->

El problema empieza con la BIOS ya que cada fabricante utiliza su propia nomenclatura y a veces es algo confusa. Lo que en equipos como LENOVO Ideapad (acceso a BIOS utilizando clip en NOVO buton o hendidura al lado del conector de auriculares) se diferencia como **UEFI / Legacy Boot** en los últimos portátiles de HP lo encontraremos como **Secure Mode** que por defecto viene habilitado.

Para acceder a sistemas como WINDOWS 8 y 10+ necesitamos que esté así, pero para acceder a sistemas anteriores de MS o GNU Linux (incluido el arranque a la mayoria de LIVE USB) tendremos que deshabilitar-lo. No obstante si queremos utilizar este modo (y a la larga acabará siendo así), para instalar otro SO como Linux deberemos utilizar alguna distro que soporte este modo, como por ejemplo Ubuntu 18.04.

Una vez tengamos la ISO arrancable en un USB con F9 accedemos a cambiar el boot mode y procedemos a la instalación. Ubuntu nos pedirá una contraseña de 8 dígitos que nos inventaremos para verificar este modo en caso necesario cuando en la BIOS nos aparezca el acceso MOK de Secure Mode para que el Kernel utilice drivers propietarios o que nosotros hallamos compilado por nuestra cuenta (por ejemplo el caso de drivers propietarios de NVIDIA, etc..)

```
sudo mokutil --sb-state
```

```
SecureBoot enabled
```

Una vez instalado el sistema accederemos a la BIOS (F10) y en el menú Configuración del sistema / Opciones de arranque, podemos dejar desactivada la compatibilidad heredada y el Arranque seguro = Activado, pero para que el arranque UEFI utilice Grub y desde allí podamos elegir Windows o Ubuntu, deberemos ir a la opción Administrador de arranque del SO y pulsar Enter, de esta forma nos aparecerá además de la línea del Windows Boot Manager, una nueva apuntando al mismo disco pero con el nombre de ubuntu o la distro utilizada. Tan sólo la desplazaremos al principio utilizando las teclas (F5 o F6) y guardamos la opción con F10. Volvemos a pulsar F10 y reiniciaremos salvando los cambios.

Ya tendremos acceso a Grub y a los dos SO con el modo seguro activado. Si queremos poder arrancar en modo seguro con algun live sin pasar de nuevo por la BIOS podemos dejar en primer lugar la opción:

- Disquete USB en clave/Unidad de disco duro USB

y a continuación …

- Administrador de arranque del SO