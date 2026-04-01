---
date: 
    created: 2018-08-03

title: 'Clonar disco de sistema Windows'

author: jmong

layout: post

categories:
    - Antigues
    - Informàtica
tags:
    - acronis
    - crucial
    - easeus
    - gpt
    - theblueblox
---
![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733945748/SSD-Drive-icon_h81ydx.png "Imatge inicial entrada")

**Clonación y migración de sistema de HDD a SSD de menor tamaño**

Es posible que te encuentres con la necesidad de clonar tu disco HDD a uno [SDD](https://es.wikipedia.org/wiki/Unidad_de_disco_duro#Comparativa_de_SSD_y_HDD) que acabas de comprar para mejorar el rendimiento de tu PC de sobremesa o portátil.

Las primeras dudas que te deberían surgir son:
<!-- more -->

1. Se transferirá todo incluida la licencia de Windows ?
2. Qué pasa si mi disco SSD es menor que el HDD pero con suficiente espacio para contener lo que realmente está ocupado?

En el primer caso y siendo el mismo equipo la respuesta es SI, no debería haber ningún problema en el 99% de los casos.

En la segunda pregunta, pongamos un ejemplo para entenderla mejor:

a) Disco origen o actual con Windows 7 o 10 en un HDD ([MBR](https://es.wikipedia.org/wiki/Registro_de_arranque_principal) o [GPT](https://es.wikipedia.org/wiki/Tabla_de_particiones_GUID)) de 1TB pero que tiene ocupados sólo 200GB.

b) Disco destino o SSD nuevo con capacidad de 512GB

No hay problema porque se reajustaran las particiones destino al espacio ocupado en origen.  
Para realizar este proceso disponemos de diferentes opciones, en el caso de Windows existe una aplicación gratuita en su versión home que cuenta con todo lo necesario para lo que queremos hacer. Dicha aplicación es: [**easeUS Todo Backup**](http://down.easeus.com/product/tb_free)

Para hacer las pruebas antes de realizar un desembolso económico en la compra del [disco SSD](https://drive.google.com/open?id=1sE-ydqZFwYad7Bcd52eMjXe3dIbAWiF5) i un [adaptador tipo USB 3](https://drive.google.com/file/d/126lc6lKTqRB_Sg64C74o-x3kUwSlqBE-/view?usp=sharing) que nos permita pinchar el disco SSD al Portátil como destino de la clonación y posteriormente pinchar el HDD para reutilizarlo como disco externo de 2,5” para datos o copias de seguridad, etc.. vamos a virtualizarlo todo. Hemos utilizado una máquina virtual con Windows 7 con un disco VDI de 25GB que contiene dos particiones.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733946026/ease02_win-org-gparted-300x187_ebsow1.png "Imatge 1")

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733946117/ease03_win-org-300x183_wx5yto.png "Imatge 2")

En las imágenes anteriores puede verse el disco origen de Windows después de instalar y clonar el sistema con EaseUS Todo Backup, por lo que el disco contiene unos 9GB contando con la instalación del programa y los archivos temporales.

Una vez descargado y instalado EaseUS Todo Backup, es tan sencillo de utilizar como elegir el disco origen y destino para clonar el sistema.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733946186/ease04_clonar-sistema-300x222_e8be77.png "Imatge 3")

En opciones avanzadas marcaremos la Optimización para SSD

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733946292/ease05_Captura-de-pantalla-2018-08-03-a-las-13.18.36-300x260_kgvq8w.png "Imatge 4")

Ahora sólo cabe esperar. Al disponer de la versión Free (tardará un poco más que si dispusiésemos de una licencia Home con soporte y otras mejoras) pero es de agradecer que se pueda utilizar toda la funcionalidad de una aplicación de forma gratuita y resolver un problema, como es la migración de un sistema de forma sencilla. Una vez acabado (dependiendo del tamaño de disco, podemos marcar la opción de apagar el sistema al terminar) el proceso, podemos sustituir el disco origen por el SSD y arrancar de este. Una vez comprobado que todo es correcto podemos proceder a reutilizar el disco origen con la carcasa que hemos comprado para disco de datos o bien mantenerlo un tiempo (opción recomendada) como disco de seguridad del sistema.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733946367/ease06_Captura-de-pantalla-2018-08-03-a-las-12.35.38-300x149_fyylb6.png "Imatge 5")

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733946443/ease07-300x182_s8lxlu.png "Imatge 6")

Otra recomendación, si tenemos espacio suficiente en el disco SSD podemos utilizar [GPARTED](https://gparted.org/download.php) o cualquier otra utilidad (el mismo EaseUS dispone de [Partition Master](https://www.easeus.com/partition-manager/)) para redimensionar nuestra partición de Windows y dejar espacio suficiente para instalar un sistema GNU/Linux y disponer así de un arranque dual con dos sistemas operativos, que siempre viene bien, sobre todo si uno de ellos es **Linux** 😉 

> [!NOTE]
Si el disco SSD que has comprado es CRUCIAL, puedes descargar la utilidad [Acronis True Image for Crucial](https://www.acronis.com/en-us/promotion/CrucialHD-download/) que te permite hacer la **clonación** al vuelo del disco origen y redimensionar al mismo tiempo para el disco destino (por ejemplo un disco de 1TB con 100GB usados a uno de 256GB). Antes de empezar asegúrate de inicializar el disco SSD.

Recuerda que puede que necesites [optimizar tu SSD ](https://protegermipc.net/2017/10/04/optimizar-disco-ssd/)