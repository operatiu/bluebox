---
title: Google sheets page
date:
  created: 2022-04-23
author: jmong
categories:
  - Informàtica
tags:
  - google
  - googlesheets
  - sheets
  - sql
  - thebluebox
---
![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1733859356/google-sheets.128_yb7dpr.png "Imatge inicial entrada")

Els fulls de càlcul de Google, ens ofereixen un bon potencial a l’hora de treballar amb elles, no només en quant al full de càlcul en si, si no també per què és el centre de control dels scripts i els plugins que podem fer servir.  
Una de les eines que més m’agrada fer servir, en aquests fulls de càlcul són les queries o consultes SQL. No son un SQL estàndard, però ens permet fer consultes interessants.

Veiem un exemple de query

```
=QUERY(nomFull!$B$2:$E$11;"Select B,C,D where B LIKE '%CFGS%' AND C > date '2020-08-01'"; -1)
```

|AutoCodi|Concepte|Data|Dades|Import|Acumulat|0|
|---|---|---|---|---|---|---|

|AutoCodi|Concepte|Data|Dades|Import|Acumulat|0|
|---|---|---|---|---|---|---|
|1|TAXES CFGS|14/08/2020|1|360,00|360,00||
|2|TAXES CFGS|05/09/2020|6|180,00|540,00||
|3|Sortides|06/09/2020|7|80,00|620,00||
|4|Sistemes digitals|07/09/2020||-500,00|120,00||
|5|TAXES CFGS|08/09/2020|9|360,00|480,00||
|6|TAXES CFGS|09/09/2020|10|360,00|840,00||
|7|Quota CCFF|07/09/2020|11|75,00|915,00||
|8|Impressions|08/09/2020||-250,00|665,00||
|9|Llibres|09/09/2020||-180,00|485,00||
|10|Mat. inventariable|10/09/2020||-426,00|59,00||
||||||||
|Query|TAXES CFGS|14/08/2020|1||||
||TAXES CFGS|05/09/2020|6||||
||TAXES CFGS|08/09/2020|9||||
||TAXES CFGS|09/09/2020|10|||| 
  
### CONTAINS

També podem fer servir cerques, basades en els valors d’alguna altra cel·la, aquesta pot contenir una llista dels possibles valors o la podem escriure directament. Els resultats s’escriuran en el nostre full i per tant no hauran de sobreescriure altres valors.  
Es recomanable doncs, que els valors que busquem estiguin en un altre full.

```
=QUERY(nomFull!A1:F11;"Select A,B,C,D,E,F WHERE (B CONTAINS '"& $B$14 &"' AND E CONTAINS '"&$C$14&"') Order by C"; -1)
```

|AutoCodi|Concepte|Data|Dades|Import|Acumulat|
|---|---|---|---|---|---|

|AutoCodi|Concepte|Data|Dades|Import|Acumulat|
|---|---|---|---|---|---|
|2|TAXES CFGS|05/09/2020|6|180,00 €|540,00 €|
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|Query|Concepte|Import||||
||TAXES CFGS|180,00 €||||
  
### DATE

Ens pot interessar filtrar per una data que podem anar substituint d’una cel·la, amb això CONTAINS ens donarà problemes, haurem de convertir la data a TEXT

```
=QUERY(nomFull!A1:F11;"Select A,B,C,D,E,F WHERE (C = date '"&TEXTO($B$14;"yyyy-MM-dd") &"') Order by C"; -1)
```

|AutoCodi|Concepte|Data|Dades|Import|Acumulat|
|---|---|---|---|---|---|

|AutoCodi|Concepte|Data|Dades|Import|Acumulat|
|---|---|---|---|---|---|
|1|TAXES CFGS|14/08/2020|1|360,00 €|360,00 €|
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|Query|Data|||||
||14/08/2020|||||

 
  