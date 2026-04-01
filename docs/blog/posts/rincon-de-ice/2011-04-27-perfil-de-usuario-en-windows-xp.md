---
date:
  created: 2011-04-27

title: 'Perfil de usuario en Windows XP'

author: jmong

layout: post

categories:
    - Antigues
tags:
    - copiarPerfil
    - perfilUsuario
    - WindowsXP
---

Hace poco me sucedió en un equipo con Windows XP, que el usuario principal del sistema (NO el usuario "administrador", aunque si con esos permisos), utilizaba un nombre diferente al que debía tener. Imagínemos por ejemplo una instalación de sistema por otra persona, un equipo nuevo con sistema preinstalado, etc..

El caso es que el usuario de nombre 'CORREO' se debia llamar, por ejemplo, 'NuevoUsuario'. 

Cómo hacemos para cambiar no sólo su nombre, sino su carpeta, programas, permisos, etc.. en Windows XP?

Lo más sencillo es lo siguiente:
<!-- more -->

1.- Entramos en una sesión como usuario administrador.

2.- Creamos un nuevo usuario con los permisos que queremos (en este caso, con permisos de administrador) y de nombre "NuevoUsuario" y entramos en una primera sesión con ese usuario para que se cree el perfil inicial, carpetas, accesos, registro, etc..

<a href="https://res.cloudinary.com/dbuv9r3p5/image/upload/User-1_hii99i.jpg" target="_blank">
  <img src="https://res.cloudinary.com/dbuv9r3p5/image/upload/w_300/User-1_hii99i.jpg" alt="user-1">
</a>

3.- Entramos nuevamente en una sesión como administrador y nos vamos al icono "Mi PC" haciendo clic con el botón derecho. Pinchamos en Propiedades / Opciones avanzadas y vamos al botón Configuración en la ficha central de Perfiles de Usuario.

<a href="https://res.cloudinary.com/dbuv9r3p5/image/upload/User-2_cciocq.jpg" target="_blank">
  <img src="https://res.cloudinary.com/dbuv9r3p5/image/upload/w_300/User-2_cciocq.jpg" alt="user-2">
</a>

4.- En este momento se abre una ventana con los usuarios existentes en el sistema. En nuestro caso, además del "administrador", tenemos al usuario "Correo" y al usuario "NuevoUsuario" que acabamos de crear hace unos minutos. Seleccionamos el usuario del que queremos heredar su perfil o dicho de otra forma, el que "clonaremos". Pinchamos en el botón, "Copiar a" y buscaremos el nombre del usuario al que vamos a transferir ese nuevo perfil. También seleccionaremos en la ficha "Está permitido usar", el botón Cambiar, para indicar ahí también el nombre del usuario "NuevoUsuario" que va a heredar el perfil.

<a href="https://res.cloudinary.com/dbuv9r3p5/image/upload/User-3_xn3g8z.jpg" target="_blank">
  <img src="https://res.cloudinary.com/dbuv9r3p5/image/upload/w_300/User-3_xn3g8z.jpg" alt="user-3">
</a>

5.- Una vez hecho esto (tardará un poco, en función del tamaño del perfil y la velocidad del PC) podremos reiniciar máquina y entrar con el NuevoUsuario viendo como este ya cuenta con el perfil heredado.

6.- A partir de aquí, una vez que comprobemos que todo funciona correctamente y todos los datos están a salvo, podemos acceder de nuevo a la ficha de perfiles de usuario de la segunda imagen, seleccionar el usuario antiguo en nuestro ejemplo de nombre, "Correo" y eliminarlo totalmente.

> **NOTA**: Antes de probar cualquier cosa de este tipo es imprescindible tener todos los datos a salvo, eso que se llama copia de seguridad y que siempre brilla por su ausencia, cuando se necesita. A mi me ha funcionado perfectamente, pero no hay que olvidarse nunca de Mr Murphy.

salu2