---
date:
  created: 2016-11-01

title: 'MAC OS X Sierra &#8211; Permitir aplicacions de terceros'

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - aplicaciones
    - mac
    - permisos
    - theblueblox
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/w_300/allow_apps_moh9ib.png "allowApps")

Apple ha ocultado la posibilidad de instalar y acceder a aplicaciones que no provengan del Apple Store o de desarrolladores identificados, lo que supone un problema con las actualizaciones de LibreOffice o Gimp por poner un ejemplo.

Para solucionar esto y devolver la tercera opción ‘desaparecida’ podemos ejecutar el siguiente comando desde un terminal.  
  
```
sudo spctl –master-disable
```
  
Si queremos dejarla desactivada  
  
```
sudo spctl –master-enable
```