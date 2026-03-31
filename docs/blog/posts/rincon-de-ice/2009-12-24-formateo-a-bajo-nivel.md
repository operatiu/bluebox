---
date:
  created: 2009-12-24

title: 'Formateo a bajo nivel'

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - dd
    - ext3
    - fdisk
    - gparted
    - mount
    - particiones
    - reiserfs
    - testdisk
    - umount
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/Hard-Drive_fk48ml.png "fdisk")

De repente, un día empezamos a notar que un disco duro empieza a fallar. El acceso a los datos es errático, a veces no se monta bien o simplemente parece que perdemos los datos al intentar acceder. Evidentemente, esto no es lo habitual, pero a quien no le ha ocurrido alguna vez?  
  
Lo primero sería utilizar algunas herramientas básicas de comprobación:  
  
Para empezar lo mejor sería tener la unidad desmontada, cosa que podremos hacer con umount siendo super usuario o root.  
  
Para saber que partición o particiones desmontar, tenemos varias opciones:  
  
desde consola:  
```
sudo fdisk -l  
```
Eso nos devolverá un listado de las particiones y el sistema de archivos utilizados, entre otras cosas:  
 <!-- more -->
  
> Disco /dev/sda: 21.5 GB, 21474836480 bytes  
> 255 cabezas, 63 sectores/pista, 2610 cilindros  
> Unidades = cilindros de 16065 * 512 = 8225280 bytes  
> Identificador de disco: 0x0004ad3e  
>   
> Disposit. Inicio Comienzo Fin Bloques Id Sistema  
> /dev/sda1 * 1 2496 20049088+ 83 Linux  
> /dev/sda2 2497 2610 915705 5 Extendida  
> /dev/sda5 2497 2610 915673+ 82 Linux swap / Solaris

  
También podríamos acceder a los puntos de montaje utilizados y guardados en **fstab**:  
```
cat /etc/fstab  
```
  
o simplemente utilizando  
**mount**  

> /dev/sda1 on / type ext3 (rw,relatime,errors=remount-ro)  
> proc on /proc type proc (rw)  
> none on /sys type sysfs (rw,noexec,nosuid,nodev)  
> none on /sys/fs/fuse/connections type fusectl (rw)  
> none on /sys/kernel/debug type debugfs (rw)  
> none on /sys/kernel/security type securityfs (rw)  
> udev on /dev type tmpfs (rw,mode=0755)  
> none on /dev/pts type devpts (rw,noexec,nosuid,gid=5,mode=0620)  
> none on /dev/shm type tmpfs (rw,nosuid,nodev)  
> none on /var/run type tmpfs (rw,nosuid,mode=0755)  
> none on /var/lock type tmpfs (rw,noexec,nosuid,nodev)  
> none on /lib/init/rw type tmpfs (rw,nosuid,mode=0755)  
> none on /var/lib/ureadahead/debugfs type debugfs (rw)  
> compartido on /media/comparte type vboxsf (rw)  
> binfmt_misc on /proc/sys/fs/binfmt_misc type binfmt_misc (rw,noexec,nosuid,nodev)  
> gvfs-fuse-daemon on /home/ice/.gvfs type fuse.gvfs-fuse-daemon (rw,nosuid,nodev,user=ice)  
> /dev/sr0 on /media/cdrom0 type iso9660 (ro,nosuid,nodev,utf8,user=ice)

También podemos acceder a la tabla de montaje del sistema de archivos.  
```
cat /etc/mtab  
```

O bien utilizar de forma gráfica, **gparted**.  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjzGLvGsjZypSvNSscgn_stS_mbxog468FnNQelNq5CRqxWA5rhqEvUiFwhhZ5D-fiyYhUlIgiFiuUiuso9b46G7DoRlu9iv4sLG_uk1VzvcRKsrdRScx3_nT7lEBxup4UGikUV_3kZFdM/s200/gparted_1_small.jpg)](http://gparted.sourceforge.net/screenshots.php)

Si nuestro disco es un IDE, tendremos unidades de tipo: HDA, HDB, etc.. con particiones hda1, hda2,..., hdb1, hdbn... y si es un disco SATA (Serial ATA), SCSI o unidades USB o lectores de tarjetas, serán del tipo SDA, SDB, etc.. con particiones del tipo sda1, sda2,..., sdb1, sdbn...

Una vez reconocida nuestra unidad y/o particiones, podemos desmontarlas de la siguiente forma: Supongamos que queremos desmontar una partición sdb1, recordemos que necesitamos ser root.  
```
sudo umount /dev/sdb1  
```

Ahora podemos pasar la utilidad testdisk y comprobar la estructura del disco. Esto lo podemos hacer desde dentro del sistema, siempre y cuando la partición no se corresponda con la partición propiamente del sistema. En cualquier caso lo mejor para realizar este tipo de operaciones sería arrancar con un live-CD, por ejemplo con [gparted live-CD](http://gparted.sourceforge.net/download.php) que además de gparted, tiene cargadas diferentes utilidades de disco que podemos utilizar desde una consola, como por ejemplo testdisk.  
  
Si después de todo (mejor si podemos recuperar datos o tenemos ya nuestra copia de seguridad a buen recaudo), no conseguimos mantenerlo estable y sin errores, una opción sería devolverlo a su estado inicial, osea escribir ceros en todos y cada uno de sus sectores. Esto implica la pérdida de todos los datos de todas las particiones del disco, osea que el formateo a bajo nivel actuará a nivel de disco y no de partición !  
  
Si la unidad afectada es por ejemplo SDB, deberemos hacer lo siguiente, siempre como root o super usuario.  
  
```
dd if=/dev/zero of=/dev/sdb  
```

Si todo va bien, después de un buen rato (dependiente de la capacidad del disco), deberíamos tener un disco totalmente limpio de datos, sectores defectuosos, etc.. y listo para inicializar de nuevo, creando y formateando nuevas particiones y sistema de archivos.  
  
En GNU/linux los sistemas de archivos más habituales son: **EXT2, EXT3, EXT4, XFS, JFS, Reiserfs**, etc..  
  
En mi caso, para particiones que gestionan multitud de archivos relativamente pequeños utilizo reiserfs. Para el resto EXT3 y EXT4. Si quiero compartir unidades entre diferentes sistemas operativos, por ejemplo con algún Windows, podemos utilizar VFAT (Fat32) y NTFS, con los que no deberíamos tener problema alguno de lectura/escritura.  

En el caso de que queramos analizar una partición ext2 o ext3 para solucionar algún problema con sectores erróneos, por ejemplo la partición de sistema "/" o una home montada "/home", necesitaremos arrancar con un Live-CD para acceder a dicha partición o disco sin que esté montado. Con un **# fdisk -l** veremos dónde  se encuentra, por ejemplo **/dev/sda4**. Con estos datos y sabiendo que es de tipo _ext2 o ext3_, podemos lanzar el comando **e2fsck -pv /dev/sda4** para que con la opción **_-p_** repare automáticamente sin preguntar y la **_-v_** para utilizar el modo verbose.  
  
Y eso es todo... de momento ;)  
  
Felices fiestas y que tengan un buen año 2010