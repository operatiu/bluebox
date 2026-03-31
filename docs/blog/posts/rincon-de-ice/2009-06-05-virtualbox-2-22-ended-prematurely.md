---
date:
  created: 2009-06-05

title: 'VirtualBox 2.22 ended prematurely'

author: jmong

layout: blog

categories:
    - Antigues
tags:
    - VBox
    - virtualmachine
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/VirtualBox-icon_ant9gc.png "VirtualBox")

En uno de mis PC's tengo un Windows XP donde está instalado Virtual Box 2.22 con varias máquinas virtuales, entre ellas Ubuntu 9.04 que funciona a la perfección. 

Cual fue mi sorpresa, cuando al intentar actualizar a la 2.24, justo cuando está a punto de acabar la instalación, se produce un error que hace imposible la instalación diciendo que esta ha finalizado prematuramente. Intenté realizar una desinstalación de la versión 2.22 y resulta que se producía el mismo error, así que estaba claro dónde estaba el problema. 
<!-- more -->

Pues bien, la solución es sencilla, se trata básicamente de sustituir las claves del registro que dicen **HKEY_CURRENT_USER por HKEY_LOCAL_MACHINE** de la rama correspondiente, que se detalla a continuación en pocos pasos.  

1.- Accedemos al editor de registro de Windows, por ejemplo mediante: Inicio / ejecutar / regedit.  

2.- En el panel izquierdo buscaremos de forma descendente, hasta llegar a la rama: **HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion?\DIFxApp\Components**  

3.- Una vez aquí pincharemos con el botón derecho del mouse sobre Components y seleccionaremos Exportar esa sub-rama a un archivo de registro que llamaremos y situaremos donde queramos, por ejemplo en **Mis documentos/errorVBox.reg**  

4.- Abrimos con un editor de texto plano el archivo que acabamos de exportar, por ejemplo pinchando sobre el con el botón derecho y seleccionando abrir con WordPad.  

5.- Substituimos donde diga, **HKEY_CURRENT_USER** por **HKEY_LOCAL_MACHINE** y lo guardamos.  

6.- Ahora hemos de importar el archivo corregido y guardado al registro principal, osea que o abrimos **Regedit** y nos vamos a Archivo / importar y lo seleccionamos o simplemente hacemos doble-click sobre el archivo para que se realice automáticamente el mismo proceso de importación.  

Una vez hecho esto ya podemos desinstalar VirtualBox 2.22 y seguidamente instalar la siguiente versión, por ejemplo la 2.24. También podemos intentar instalar directamente la versión 2.24 (como si nunca hubiese pasado nada), pero en este caso al finalizar lo que hará será realizar la desinstalación de la 2.22) no pasa nada, ahora lo volvemos a intentar y veremos que la instalación, finalmente se realiza con éxito y nuestras máquinas virtuales continúan intactas, o al menos a mi me sucedió así. 

[Fuente origen:](http://www.virtualbox.org/ticket/3701#comment:6)