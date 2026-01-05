---
title: MySQL - Scripts d'entrada i sortida

date: 
    created: 2024-05-18

author: Joan M.

categories:
    - mysql
    - Portada
tags:
    - mysql
    - pager
    - script
    - tee
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948003/script.64_txf7zr.png "Imatge inicial entrada")

En alguna ocasió ens pot resultar útil, que totes aquelles comandes que enviem al servidor MySQL, no només retornin la sortida per pantalla, sinó que s’escrigui en un arxiu. És tan fàcil com redireccionar la sortida en el mateix arxiu de guió. Un cop provem les comandes de forma manual, les escriurem i executarem des de dins o bé des de fora del servidor.
<!-- more -->

**Exemple d’script**

```sql
! clear;
pager cat | tee -a /tmp/sortida.txt
USE prova

SELECT 'SELECT id,data,valor,substr(objecte, 1, 16) AS objecte FROM auto;' AS 'QUERY 1'\G

SELECT id,data,valor,substr(objecte, 1, 16) AS objecte FROM auto;

SELECT "SELECT DATE(logged), prio, substr(data, 1, 78) as Data_Resum FROM performance_schema.error_log WHERE logged \
   BETWEEN 20240501 AND 20240510 AND prio NOT LIKE 'Warning';" AS 'QUERY 2'\G

SELECT DATE(logged), prio, substr(data, 1, 78) as Data_Resum FROM performance_schema.error_log WHERE logged \
   BETWEEN 20240501 AND 20240510 AND prio NOT LIKE 'Warning';

 /*
 --
 --
 */
nopager
notee
```

- Des de dins del client MySQL el carregarem amb SOURCE

```shell
SOURCE ~/script_out.sql
```

**Per veure el resultat obtingut**:

```shell
sudo cat /tmp/sortida.txt
```
![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948044/ses-01_otzobk.png "Imatge 01")

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733948050/ses-02_qdqcdr.png "Imatge 02")

Des de fora de MySQL, executarem el client amb l’entrada de l’script i la sortida que volem obtenir.

```shell
mysql -u root -p < script_out.sql > nou.txt
```

En aquest cas no necessitem activar **pager** i tampoc **tee**, el redireccionarem directament, encara que no amb format taula.

Si volem accedir a mysql i que les sortides no tinguin capçaleres ni línies de taula podem fer servir els paràmetres -sN

```shell
mysql -u root -p -sN
```

> **-s** (sense línes de taula)
> 
> **-N** (sense capçaleres de taula)
