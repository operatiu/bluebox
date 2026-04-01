---
date: 
    created: 2018-08-31

title: 'Ubuntu y driver wifi'

author: jmong

layout: post

categories:
    - Antigues
tags:
    - modprobe
    - module
    - mok
    - rtl8822be
    - secure
    - secureboot
    - theblueblox
    - ukuu
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/Wifi-1-icon_iqx26x.png "wifi")

### Ubuntu 18.04 y driver RTL8822BE

Después de la instalación de Ubuntu 18.06 64 bits en modo seguro, resultó que el sistema no reconocía la WIFI. Maticemos, no reconocía el hardware que incorpora el portátil (un HP 15-DA0113NS) i por tanto no podía configurar el acceso de red usando la wifi.

Primero debemos saber que chipset estamos utilizando. Para ello podemos acceder a la web del fabricante i consultar las características de nuestro modelo de portátil, aunque en este caso HP no proporciona esa información, al menos directamente. Otra opción es entrar en Windows i consultarla en las propiedades del dispositivo y la más fácil, escribir alguna órden que me devuelva esa información, como **dmesg** o **lshw** entre otras.
<!-- more -->

```
dmesg | grep -i wire  
sudo lshw -C net | grep -i wire  
lspci -k -nn | grep -A 3 -i net  
```
etc…

Podemos utilizar los comandos **iwconfig** o **ifconfig -a**  
Es posible que para usar ifconfig tengas que instalar las **net-tools** según la versión de Ubuntu que utilices, por estar obsoleto (deprecated).

También nos conviene conocer que kernel estamos usando.

**uname -a**

Después de consultar muchas fuentes, las opciones apuntaban a la instalación del paquete de módulos **rtl** que contiene los drivers que nos interesan.

Opción 1.- (Aunque no funciona con el modo seguro activo en Bios)

```
git clone https://github.com/lwfinger/rtlwifi_new.git
cd rtlwifi_new/
git checkout origin/extended -b extended
make
sudo make install
```

Opción 2.- Usar dkms para la instalación de los módulos

```
sudo apt-get install git build-essential dkms linux-headers-$(uname -r)
git clone -b extended https://github.com/lwfinger/rtlwifi_new.git
sudo dkms add ./rtlwifi_new
sudo dkms install rtlwifi-new/0.6
sudo cp /usr/src/rtlwifi-new-0.6/firmware/rtlwifi/* /lib/firmware/rtlwifi/
```

Opción 3.- Actualizar el kernel

Después de los dos intentos anteriores y consultar el ultimo kernel stable en <https://www.kernel.org/> **4.18.5** y comprobar que era superior al que yo estaba utilizando **4.15.0-33-generic** decidí probarlo usando **ukuu**, una herramienta que facilita bastante la instalación de uno u otro kernel en Linux.

```
sudo apt-add-repository -y ppa:teejee2008/ppa<br></br>sudo apt-get update<br></br>sudo apt-get install ukuu
```

En modo gráfico:

**ukuu-gtk &amp;**

En modo terminal:

**ukuu –list**

Deshabilitamos posibles conexiones remotas temporalmente  
**xhost +**

Ahora instalamos el kernel elegido, en nuestro caso el que aparece como: v4.18.5  
**sudo ukuu –install v.4.18.5**

Al finalizar deberemos reiniciar, pero atención porque al estar en Secure Mode, se nos pedirá una contraseña para introducirla en el siguiente reinicio y asegurarnos así de que somos nosotros y no un programa externo el que quiere utilizar un módulo y un kernel diferente.

- sudo init 6
- Opción: MOK
- Enrolled

Introducimos el password que definimos antes de reiniciar y continuamos el arranque, ahora ya con el nuevo kernel.


Sitios consultados:

[Ubuntu fòrums](https://forum.ubuntu-fr.org/viewtopic.php?id=2027300)

[Ubuntu fòrums](https://ubuntuforums.org/showthread.php?t=2383612&page=2&p=13734908#post13734908)

[Ubuntu archive](http://de.archive.ubuntu.com/ubuntu/pool/)

<a href="https://www.kernel.org/">https://www.kernel.org/</a><br></br><a href="https://ubuntuforums.org/showthread.php?t=2392454">https://ubuntuforums.org/showthread.php?t=2392454</a><br></br><a href="https://linuxhint.com/how-to-upgrade-the-kernel-on-ubuntu-17-10/">https://linuxhint.com/how-to-upgrade-the-kernel-on-ubuntu-17-10/</a>
