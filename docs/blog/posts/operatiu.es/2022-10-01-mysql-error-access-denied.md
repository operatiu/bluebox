---
date:
  created: 2022-10-01
title: "MySQL error: Access denied"
author: jmong
layout: post
categories:
  - Informàtica
  - mysql
  - Portada
tags:
  - error
  - mysql
  - root
  - sql
  - user
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/error-Database-icon_hfjezf.png "mysql-error")

Aquest error a vegades és més comú del que seria desitjable. El problema és que a vegades afecta a **root** i altres a qualsevol usuari.

Quan cerques informació per tal de resoldre’l, en la majoria de llocs, la resposta és:

O bé que li donis a l’usuari tots els permisos i privilegis.

**Exemple**

```
CREATE USER 'usuari'@'%' IDENTIFIED BY 'Contras3ny@';
GRANT ALL PRIVILEGES ON *.* TO 'usuari'@'%' WITH GRANT OPTION;
```

Aquesta pot **no** ser una bona solució, sobretot si aquest usuari no és admin o root.

O bé que el canvis el plugin per defecte a **mysql_native_password**.

**Exemple**

```
ALTER USER 'usuari'@'%' IDENTIFIED WITH mysql_native_password BY 'Contras3ny@';
FLUSH PRIVILEGES;

```

Tampoc és necessàriament la solució, si la teva política, és fer servir el plugin per defecte.

<code class="language-sql">
SELECT user, host, plugin FROM mysql.user WHERE user IN('dummy', 'root');
</code>

| user | host | plugin | 
|--|--|--|
| dummy | % | caching_sha2_password |
| root | localhost | caching_sha2_password |
2 rows in set (0,00 sec)


O bé, en altres casos, he trobat que recomanen que afegeixes l’entrada: **skip-grant-tables** a l’arxiu de configuració /etc/mysql/my.cnf o /etc/mysql/mysql.cnf o **/etc/mysql/mysql.conf.d/mysqld.cnf** (segons versió de MySQL i distribució Linux que facis servir) dintre de la clase **\[mysqld\]** de configuració del servidor.

Això segurament et funcionarà però a la llarga o a la curta, veuràs que et donarà molts altres problemes, limitant moltes accions.

Bé, ara arribem on jo volia arribar, ja que en el meu cas m’interessava crear un usuari regular amb permisos limitats i fins i tot amb una contrasenya senzilla, sense variar res del servidor de forma permanent.

El problema era què, per mandra i per entrar de forma ràpida en local amb l’usuari **root**, havia creat l’arxiu **.my.cnf** en la meva home d’usuari.

\[mysql\]  
user = ‘root’  
password = ‘elMeuP@ssw0rd’

Això provocava que en MySQL 8.x encara que entrés amb: **mysql -u root -p** no em demanava password, però a més restringia l’entrada a qualsevol altre usuari que no fos root.

```
mysql -u dummy -p
ERROR 1045 (28000): Access denied for user 'dummy'@'localhost' (using password: YES)
```

**La solució és senzilla**, només cal eliminar o renombrar l’arxiu .my.cnf o simplement comentar ( # ) #user i #password dins l’arxiu, per a que no el tingui en compte.

Ara ja podem crear qualsevol tipus d’usuari: per connectar-se des de qualsevol IP ‘%’, amb ip fixa indicant la seva IP, o bé usuari local amb ‘localhost’ i accedir al servidor, per exemple des del client mysql.

Primer, entrem com a root i baixem la política de complexitat de contrasenya i la longitud mínima (si t’interessa)

```
SET GLOBAL VALIDATE_PASSWORD.POLICY=LOW;
SET GLOBAL VALIDATE_PASSWORD.LENGTH=4;
```

Ara crearem l’usuari ‘dummy’

```
CREATE USER 'dummy'@'%' IDENTIFIED BY 'dummy';
GRANT SELECT, INSERT, UPDATE, DELETE ON geografia.distancia TO 'dummy'@'%';
```

Cal dir, que aquesta bbdd conté altres taules, però només vull que l’usuari ‘dummy’ tingui els permisos anteriors sobre la taula `distancia`.

Ja està, puc entrar en local o en remot amb ‘dummy’ i quan reinici el servidor (o fent el canvi de variables manualment), aquestes tornaran al seu estat anterior sobre la creació de contrasenyes d’usuari.

<code class="language-sql">
mysql -u dummy -p
Enter password:
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 18
Server version: 8.0.30 MySQL Community Server - GPL
</code>

En canvi hauré d’entrar amb **root** introduint la seva contrasenya, cosa que està bé 🙂