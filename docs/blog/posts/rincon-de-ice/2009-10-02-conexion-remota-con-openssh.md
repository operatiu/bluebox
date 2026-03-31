---
title: 'Conexión remota con OpenSSH'

date:
  created: 2009-10-02

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - openssh
    - SSH
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/ssh-icon_kfahra.png "ssh")

El otro día hice unas pruebas para crear una conexión remota y segura hacia mi pc.

Para dicho propósito utilicé Openssh bajo Ubuntu 9.04. Ya había utilizado antes el escritorio remoto (usando la aplicación ‘Vinagre’), pero para según que, es preferible un túnel usando la consola y ssh o incluso accediendo en modo gráfico.

No pretendo explicar todo lo que conlleva dicho montaje, para eso hay miles de páginas y tutoriales muy bien detallados, pero si hacer una especie de recetario de las cosas a tener en cuenta y luego cada cual que busque en el lugar adecuado.

<!-- more -->

Supongamos que en el pc de casa tenemos corriendo a Ubuntu o cualquier otra distro GNU/Linux, este pc hará las veces de servidor y cualquier otro, por ejemplo el pc de la oficina será el cliente.

**1.-** Para conectar remotamente con nuestro servidor, necesitaremos tener una IP fija o bien si nuestra IP es dinámica, saber que IP pública le corresponde en cada momento, ya que esta irá variando (según nuestro ISP) o cada vez que reiniciemos el router, y claro no es cuestión de que cada mañana antes de salir accedamos a [http://www.whatismyip.com/](http://www.whatismyip.com/) para obtenerla, por lo tanto lo fácil es registrarnos en alguna página del tipo [http://www.no-ip.com/](http://www.no-ip.com/) o [http://www.dyndns.com/](http://www.dyndns.com/) o cualquier otra que nos proporcione de forma gratuita una url a la que redireccionar nuestra IP dinámica.  
  
**2.-** Necesitamos tener instalado en nuestro servidor (casa), openssh-server ya que el cliente (openssh-client) es el que suele venir instalado.  
  
**3.-** Una vez instalado podemos modificar su configuración /etc/ssh/sshd_config para establecer ciertas políticas de seguridad, como cambiar el puerto por defecto Port 22, no permitir loguearse al root PermitRootLogin no (sobre todo si utilizamos conexiones remotas), permitir la ejecución de aplicaciones gráficas en forma segura X11Forwarding yes, minimizar el número de intentos fallidos de conexión, por ejemplo con 2 o 3 ya está bien, más fallos consecutivos serían muy sospechosos y por tanto cerrariamos la conexión, MaxAuthTries 2 y otra más para evitar intentos masivos de conexión MaxStartUps 3 y para terminar diremos a que usuario permitiremos conectarse AllowUsers usuario_1 usuario_2 usuario_n podemos crear un usuario para esta finalidad y agregarlo al grupo SSH o simplemente utilizar el nuestro y meterlo también en dicho grupo.  
  
Otra opción de seguridad muy importante y que además nos evitaría tener que introducir nuestro usuario y contraseña en cada sesión, sería la utilización de una clave pública y privada, que podríamos generar por ejemplo con: ssh-keygen -t rsa para el par de claves utilizando cifrado rsa o dsa (desde nuestro cliente y que en principio se almacenará en /home/usuario/.ssh/id_rsa para la clave privada y /home/usuario/.ssh/id_dsa.pub para la clave pública) y ahora copiándola en el servidor, ssh-copy-id usuario_remoto@IP_remota ( si habíamos cambiado el puerto de conexión, podemos utilizar -p numero_de_puerto ) la clave pública debería copiarse en .ssh/authorized_keys  
  
Evidentemente hay muchas directivas a utilizar, pero no es el objeto de este recetario, para eso tenemos a **man**.  
  
**4.-** Ya tenemos en nuestro servidor casi todo listo, deberíamos asegurarnos de que nuestro redireccionamiento dinámico funciona correctamente (desde fuera de la red local) y que dentro del router tenemos activado el acceso externo por SSH.  
  
En principio ya debería estar todo listo en nuestro servidor (si hemos cambiado algo últimamente en la configuración del servidor, acordémonos de reiniciarlo con sudo /etc/init.d/ssh restart ) y ahora si desde nuestro cliente linux, con ssh instalado podemos probar desde un terminal:  

```
ssh -p numero_puerto usuario@direccion_servidor  
```

La primera vez nos mostrará una advertencia que deberemos aceptar con "yes" e inmediatamente introducir nuestro password o bien generar un par de claves (pública/privada) y así poder entrar sin problemas a partir de nuestra clave privada, ya que la pública estará almacenada en el servidor.  
  
Para copiar archivos desde el cliente al servidor: 

```
 scp  file.txt  user@dir_remota:/home/user/ 
```
Si queremos acceder a aplicaciones en modo gráfico, lo podemos hacer entrando con la opción -X de la siguiente forma:  

```
ssh -p numero_puerto -X usuario@direccion_servidor  
```
  
Para ejecutar una aplicacion gráfica en local:  **DISPLAY=":0"**  

o podemos exportar el display

```
echo $DISPLAY  
export DISPLAY=dir_cliente:10.0  
```
  
A veces puede suceder que el servidor nos devuelva un error del tipo:

```
_cannot open display: localhost:10.0_   
```
o un valor parecido.   
  
Una de las posibles causas es que el archivo **_.Xauthority_** que se encuentra en la home del usuario con el que nos hemos logueado en el servidor, sea propiedad de root en lugar del usuario. Esto lo podemos solucionar cambiando el propietario con:   

```
sudo chown usuario:usuario /home/usuario/.Xauthority
```

**5.-** Y si queremos entrar directamente en modo gráfico, por ejemplo con el navegador de archivos, podemos utilizar la opción (en Gnome) de Lugares / Conectar con el servidor e introducir los datos correspondientes a Tipo de servicio: (SSH), Servidor: (dirección del servidor), Puerto: (el 22 o el que tengamos), Carpeta: (la carpeta en la que apareceremos, ej. /home/usuario) y Nombre de usuario: (el usuario con el que nos conectaremos).  
  
**6.-** Si queremos conectarnos desde un cliente Windows, podemos utilizar una pequeña pero gran utilidad libre, llamada Putty que podemos descargar desde [http://www.putty.org/](http://www.putty.org/)

**7.-** Para terminar, al llegar a casa (a nuestro servidor), nos puede apetecer saber que intentos de conexión se han producido, para ello basta con teclear last o lastb en un terminal, para que así nos presente el contenido del archivo **/var/log/wtmp o /var/log/btmp** para las conexiones realizadas y fallidas, respectivamente.

  
salu2