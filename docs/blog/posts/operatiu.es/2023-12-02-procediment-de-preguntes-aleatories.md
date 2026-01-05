---
title: 'Procediment de preguntes aleatòries'
date: 
  created: 2023-12-02

author: Joan M.

categories:
    - Educació
    - Informàtica
    - mysql
    - Portada
tags:
    - mysql
    - preguntes
    - procedure
    - questions
    - sql
---
![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1734024158/help-file-icon_h4czow.png "Imatge inicial entrada")

L’altre dia, els meus alumnes havien de fer un examen de consultes SQL en un servidor MySQL, així que vaig pensar que seria interessant, que de les 10 queries que havien de resoldre, què millor que crear un banc de consultes de la base de dades en la que havien de treballar i fer un sorteix aleatori abans de començar la prova. D’aquesta forma, tots treballarien en les seves consultes i no havien de ser exactament les mateixes que les del company. Vaig crear una petita base de dades de dues taules amb l’alumnat i el total de preguntes a utilitzar.

<!-- more -->

```sql
DROP DATABASE IF EXISTS random;
CREATE DATABASE IF NOT EXISTS random;
USE random

CREATE TABLE alumnat (
id CHAR(4) COMMENT 'Quatre darrers dígits del DNI o NIE',
email VARCHAR(40) COMMENT 'correu institut - opcional',
curs tinyint(1) COMMENT 'Curs, per si tenim repetidors',
PRIMARY KEY (id)
)engine=InnoDB;

CREATE TABLE preguntes (
num tinyint auto_increment PRIMARY KEY
)engine=InnoDB;

LOAD DATA LOCAL INFILE '~/sql/m02uf2_alumnat.tsv' INTO TABLE `alumnat` LINES TERMINATED BY '\n' IGNORE 1 LINES;
```

Ara, un cop tenim les dues taules i hem inserit les dades de l’alumnat, només cal introduir la numeració de les preguntes per assegurar-nos que no tindrem preguntes repetides en cada assignació.

El podem fer amb un simple insert.

```sql
INSERT INTO preguntes (num) VALUES (1), (2), (3), (4), (5), (6), (7), (8), (9), (10), (11), (12), (13), (14), (15);
```

O podem fer un altre procediment amb argument d’entrada, per que ho faci ell, segons la quantitat de preguntes.

```sql
DROP procedure if exists queries;
DELIMITER //  
CREATE PROCEDURE queries(IN q INT)   
BEGIN
DECLARE i INT DEFAULT 1;
WHILE i<=q DO
    INSERT INTO `preguntes` (num) values (i);
    SET i = i+1;
END WHILE;
END//
DELIMITER ;  

CALL queries(15); 
```

Ara només queda fer el procediment d’assignació a cada alumne de preguntes aleatòries.

```sql
DROP PROCEDURE IF EXISTS preg;
DELIMITER //
CREATE PROCEDURE preg()
BEGIN
  DECLARE fi TINYINT(1) DEFAULT 0;
  DECLARE cont INT DEFAULT 0;
  DECLARE preguntes VARCHAR(32);
  DECLARE idc CHAR(4);
  DECLARE emailc VARCHAR(40);
  DECLARE cursc INT;
  DECLARE curs_aleat CURSOR FOR SELECT id, email, curs FROM alumnat Order by curs, email;
  DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET fi=1;
  DECLARE CONTINUE HANDLER FOR SQLSTATE '23000' SET fi=1;
  DROP TEMPORARY TABLE IF EXISTS tt_rpreg;
  CREATE TEMPORARY TABLE tt_rpreg (
    id CHAR(4) PRIMARY KEY, email VARCHAR(40), curs INT, preguntes_examen VARCHAR(32)
  );
  OPEN curs_aleat;
  REPEAT
  FETCH curs_aleat INTO idc, emailc, cursc;
    IF (fi = 0) THEN 
     SET preguntes = (SELECT group_concat(num Order By num) as preg FROM 
       (Select num FROM preguntes Order by rand() LIMIT <strong>10</strong>) as aleat);
     INSERT INTO tt_rpreg SELECT idc, emailc, cursc, preguntes;
     SET cont = cont+1;
     END IF;
   UNTIL fi END REPEAT;
   SELECT * FROM tt_rpreg Order by curs, email;
   SELECT cont as Alumnes;
  CLOSE curs_aleat;
END//
DELIMITER ;

CALL preg();
```

Podem veure com el LIMIT té assignat 10 com a valor de quantitat de registres que aniran al GROUP\_CONCAT, si volem un altre valor només l’haurem de canviar. També podriem fer servir un paràmetre d’entrada i assignar-lo dintre d’una sentència preparada (prepared statement), però potser en aquest cas no és necessari complicar-nos més.

**Nota**: La taula temporal només es mantindrà en la sessió activa, però si volem la podem eliminar després de fer-li el select.

Per altra banda, si volem que aquest SELECT ens enviï el resultat a un arxiu, el podem redirigir mitjançant l’activiació d’una pipe fent servir tee i pager abans i després del procediment preg()

```sql
\! clear;
pager cat | tee -a /tmp/preguntes.out
Select '/tmp/preguntes.out' AS "Arxiu resultant es troba al directori";

DROP PROCEDURE IF EXISTS preg;
DELIMITER //
CREATE PROCEDURE preg()
BEGIN

.. / ...

DELIMITER ;

CALL preg();

nopager
notee
```

També podem fer que la sortida del SELECT vagi directament a un arxiu CSV, si no volem fer servir tee i pager

```sql
SELECT * FROM tt_rpreg Order by curs, email INTO OUTFILE '/tmp/sortida.csv';
```

Evidentment existeixen formes més senzilles de fer el que acabem de veure, però aquesta és més divertida i també ens pot servir com activitat de procedures.