
---
date: 
    created: 2026-05-12

title: '0487-RA4-A01_Enunciat-GestKaixa'

author: jmong

layout: post

summary: Activitats: Fase 1 i Fase 2

categories:
    - Uncategorized

tags:
    - activitat
    - theblueblox
---


# Activitat RA4-A01 - GestKaixa

## 1. Objectiu de l'activitat

En aquesta activitat es desenvoluparà una aplicació de consola en C# anomenada `GestKaixa` que permetrà gestionar una base de dades bancària simplificada.

L'aplicació haurà de connectar-se a la base de dades `kaixa` i permetre el següent:

## FASE 1

- Autenticar usuaris.
- Gestionar comptes bancaris.
- Consultar saldos.
- Registrar moviments.
- Visualitzar alertes.
- Administrar usuaris i comptes.

Aquesta pràctica integra els continguts següents del mòdul:

- Connexió a bases de dades des de C#.
- Programació orientada a objectes.
- Triggers i lògica de negoci en SQL.
- Refactorització.
- Control de versions amb Git i GitHub.
- Documentació tècnica en Markdown.


# 2. Material proporcionat

Es facilitarà l'arxiu:

- [kaixa.sql](https://drive.google.com/file/d/16eFb5MU9tdDsk73taVgg6cSGf0CnwNiT/view?usp=sharing)
- [kaixa.png](https://drive.google.com/file/d/1W4mlDd8fQFPQmFLOIUg3n5rFqbc_xcsA/view?usp=sharing)


Aquest script crearà:

- La base de dades `kaixa`.
- Les taules necessàries.
- Les relacions entre taules.
- La vista `VistaSaldos`.
- Els triggers de validació i control.

La base de dades es lliurarà sense registres.


# 3. Preparació inicial

## 3.1 Importació de la base de dades

```bash
mysql -u root -p < kaixa.sql
```

## 3.2 Creació de l'usuari d'aplicació

```sql
CREATE USER 'cashbox_app'@'%' IDENTIFIED BY 'app123';

GRANT ALL PRIVILEGES ON kaixa.* TO 'cashbox_app'@'%';

FLUSH PRIVILEGES;
```

## 3.3 Creació del projecte

```bash
dotnet new console -n GestKaixa
cd GestKaixa
dotnet add package MySql.Data
```

# 4. Repositori GitHub

L'alumnat crearà al seu compte de GitHub, un repositori privat anomenat:

```text
GestKaixa
```

El repositori s'haurà de compartir amb mi ( **operatiu** ) com a col·laborador.

![img_post](https://res.cloudinary.com/dbuv9r3p5/image/upload/v1778603597/share_repository_ybfbqh.png "share_repository")

El projecte s'haurà de versionar mitjançant Git i etiquetes (`tags`).


# 5. Requisits funcionals

## 5.1 Accés al sistema

L'aplicació ha de permetre dos tipus d'accés:

### Administrador
Usuari:
```text
cashbox_app
```

Password:
```text
app123
```

### Clients
Usuaris registrats a la taula `Usuaris`.


# 6. Funcionalitats de l'usuari client

Un usuari autenticat podrà:

1. Veure els seus comptes.
2. Consultar el saldo.
3. Veure moviments.
4. Fer un ingrés.
5. Fer una retirada.
6. Veure alertes.
0. Sortir.


# 7. Funcionalitats de l'administrador

L'usuari administrador podrà:

1. Registrar nous usuaris.
2. Obrir nous comptes.
3. Assignar usuaris a comptes.
4. Llistar tots els usuaris i els comptes associats.
0. Sortir.


# 8. Estructura mínima del programa

Les primeres versions es poden implementar en un únic fitxer  de codi font tipus, `Program.cs`.
S'ha de diferenciar correctament l'accés a la BBDD, el menú dels usuaris (clients) i el menú de l'usuari administrador dels comptes bancaris.

# 9. Versions obligatòries

El desenvolupament s'ha de realitzar com a mínim en tres versions.

## Versió 1.0

Funcionalitats mínimes:

- Connexió a la base de dades.
- Login d'usuaris.
- Visualització dels comptes associats.
- Consulta de saldo.
- Ingressos i retirades.
- Visualització de moviments.
- Visualització d'alertes.

Etiqueta Git:
```text
v1.0
```

## Versió 1.1

Millores:

- Accés com a administrador.
- Registre de nous usuaris.
- Obertura de nous comptes.
- Assignació d'usuaris a comptes.

Etiqueta Git:
```text
v1.1
```

## Versió 1.2

Millores:

- Llistat complet d'usuaris des del menú de l'administrador de comptes.
- Visualització dels comptes associats.
- Millores d'usabilitat.
- Ocultació del password amb asteriscs.

Etiqueta Git:
```text
v1.2
```


# 10. Documentació obligatòria

Cada versió haurà d'incloure un document Markdown:

- `v1.0.md`
- `v1.1.md`
- `v1.2.md`

També es pot utilitzar:

- `README_v1.0.md`
- `README_v1.1.md`
- `README_v1.2.md`

## Contingut mínim de cada document

### Descripció de la versió
Explicació del funcionament del programa.

### Estructura del codi
Classes i mètodes principals.

### Novetats
Només a partir de la versió 1.1.

### Jocs de proves
Execucions reals del programa en cada versió.

### Verificació a MySQL
Consultes SQL que demostrin que els resultats són correctes.

### Conclusions
Comentaris sobre el funcionament i les dificultats trobades.


# 11. Exemple de joc de proves

## Execució del programa

```text
Usuari: anna
Password: ****

1. Veure comptes
2. Consultar saldo
3. Veure moviments
4. Fer ingrés
5. Fer retirada
6. Veure alertes
0. Sortir

Opció: 4
Import de l'ingrés: 1000
Concepte: Nòmina

Moviment registrat correctament.
```

## Verificació en MySQL

```sql
SELECT * FROM Moviments ORDER BY id DESC LIMIT 1;
```

```sql
SELECT * FROM VistaSaldos;
```

```sql
SELECT * FROM Alertes ORDER BY id DESC;
```


# 12. Estructura recomanada del repositori

```text
GestKaixa/
├── Program.cs
├── GestKaixa.csproj
├── v1.0.md
├── v1.1.md
├── v1.2.md
└── .gitignore
```


# 13. Publicació de versions

Per a cada versió:

```bash
git add .
git commit -m "Versió 1.0"
git tag v1.0
git push origin main --tags
```

Repetir el mateix procés amb:

- `v1.1`
- `v1.2`

També es pot gestionar amb GitHub Desktop i crear els tags i les version a GitHub (web).

# 14. Criteris d'avaluació

## Funcionament de l'aplicació
- Login correcte.
- Connexió a la base de dades.
- Execució de totes les funcionalitats.

## Qualitat del codi
- Organització.
- Llegibilitat.
- Ús de classes i mètodes.

## Ús de Git i GitHub
- Repositori privat.
- Versions etiquetades.
- Historial coherent.

## Documentació
- Completa i clara.
- Amb jocs de proves.
- Amb comprovacions SQL.

## Validació funcional
- Coherència entre sortides del programa i dades reals a la base de dades.


# 15. Lliurament

Cal lliurar:

1. Repositori privat `GestKaixa`.
2. Compartició de col·laboració correcta.
3. Etiquetes:
   - `v1.0`
   - `v1.1`
   - `v1.2`
4. Documents Markdown corresponents.


# 16. Recomanacions

Es recomana:

- Desenvolupar i provar cada funcionalitat abans de continuar.
- Realitzar commits freqüents.
- Documentar immediatament cada versió.
- Verificar sempre els resultats amb consultes SQL.


# 17.  FASE 2

Els alumnes que finalitzin la primera fase, poden implementar la segona:

- Generació automàtica del número de compte.
- Registre de l'usuari que realitza cada moviment.
- Sol·licituds pendents per afegir titulars o autoritzats.
- Refactorització en múltiples fitxers.
- Tests automatitzats.

Les etiquetes de versió començaran en **v2.0**

# 18. Resultat final esperat

En finalitzar la pràctica, l'alumne haurà desenvolupat una aplicació realista de gestió bancària que integra:

- C#
- MySQL
- Triggers
- Git
- GitHub
- Documentació tècnica
- Refactorització

Aquesta activitat constitueix la síntesi final dels continguts treballats al mòdul i en concret a l'RA4 de Refactorització i control de versions
