---
date:
  created: 2012-05-06

title: 'Clonación de disco a unidad externa'

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - clonar
    - image
---

### Imagen de disco

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/HD-OpenDrive-Golden-icon_rmjwfm.png 'dd')

Una utilidad muy potente que nos permite hacer imágenes de disco o CD/DVD es **dd**, pero además nos permite hacer una copia idéntica y funcional de nuestro sistema en un disco externo, del que además podemos arrancar y trabajar como si en nuestro propio pc lo estuviésemos haciendo.

Tan sólo debemos utilizar un: **\# fdisk -l** para determinar que unidad queremos clonar o unidad origen (**if**) y unidad destino (**of**)

El comando es el siguiente:
<!-- more -->

```
sudo dd if=/dev/sda of=/dev/sdh bs=4096 conv=notrunc,noerror
```

El problema de **dd** es que no nos mostrará información hasta que acabe la copia y eso puede acabar con la paciencia de más de uno, así que podemos hacer uso del paquete pv (si no lo tenemos instalado **dpkg -l |grep pv** ) lo instalamos  **sudo apt-get install -f pv**  y ahora podemos crear una pipe ( **|** ) dentro del comando anterior para que nos informe del progreso de copia.  
  
```
sudo dd if=/dev/sda |pv|dd of=/dev/sdh bs=4096 conv=notrunc,noerror
```

Dependiendo del tamaño de disco y la velocidad, el tiempo puede ser considerable, por ejemplo un disco de 500 GB con cuatro particiones clonado a otro disco de 500 GB USB 2.0 tardó unas cuatro horas, en mi máquina.

saludos