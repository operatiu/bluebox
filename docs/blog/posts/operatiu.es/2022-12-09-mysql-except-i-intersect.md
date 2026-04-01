---
date:
  created: 2022-12-09

title: 'MySQL Except i Intersect'

author: jmong

layout: post

categories:
    - Informàtica
    - mysql
    - Portada
tags:
    - except
    - intersect
    - mysql
---

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/w_150/mysql_8.0.31_logo-1_gbjium.png)

**MySQL versió 8.0.31**

S’han incorporat molt canvis en aquesta versió de MySQL, però en aquest cas destacarem les sentències **Except** i **Intersect**

Provem amb uns exemples sobre la BBDD vk22

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/vk22-300x276_nrln9r.png "vk22")

Suposem les següents consultes i el que ens retorna el servidor en cada cas.
<!-- more -->

```
SELECT * From productes where stock > 50 and stock < 100;
```

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/vk1_t7m7hw.png)

SELECT \* From productes where preu > 1000;

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/vk2_xxbtft.png)

#### **INTERSECT**

Permet obtenir els registres **comuns** a dues taules o relacions.

```
SELECT * From productes where preu > 1000 INTERSECT Select * From productes where stock > 50 and stock < 100;
```

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/vk3_o5fl5m.png)

Abans el feiem amb INNER JOIN, JOIN o NATURAL JOIN

```
SELECT * FROM (Select * From productes where preu > 1000) as t1 NATURAL JOIN (SELECT * From productes where stock > 50 and stock < 100) as t2;
```

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/vk4_dcn9ye.png)

o amb JOIN i les dues consultes

```
SELECT * FROM (Select * From productes where preu > 1000) as t1 JOIN (SELECT * From productes where stock > 50 and stock < 100) as t2 USING(id_fab,id_producte);
```

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/vk5_hos1cf.png)

#### **EXCEPT**

Permet obtenir els registres **no comuns** a dues taules o relacions, segons l’ordre de taules indicat.

```
Select * From productes where preu > 1000 EXCEPT Select * From productes where stock > 50 and stock < 100;
```

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/vk6_nnra1r.png)

Abans el feiem amb RIGHT JOIN o LEFT JOIN, segons l’ordre de taules.

```
SELECT * FROM (Select * From productes where preu > 1000) as t1 LEFT JOIN (Select * From productes where stock > 50 and stock < 100) as t2 USING(id_fab,id_producte) WHERE t2.id_fab IS NULL and t2.id_producte IS NULL;
```

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/vk7_wijoko.png)

Si per una altra banda, volem obtenir tots els registres no comuns de les dues taules, aleshores farem servir un FULL OUTER JOIN, que en MySQL es farà amb un \[LEFT OUTER JOIN\] **UNION** \[RIGHT OUTER JOIN\]

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/vk8_zrj41b.png)