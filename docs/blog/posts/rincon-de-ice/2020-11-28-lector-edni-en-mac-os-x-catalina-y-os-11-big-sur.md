---
date: 
    created: 2020-11-28

title: 'Lector eDNI en MAC OS X (Catalina) y OS 11 (Big Sur)'

author: jmong

layout: post

categories:
    - Antigues
tags:
    - e-dni
    - edni
    - firmware
    - lector
    - pdf
    - thebluebox
    - usb
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/dni-icon_bbicr8.png "dni")

## Instalación y configuración del lector USB de e-DNI en MAC

 Con un DNI-e hasta versión 2.0 (Chip en la parte anterior o anverso del DNI) o DNI-e **3.0** (Chip en el reverso del DNI y fotografía en el anverso), [aquí puedes ver](https://www.dnielectronico.es/PortalDNIe/PRF1_Cons02.action?pag=REF_038&id_menu=1) las diferencias, puedes utilizarlo para validar tu identidad en páginas web oficiales o para firmar archivos PDF, entre otras cosas.

 Necesitarás un lector, de los muchos que existen en el mercado, yo he hecho la prueba con un **SVEON SCT011M** del que no me supieron solucionar, el porque no lo detectaba en MAC OS

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-01_i4nnii.png "lector eDNI")

Pues bien, supongo que esto será extensible a cualquier lector de DNIe así que lo primero que tienes que hacer es la instalación de los certificados y librerías, que podrás descargar desde el [siguiente enlace](https://www.dnielectronico.es/PortalDNIe/PRF1_Cons02.action?pag=REF_1113). 

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-02_uafvye.png "software")

El software anterior funciona en Catalina y Big Sur. Da igual que descargues el archivo .dmg o el .pkg (el .pkg está encapsulado en el .dmg), eso si, tienes que dar permisos de ejecución, así que al intentar ejecutar la descarga te aparecerá un aviso bloqueando la instalación. Accede a **PREFERENCIAS / SEGURIDAD Y PRIVACIDAD** y en **Permitir apps descargadas de:** *“App Store y desarrolladores identificados*“. 

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-03_lsktvh.png "unlock")

Desbloquea el candado y permite que prosiga la instalación.  
Cuando finalice aparecerá una página que te indica como continuar con la modificación de las preferencias de privacidad y seguridad del navegador Firefox (yo he hecho las pruebas con la versión 83.0 de 64 bits, obviamente).

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-04_ptcg4v.png)

Ahora tienes que seguir los pasos de la imagen siguiente para instalar el módulo de cifrado PKCS#11 desde el botón **Dispositivos de seguridad..** (imagen superior de Firefox) y a continuación el Certificado Raíz de la pestaña Autoridades, a la que accederás después de pulsar en **Ver certificados..** Lo único que hay que hacer en cada caso es indicar la ruta donde encontrarlos y que está en: **/Library/Libpkcs11-dnie/lib/libpkcs11-dnie.so** para el primer caso y la ruta **/Library/Libpkcs11-dnie/ac\_raiz\_dnie.crt** en el segundo caso.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-05_ensbmi.png "cert")
Certificado raíz

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-06_pe4mbg.png "inst")

Una vez hecho esto y cada vez que quieras utilizar el lector de DNIe ***lo que tienes que hacer (en la mayoría de los casos) es arrancar con el lector conectado al una ranura USB para que lo detecte al inicio***, de esa forma si lo quieres utilizar lo siguiente será introducir el DNIe con el chip hacia abajo (en este lector en particular, en otros podría ser justo al revés) y lo siguiente, iniciar el navegador Firefox.

Para comprobar si la detección es correcta, sólo tienes que abrir las preferencias de Firefox y en privacidad y seguridad, pulsar en Ver Certificados y debería aparece tu Certificado del DNIe

## Firmar documentos PDF en Adobe Reader 

1.- Pinchar en el menú superior **Acrobat Reader / Preferencias / Firmas** y a continuación en las opciones que aparecen a la derecha, haz clic en el botón **Más..** que aparece en **Identidades y certificados de confianza**.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-07_njiczl.png)

Dentro de ID digitales, sitúate en **Módulos y distintivos PKCS#11** y pincha en **Adjuntar módulo**.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-08_q7ufrv.png "pkcs")

La ruta del módulo és la que utilizamos anteriormente y se encuentra en: **/Library/Libpkcs11-dnie/lib/libpkcs11-dnie.so**

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-09_dmmrmv.png "update")

Una vez cargada, si le das al botón **Actualizar** debería pedirte la clave o PIN de seguridad de tu DNIe (10 dígitos alfanuméricos) que en su día debiste configurar en la máquina correspondiente de la oficina de expedición del DNIe

Una vez aparezca el DNIe como **Conectado**, ya puedes utilizarlo para firmar en Adobe Reader.

Para firmar un PDF, ábrelo y haz clic en Herramientas / Certificados.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-10_uuzqtf.png "certificates")

Ahora podremos pulsar en **Firmar digitalmente** y marchar un rectángulo dónde se incluirá nuestra firma digital.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-11_kptdec.png "sign")

Nos aparecerá la selección del Certificado a utilizar y el nombre y ubicación donde guardaremos el nuevo archivo PDF firmado, que podemos dejar bloqueado para que no se pueda alterar o volver a firmar o bien dejarlo sin bloquear en el caso de que más personas tengan que firmar digitalmente en el.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/sveon-12_ensdje.png "signed")

La firma digital no es una firma manuscrita, si no que nos identifica de forma irrevocable mediante un certificado digital válido, expedido por una entidad certificadora reconocida. Su apariencia puede incluir nuestro nombre y apellidos con la fecha y hora del momento de la firma o bien cualquier otro elemento, como la emulación de nuestra firma manuscrita. El original siembre es el documento firmado digitalmente y cualquier impresión de el, se considera una copia y no el documento original en si, que hay que guardarlo de forma digital.