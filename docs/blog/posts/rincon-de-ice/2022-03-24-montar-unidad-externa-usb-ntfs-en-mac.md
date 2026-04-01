---
date: 
    created: 2022-03-24

title: 'Muntar unitat externa USB ntfs en MAC'

author: jmong

layout: post

categories:
    - Informàtica
    - mac
tags:
    - catalina
    - diskutil
    - ntfs
    - thebluebox
    - usb
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/USB-HD-Drive-icon_edxxya.png)

Amb la darrera actualització de MacOS Catalina de març de 2022, macOS 10.15.7 (19H1824), existeix un problema de reconeixement de les unitats externes USB amb sistema d’arxius NTFS.

Si utilitzes el programari de Tuxera o Paragon, en intentar muntar la unitat apareix un error del tipus: **Disc quota exceeded**, i si intentes muntar la unitat des de la utilitat de discos del MAC, obtindràs un error de tipus: **Could not mount “NTFS-DISK” (com.apple.DiskManagement.disenter error 49221)**

Una forma temporal de solucionar-ho, fins a una nova actualització per part d’Apple o bé de Tuxera o Paragon.. és muntar la unitat de manera manual.

1.- Obre un terminal (cmd + barra espaiadora) i escriu terminal o busca-ho punxant en la icona de Launchpad en el Dock.

2.- Cerca la teva unitat i particion USB ntfs que vols muntar amb la sentència:

**diskutil list**

Això mostrarà les diferents unitats  

```
/dev/disk0 (internal, physical):  
\#: TYPE NAME SIZE IDENTIFIER  
0: GUID\_partition\_scheme \*121.3 GB disk0  
1: EFI EFI 209.7 MB disk0s1  
2: Apple\_APFS Container disk2 121.1 GB disk0s2

/dev/disk1 (internal, physical):  
\#: TYPE NAME SIZE IDENTIFIER  
0: GUID\_partition\_scheme \*1.0 TB disk1  
1: EFI EFI 209.7 MB disk1s1  
2: Apple\_APFS Container disk2 1000.0 GB disk1s2

/dev/disk2 (synthesized):  
\#: TYPE NAME SIZE IDENTIFIER  
0: APFS Container Scheme – +1.1 TB disk2  
Physical Stores disk0s2, disk1s2  
1: APFS Volume Macintosh HD – Datos 732.2 GB disk2s1  
2: APFS Volume Preboot 24.4 MB disk2s2  
3: APFS Volume Recovery 525.8 MB disk2s3  
4: APFS Volume VM 1.1 MB disk2s4  
5: APFS Volume Macintosh HD 11.3 GB disk2s5

/dev/disk3 (external, physical):  
\#: TYPE NAME SIZE IDENTIFIER  
0: Apple\_partition\_scheme \*1.0 TB disk3  
1: Apple\_partition\_map 32.3 KB disk3s1  
2: Apple\_Driver43 28.7 KB disk3s2  
3: Apple\_Driver43 28.7 KB disk3s3  
4: Apple\_Driver\_ATA 28.7 KB disk3s4  
5: Apple\_Driver\_ATA 28.7 KB disk3s5  
6: Apple\_FWDriver 262.1 KB disk3s6  
7: Apple\_Driver\_IOKit 262.1 KB disk3s7  
8: Apple\_Patches 262.1 KB disk3s8  
9: Apple\_HFS Passport 1.0 TB disk3s10

/dev/disk4 (external, physical):  
\#: TYPE NAME SIZE IDENTIFIER  
0: FDisk\_partition\_scheme \*250.1 GB disk4
```  
**1: Windows\_NTFS SSD 250.1 GB disk4s1**
```
/dev/disk5 (disk image):  
\#: TYPE NAME SIZE IDENTIFIER  
0: GUID\_partition\_scheme +1.9 TB disk5  
1: EFI EFI 209.7 MB disk5s1  
2: Apple\_HFS Copias de Time Machine 1.9 TB disk5s2
```

En aquest cas i en color vermell, he assenyalat la partició de la unitat a muntar.

3.- Crearem un directori o punt de muntatge

```
sudo mkdir /Volumes/SSD
```

En aquest cas utilitzo el mateix nom de l’etiqueta del disc, que és el que utilitza el sistema quan fa el muntatge automàtic en connectar la unitat USB. D’aquesta manera, les màquines virtuals que tinc en el disc i on apunta VirtualBox quan l’utilitzo, les trobarà sense problemes per a poder continuar treballant amb elles.

4.- Muntem la unitat

```
sudo /Library/Filesystems/tuxera\_ntfs.fs/Contents/Resources/mount\_tuxera\_ntfs -o nodev -o noowners -o nosuid /dev/disk4s1 /Volumes/SSD
```

Per a desmuntar la unitat en acabar, pots utilitzar com sempre el botó dret sobre la seva icona i triar expulsar o bé des del terminal, l’ordre:

```
diskutil unmount /dev/disk4s1
```