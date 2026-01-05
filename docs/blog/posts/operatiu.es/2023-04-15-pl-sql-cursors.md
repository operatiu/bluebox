---
title: 'PL/SQL Cursors'
date: 
  created: 2023-04-15

author: Joan M.

categories:
    - Informàtica
    - mysql
    - Portada
tags:
    - cursor
    - mysql
    - pl/sql
---
![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1734028703/cursor-text-icon_wg4smq.png "Imatge inicial entrada")

Quant parlem de PL/SQL ens estem referint a la part del llenguatge SQL, que s’encarrega de extendre la sintaxi bàsica d’SQL amb altres elements propis de llenguatges de programació no funcionals o exclusivament declaratius.

Amb PL/SQL, dispossem d’eines que ens permetran convertir les nostres bases de dades relacionals, d’estàtiques a dinàmiques.

<!-- more -->

Bàsicament disposeem de:

Disparadors (triggers)  
Routines

- Funcions (functions)
- Procediments emmagatzemats (stored procedure)
- Events

Dintre del que serien els nostres procediments emmagatzemats, podem fer servir altres recursos com:

- Sentències preparades (prepared statement)
- Cursors

Parlem d’aquests últims, enfocats en aquest cas en els servidors de MySQL, encara que de forma similar, treballarien en PostgreSQL, Oracle i d’altres.

Què és un cursor?  
En el nostre cas, un cursor no és més que un iterador, que podrà recorrer tots els registres d’una taula o del resultat d’una query o sentència SQL i processar cada fila individualment en el seu codi.

Fent una analogia amb els triggers, podem parar atenció en la següent sentència:

```sql
CREATE TRIGGER nomTrigger [BEFORE/AFTER] [INSERT/UPDATE/DELETE] ON nomTaula  
FOR EACH ROW  
…
```

## Aquesta línia FOR EACH ROW què és el què fa? 

Doncs, s’encarrega de que tot el que hagi de fer el trigguer per a la taula de la que estarà pendent, ho faci en tots els registres de la taula, la qual cosa ens facilita la vida.

Per què voldríem fer servir un cursor en un procediment ?

En el cas dels procediments no tenim aquesta opció i és per tant que si necessitem recórrer tots els  
registres d’una taula o aquells registres d’una taula que compleixen una determinada condició, per a recórrer-los necessitarem d’un cursor.

Posem un exemple pràctic.

Imaginem una base de dades que gestiona els diferents trens de rodalies, juntament amb els seu horaris, línies, zones, serveis, etc..

Suposem que a nivell didàctic i per omplir les dades dels diferents horaris de trens, volem crear un procediment que el generi a partir de l’identificador de l’estació i la línia a la que pertany, indicant també l’hora d’inici i l’hora final. Podríem anar passant aquests quatre arguments i aniríem generant l’horari de trens. Evidentment, un procediment d’aquest tipus potser no és gaire productiu, sobretot si volem generar tots els horaris aleatòriament en base a alguna condició.

Agafem doncs la taula `horaris`, de la nostra base de dades.

```sql
CREATE TABLE horaris (
  id_estacio INT NOT NULL,
  id_linia CHAR(3) NOT NULL,
  hora TIME NOT NULL,
  PRIMARY KEY (id_estacio, id_linia, hora),
  FOREIGN KEY (id_estacio, id_linia) REFERENCES estacions_linies_zona (id_estacio, id_linia)
)engine=innodb;
```

Exemple de procediment manual d’horaris

**PROCEDIMENT** manual per afegir horaris aleatoris entre 10 i 30 min a la taula horaris indicant els arguments d’entrada

```sql
DROP PROCEDURE IF EXISTS gen_rodhorari;
DELIMITER //
CREATE PROCEDURE gen_rodhorari(IN p_id_estacio INT, IN p_id_linia CHAR(3), IN hora_inici TIME, IN hora_final TIME)
BEGIN
  DECLARE ctime TIME;
  DECLARE interval_min INT;
  SET ctime = hora_inici;

  WHILE ctime < hora_final DO
    SET interval_min = ROUND(RAND() * 20 + 10);
    INSERT IGNORE INTO horaris (id_estacio, id_linia, hora) VALUES (p_id_estacio, p_id_linia, ctime);
    SELECT ctime as 'time';
    SET ctime := ctime + INTERVAL interval_min MINUTE;
  END WHILE;
END //
DELIMITER ;
```

Si ens centrem en els horaris i partim de la taula `estacions_linies_zona`, que és on tindrem les diferents estacions amb cada línia i zona a la que pertany, la millor i més ràpida forma de fer-ho serà amb cursors.

```sql
CREATE TABLE estacions_linies_zona (
  id_estacio INT NOT NULL,
  id_linia CHAR(3) NOT NULL,
  id_zona CHAR(2),
  PRIMARY KEY (id_estacio, id_linia),
  FOREIGN KEY (id_estacio) REFERENCES estacions(id_estacio),
  FOREIGN KEY (id_linia) REFERENCES linies(id_linia),
  FOREIGN KEY (id_zona) REFERENCES zones(id_zona)
)engine=innodb;
```

**Procediment amb CURSOR**

```sql
DROP PROCEDURE IF EXISTS gen_rodhorari;
DELIMITER //
CREATE PROCEDURE gen_rodhorari() -- (IN p_id_estacio INT, IN p_id_linia CHAR(3), IN hora_inici TIME, IN hora_final TIME)
BEGIN
  DECLARE ctime TIME;
  DECLARE interval_min INT;
  DECLARE fi INT(1) DEFAULT 0;
  DECLARE p_id_estacio INT;
  DECLARE p_id_linia CHAR(3);
  DECLARE hora_inici TIME DEFAULT '06:00:00';
  DECLARE hora_final TIME DEFAULT '21:00:00';
  DECLARE voltes INT DEFAULT 1;
  DECLARE total INT;
  DECLARE curstaula CURSOR FOR
  Select id_estacio, id_linia FROM estacions_linies_zona;
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET fi=1;
  SET ctime = hora_inici;
  SELECT count(*) FROM estacions_linies_zona INTO total;
  OPEN curstaula;
  WHILE (fi <> 1) DO
    FETCH curstaula INTO p_id_estacio, p_id_linia;
    SELECT voltes, ' de ', total AS CONTADOR;
    IF (fi = 0) THEN
      WHILE (ctime < hora_final) DO
        SET interval_min = FLOOR(RAND() * 21 + 10); -- intervals aleatoris entre 10 i 30 min
        INSERT IGNORE INTO horaris (id_estacio, id_linia, hora) VALUES (p_id_estacio, p_id_linia, ctime);
        -- SELECT ctime as 'time';
        SET ctime := ctime + INTERVAL interval_min MINUTE;
      END WHILE;
    SET ctime = hora_inici;
    SET voltes:= voltes + 1;
    END IF;
  END WHILE;
  CLOSE curstaula;
END //
DELIMITER ;
```

En aquest cas he comentat, els arguments d’entrada i els he declarat com a constants.