---
title: Històries de CAD
date:
  created: 2022-02-03
author: jmong
layout: page
categories:
  - Cad
tags:
  - cad
  - thebluebox
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/LISP-Programming-Language-ebook-by-Su-TP-Rakuten-Kobo_o6nyop.jpg "eBook")

A finals dels ’80, vaig començar una època interessant que va durar una mica més de vint anys.

Aleshores, treballava en un despatx d’arquitectura on la meva tasca era fer una mica de tot. Des de dibuixar els plànols d’arquitectura, que finalment es convertirien en edificis, (en un inici els dibuixàvem a mà), a portar la contabilitat, la gestió informàtica d’aquells primers 286 que van entrar en la nostra quotidianitat i que van deixar arraconats els petits però potents Sinclair Spectrum de tecles de goma.

El temps passava més a poc a poc. A l’estiu feia calor i a l’hivern plovia i feia fred.

De sobte, els anys noranta ens van portar el CAD (**C**omputer **A**ided **D**esign) i allò va obrir la porta d’un nou futur tecnològic, que seria imparable.

Els equips els vam connectar en una xarxa d’estrella amb cablejat coaxial (teníem tres). Encara recordo aquells manuals i revistes del Windows NT, on configurar drivers era una odissea. L’MS-DOS amb el seu menú d’arrencada amb files i buffers, ramdrive i memòria virtual. De fons, la millor música dels ’80 i ’90 ens feia companyia en una antiga ràdio de color negre i dial vermell.

Amb les Olimpíades del ’92, vam descobrir Slackware i Debian, més tard (finals dels noranta) també va absorbir part del meu temps Mandrake i Gentoo, però és una altra història.

Quins temps, però tornem al CAD, aquells nous i rudimentaris sistemes de dibuix que implementaren [AutoLISP](https://www.autodesk.com/blogs/autocad/tag/autolisp-to-automate-your-tasks/) i que van permetre personalitzar amb rutines la nostra feina. En aquells temps vaig crea una de les meves primeres pàgines: [Estación de CAD](http://www.geocities.com/jmong.geo/) que ja fa temps va desaparèixer.

## AutoLISP

AutoCAD va permetre certa programació en el seu producte estrella, basada en un llenguatge de intel·ligència artificial dels anys cinquanta.

Encara recordo com li dèiem de forma afectuosa: Lost In Stupid Parents

Doncs bé, aquí us deixo alguna d’aquelles primeres rutines que més tard van anar evolucionat a coses més grans i gestionades en sistemes de interfície gràfica amb diàlegs controlats amb DCL (Dialog Control Language)

### Macros

```
(defun c:p() (command “\_pline”) )
(defun c:e() (command “\_erase” “\_c”) )
(defun c:v() (command “\_move” “\_c”) )
```

### Rutines

```
(Defun C:sc () 
(setq E (car (entsel "Designa objecte de capa on et vols situar: "))) 
(If E 
  (progn 
    (setq E (entget E )) ;obté les dades definidores de l'entitat
    (setq N (cdr (assoc 8 E ))) 
    (command "_LAYER" "_M" N "" ) 
    )
  ) 
)
```

```
(setvar "cmdecho" 0)
(Defun C:sf ()
(setq E (car (entsel "Apuntar objeto situado en capa a congelar: ")))
(If E (progn
	   (setq E (entget E )) ;obté les dades definidores de l'entitat
	   (setq N (cdr (assoc 8 E )))  
	   (PRINC (STRCAT "\nCongelant capa: < "N" >"))
	   (command "_LAYER" "_F" N "" )
))
(princ)
)
```

Salu2