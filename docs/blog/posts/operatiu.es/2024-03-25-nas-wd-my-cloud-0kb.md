---
title: NAS WD My Cloud - 0KB
date: 
    created: 2024-03-25

author: Joan M.

categories:
    - Informàtica
    - Portada
tags:
    - 0KB
    - e2fsck
    - MyCloud
    - NAS
    - recover
    - WD
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733947125/NAS_dev_dwmae4.png "Imatge inicial entrada")

Després de la darrera actualització de firmware al febrer de 2024, la versió 5.27.161 tot semblava anar bé, però…

De sobte, quan intentava accedir al dispositiu, no tenia accés, era com si es quedés bloquejat o hagués algun problema amb la targeta de xarxa, cosa que realment no passava.

Com aquest dispositiu no disposa d’un botó per apagar o reiniciar, l’única forma era forçar un reinici amb el reset de 4 segons o bé desconnectant del corrent. En qualsevol cas, no és una bona praxis i un problema de disseny del NAS.

<!-- more -->

Així doncs, un dia quan intentava accedir-hi em vaig trobar la següent imatge en la pantalla d’inici del NAS.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948844/20240322_NAS_0KB_cmglcx.png "Imatge 01")

Un cop inesperat i dur, ja que tinc aproximadament la meitat de l’espai ocupat amb arxius i documents importants. Qualsevol que fa servir un NAS i guarda les seves còpies allà, és perquè per a ell són molt valuoses.

El servei tècnic, al que em vaig dirigir obrint una incidència des del mateix NAS em va respondre amb un enllaç al [fòrum i als documents](https://community.wd.com/categories) d’ajut (lamentable que ningú es posses en contacte amb mi o em donés algun tipus d’orientació tècnica).

Vaig exposar el què passava, però no vaig obtenir resposta, només frustració.

Em vaig connectar per ssh

```shell
ssh sshd@192.168.16.254
```

i vaig executar aquelles comandes que podien donar-me informació del disc.

**df -h**

```shell
Filesystem Size Used Available Use% Mounted on
/dev/root 53.4M 9.0M 41.5M 18% /
devtmpfs 248.6M 4.0K 248.6M 0% /dev
mdev 248.6M 4.0K 248.6M 0% /dev
/dev/sda3 976.1M 288.8M 661.2M 30% /boot
/dev/sda7 976.1M 1.5M 948.5M 0% /usr/local/config
/dev/loop0 141.3M 141.3M 0 100% /usr/local/modules
tmpfs 1.0M 104.0K 920.0K 10% /mnt
tmpfs 40.0M 764.0K 39.3M 2% /var/log
tmpfs 100.0M 6.2M 93.8M 6% /tmp
/dev/md0p1 525.3M 20.0K 514.3M 0% /usr/local/upload
/dev/sda4 928.9M 115.0M 797.9M 13% /mnt/HD_a4
```

Però algo faltava

```shell
/dev/sda2             3.6T  1.6T  1.9T  46% /mnt/HD/HD_a2
```

En un principi, vaig pensar que la partició de dades podia estar muntada en /dev/sda1 però en realitat aquí munta una SWAP (el vaig averiguar més tard)

Cal dir que no estan disponibles totes les comandes que tindries en una distribució GNU/Linux, com la que incorpora (en teoria un Debian 11).

A més, els resultats que tornaven les comandes, podrien fer creure que el disc estava treballant amb un RAID cosa que no és cert, ja que només tinc un disc (4 TB)

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948799/mdadm_px3ngu.png "Imatge 02")

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948793/blkid_q5gic5.png "Imatge 03")

I per últim vaig llençar un **smartctl** (un **fdisk -l** no l’executava)

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948804/smartctl_pwhnus.png "Imatge 04")

Al final, la resposta era obvia, si no podia desmuntar la partició i fer una reparació dels sectors danyats directament, havia d’aïllar el disc.

Havia provat ha desmuntar-la amb: **umount -l /dev/sda1** o /dev/sda2 però no em deixava

Havia provat d’executar **mount**, de veure l’arxiu **/etc/fstab** de mirar en **/proc/mounts**

Arribava el moment de desmuntar físicament la unitat de disc.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948709/IMG_8120-1_rvky9l.jpg "Imatge 05")

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948715/IMG_8121_ph5lpj.jpg "Imatge 06")

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948726/IMG_8122_qaw4ya.jpg "Imatge 07")

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948732/IMG_8124_vsbwwc.jpg "Imatge 08")

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948738/IMG_8125_ghi17j.jpg "Imatge 09")

Per extraure el disc només l’has de fer bascular una mica cap a munt, pero abans has de treure o afluixar unes pestanyes metàl·liques a esquerra i dreta dels connectors de xarxa, USB i alimentació, que fan que el disc quedi fixat i no es mogui. Si no ho fas, pateixes el risc de trencar la subjecció i no poder fixar-lo bé quan el tornis a muntar.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948743/IMG_8126_rmouqi.jpg "Imatge 10")

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948748/IMG_8129_om7gvf.jpg "Imatge 11")

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948753/IMG_8130_mvfsxj.jpg "Imatge 12")

Un cop fora, només cal treure els tres cargols i els tubs de recolzament que fan servir.

Hauràs de desenganxar una mena de celo platejat que el fixa al lateral del disc.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948759/IMG_8132_jf19ee.jpg "Imatge 13")

Ja el tenim.

**Segona part**: Recuperació de dades

Per aquesta part, la més important, cal punxar el disc en un equip amb Linux (amb cablejat d’alimentació i dades).

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948784/IMG_8137_znrj04.jpg "Imatge 14")

Un cop connectat, pots encendre l’equip i passar per la BIOS per veure que reconeix el disc.

Ara ja pots arrencar i començar la feina de recuperació.

Primer vaig pasar-li l’eina **gparted** però quan vaig localitzar la partició de les dades (ara era /dev/sdb2) el resultat no va ser satisfactori. Em començava a preocupar..

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948764/IMG_8133_xt8t4l.jpg "Imatge 15")

Vaig obrir una consola i en modo no invassiu va fer un e2fsck

**sudo e2fsck -n /dev/sdb2**

Aleshores vaig veure que sortien errors a recuperar i eren molts, per tant vaig enviar la comanda que em recuperava l’estat del disc.

**sudo e2fsck -fy /dev/sdb2**

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948771/IMG_8135_agubtn.jpg "Imatge 16")

Això ja pintava bé !

Només calia tornar a muntar-lo tot com estava i conectar novament el NAS

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948777/IMG_8136_dlrmc1.jpg "Imatge 17")

Per últim vaig entrar al NAS fent servir el navegador i la meva adreça local, que cal dir que si no funciona internet no hi pots accedir. Quina cagada de WD, que hagi de redireccionar la teva adreça local a un remota en el port 8543, ja que si no, no et deixa accedir localment al teu dispositiu. Em sembla penós a la part que horrorós.

Però, ja tinc les meves dades !!

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948846/20240325_NAS_OK_whvkgc.png "Imatge 18")

En resum, ja porto molts anys amb aquest NAS i he tingut algun que altre ensurt pel tema del firmware. Això no és admissible i si els tècnics i fabricant no es posen les piles, a part de perdre els seus clients, a la llarga seran ells els que perdran, ja que no hi ha res pitjor, que els teus clients perdin la confiança en la marca.

Senyors de WD, milloreu el vostre producte i el vostre servei !!