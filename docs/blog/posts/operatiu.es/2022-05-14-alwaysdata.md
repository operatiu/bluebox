---
title: alwaysdata
date: 
    created: 2022-05-14
author: jmong

categories:
    - Informàtica
    - internet
tags:
    - alwaysdata
    - bbdd
    - domain
    - server
    - site
    - thebluebox
---
![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733859076/Alwaysdata.128_aotyla.png "Imatge inicial entrada")

És bastant habitual, que en algun moment necessitem un espai al núvol on poder tenir accés a determinats servidors (de correu, de bases de dades relacionals i noSQL, FTP i SSH, etc). A nivell docent ens pot funcionar molt bé com a eina educativa, ja que ens podem registrar (i cadascú del nostre alumnat) de forma gratuïta amb el pack 100MB.

<!-- more -->

Podem fer servir [alwaysdata](https://www.alwaysdata.com/en/) com a eina que ens proporciona un domini (limitat però gratuït) amb 100MB d’espai on podrem treballar amb:

**WEB**  
Afegir un lloc i instal·lar aplicacions de diferents categories:  
Mailing, marqueting, gestors de continguts, eines de desenvolupament, e-commerce, fòrums, frameworks, gestió de projectes, de documents, etc… i llenguatges com: HTML, Elixir, Go, Java, Javascript, NodeJS, PHP, Perl, Python, Ruby, etc..

**Dominis**

Crea dominis personalitzats.

**Correu electrònic**

Adreces, mailing-lists, historial, Configuració

**Bases de dades**

MySQL, PostgreSQL, MongoDB, CouchDB i RabbitMQ

Per exemple, amb MySQL (i amb la resta d’SGBD) podrem crear i/o pujar les nostres bbdd, fent servir el terminal o phpMyAdmin.   
Crear usuaris, gestionar-los i monitoritzar-los

Per connectar-nos des del client mysql, obrirem un terminal i escriurem la següent comanda.

Suposem que el nostre usuari principal del lloc es diu **operatiu**.

```sql
mysql -u operatiu -p -h mysql-operatiu.alwaysdata.net
```
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 33247265
Server version: 5.5.5-10.4.22-MariaDB MariaDB Server

```sql 
show databases; 

|  Database          |
|--------------------|
| bbdd_backdrop      |
| information_schema |

2 rows in set (0,01 sec)
```

**Accés remot**

:hash:FTP  :hash:SSH  :hash:WebDAV

**Sistemes de gestió avançada**

Estat del servidors, tasques programades, serveis, processos, adreces IP, certificats SSL, recuperació de còpies de seguretat, logs, migracions, etc..

Amb les eines que ens proporcionen, podem instal·lar des d’un gestor de continguts senzill com Backdrop o Yellow, fins a un Joomla o un wordpress, una wiki o un moodle, etc..

!!!note "Haurem de tenir en compte què:"

Només disposem de 100MB amb el registre gratuït, per tant haurem de consultar de quan en quan, quin és l’espai de disc que hem consumit. En el cas de que arribem al nostre límit podrem esborrar allò innecessari que no fem servir i ens malbarata l’espai, la millor manera és mitjançant una connexió SSH via web o des d’un terminal. Si estàs acostumat a treballar amb Linux, no serà cap problema.

Quan t’has registrat has creat un compte amb un nom, aquest compte principal (serà en principi) el que hauràs de fer servir, perquè serà el propietari que tindrà els permisos per crear i eliminar qualsevol cosa al teu espai de disc.

Imaginem que el nostre usuari principal del lloc es diu **operatiu**.

Simplement habilitarem l’accés i li donarem una contrasenya en l’apartat **Accés remot / SSH**: https://admin.alwaysdata.com/ssh/ de forma que des d’un terminal podrem escriure la comanda ssh per accedir remotament.

```console
ssh operatiu@ssh-operatiu.alwaysdata.net
```