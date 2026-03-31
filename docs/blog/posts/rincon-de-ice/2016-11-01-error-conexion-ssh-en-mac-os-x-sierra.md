---
title: 'Error conexión SSH en MAC OS X Sierra'

date:
  created: 2016-11-01

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - consola
    - sierra
    - SSH
    - theblueblox
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/Terminal-icon_kqgazm.png "ssh")

Recientemente, tras la actualización de Capitan a Sierra, he podido comprobar que no podia conectar vía ssh mediante un terminal de MAC OS X Sierra con otros equipos, como lo hacía normalmente hasta la actualización.

El error al intentar conectar era el siguiente:
<!-- more -->

```
Unable to negotiate with X.X.X.X port 22: no matching host key type found. Their offer: ssh-dss
```

Este error se produce coincidiendo con una actualización igual o superior a la 7.0 de OpenSSH donde queda desactivado el algoritmo de clave pública ssh-dss (DSA)

Según lo publicado en:  
[https://www.gentoo.org/support/news-items/2015-08-13-openssh-weak-keys.html ](https://www.gentoo.org/support/news-items/2015-08-13-openssh-weak-keys.html)

“*A partir de la versión 7.0 de OpenSSH, la compatibilidad con las claves ssh-dss se ha desactivado de forma predeterminada en tiempo de ejecución debido a su debilidad inherente. Si confía en estos tipos de claves, tendrá que tomar medidas correctivas o arriesgarse a ser bloqueado.*

*Su mejor opción es generar nuevas claves usando algoritmos fuertes como rsa o ecdsa o ed25519. Las claves RSA le darán la mayor portabilidad con otros clientes / servidores, mientras que ed25519 le brindará la mejor seguridad con OpenSSH.*  
*…*“

Por lo que una forma de conectar es forzando el uso del algoritmo ssh-dss

```
ssh -o HostKeyAlgorithms=+ssh-dss usuario@host
```

Si quieres habilitarlo de forma permanente puedes hacerlo mediante un archivo **~/.ssh/config** con el siguiente contenido:

```
Host *<br></br>HostkeyAlgorithms +ssh-dss<br></br>PubkeyAcceptedKeyTypes +ssh-dss
```

Para más información consulta:  
<https://www.openssh.com/legacy.html>