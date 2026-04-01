---
date: 
    created: 2021-10-23

title: 'NAS WD My cloud, firmware error update OS 3 to OS 5'

author: jmong

layout: post

categories:
    - Antigues
tags:
    - actualización
    - backup
    - mycloud
    - NAS
    - os3
    - os5
    - theblueblox
    - update
    - upgrade
    - WD
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/NAS_WD_My_Cloud_4TB_upcwxx.png "NAS")

En la última actualización del NAS Western Digital (WD) My Cloud, en un dispositivo de bahía única de 4TB se corrompió el sistema…

WD My Cloud, a partir de la versión **2.41.116** del firmware (OS 3) para versiones 2 de sus dispositivos NAS WD My Cloud (son los que el product number acaba de esta forma (P/N: **WD**xxxxxxxx**HWT – 10**), actualiza a su versión OS 5.

Resulta que de OS 3 firmware 2.41.116 había recomendación de actualizar a OS 5 y la actualización en línea se quedaba a medias, así que lo [descargué](https://support-en.wd.com/app/products/product-detail/p/126#WD_downloads) y instalé manualmente des del panel web de control (al que accedes mediante la url o IP del NAS). Una vez instalado y reiniciado el sistema, tendrás que configurar de nuevo ciertas cosas, como la contraseñas que no cumplan la nueva política o el acceso de contenidos a la nube, etc.. Te recomiendo que antes y después de la actualización hagas copia de seguridad del archivo de configuración y lo referencies con su firmware, en cada caso.

Después de unos días con el nuevo firmware 5.16.105 aparece un nuevo aviso de actualización a la 5.17.107

<a href="https://res.cloudinary.com/dbuv9r3p5/image/upload/firmware_update_hedqht.png" target="_blank">
  <img src="https://res.cloudinary.com/dbuv9r3p5/image/upload/w_400/firmware_update_hedqht.png">
</a>

Al pinchar en actualizar ahora, se inicia el proceso de actualización bajando el archivo binario del firmware y instalando en el NAS. La descarga se realiza rápida y correctamente pero no así la instalación, que tras bastantes minutos de espera y viendo que la luz azul del indicador led frontal del NAS no parpadeaba y que des del panel de control tampoco se podía hacer nada, decidí reiniciarlo manualmente (desconectar y volver a conectar la alimentación) aunque las indicaciones en pantalla eran, no desconecte el producto de la red mientras esté actualizando… si pero es que se había quedado bloqueado, qué podía hacer..?

Otras veces no había pasado nada, pero esta si, el indicador azul se volvió rojo parpadeante y apareció la siguiente pantalla de SAFE MODE

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/w_400/Safe_Mode_rabsou.png "safe_mode")

Aparentemente debía volver a cargar el archivo del último firmware que tenía (5.16.105) o bajar el último que se había quedado a medías y debería solucionarse, pero no, en ningún caso hay respuesta posible. Tus datos deberían estar a salvo, pero no puedes acceder al sistema de ninguna manera. 

La solución era simple, el sistema de recuperación espera una versión anterior a la que le intentamos cargar, así que le pasé la última antes del cambio de versión, de OS 3 a OS 5, la (2.41.116) que tenía guardada en el ordenador y por fin arrancó el sistema. Es posible que no puedas acceder como admin y tengas que reiniciar el usuario admin a su [password por defecto](https://support-en.wd.com/app/answers/detail/a_id/24022) (sin password) para ello deberás hacer un reset de 4 segundos o si no funciona, el de 40 segundos. La unidad también pasará a modo DHCP (puedes utilizar nmap para localizar la IP asignada o entrar en el mapa de red de tu router). Otras funcionalidades quedaran desactivadas (como el acceso SSH). Este proceso puede tardar entre 5 y 15 minutos aproximadamente, hasta que la luz led de color azul, deje de parpadear.  

Ahora sólo había que importar su archivo de configuración y básicamente tendría todo como antes. Después de esto volví a enviar de forma manual el firmware de OS 5 (5.16.105) y de nuevo su archivo de configuración. Todo a salvo, pero de momento esperaré a un nuevo firmware antes de actualizar por si acaso y eso si, prefiero bajarlo yo antes y cargarlo de forma manual.  

Por cierto, si quieres tener todo a salvo, también deberías tener copia del NAS y no dejarla como única opción. En la versión 5 puedes descargar la aplicación USB Backups y con un disco externo USB 3.+ de 4TB hacer copias sincronizadas.

<a href="https://res.cloudinary.com/dbuv9r3p5/image/upload/USB_Backup_uo1inm.png" target="_blank">
  <img src="https://res.cloudinary.com/dbuv9r3p5/image/upload/w_400/USB_Backup_uo1inm.png">
</a>

Más info sobre: [NAS WD My Cloud](https://support-en.wd.com/app/products/product-detail/p/126#WD_downloads)  
