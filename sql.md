 # SQL
 
 source : https://sqlbolt.com

 SQL est un langage conçu pour permettre à la fois des utilisateurs pour __interroger, manipuler et transformer__ des données à partir d’une base de données relationnelle.

 ## Requêtes SELECT 101

 Pour récupérer des données d’une base de données SQL, nous devons écrire des instructions, qui sont souvent familières appelés requêtes.

### Sélectionner une requête pour des colonnes spécifiques:

 Et étant donné une table de données, la requête la plus basique que nous pourrions écrire serait celle qui sélectionne pour un couple colonnes (propriétés) de la table avec toutes les lignes (instances). 

```sql
SELECT column, another_column, …
FROM mytable;
```
Le résultat de cette requête sera un ensemble bidimensionnel de lignes et de colonnes, en fait une copie de la table, mais uniquement avec les colonnes que nous avons demandées.

### Sélectionner la requête pour toutes les colonnes :

Si nous voulons récupérer absolument toutes les colonnes de données d’une table, nous pouvons alors utiliser l’astérisque () abréviation au lieu de lister tous les noms de colonnes individuellement.*

```sql
SELECT * 
FROM mytable;
```
Cette requête, en particulier, est vraiment utile car c’est un moyen simple d’inspecter une table en dumpant toutes les données en une seule fois

##  Requêtes avec contraintes

### Sélectionner une requête avec des contraintes

```sql
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```

| Operator               | Condition                                       | SQL Example                        |
|-----------------------|-------------------------------------------------|------------------------------------|
| =, !=, <, <=, >, >=   | Opérateurs numériques standard                   | col_name != 4                      |
| BETWEEN … AND …       | Le nombre est compris entre deux valeurs (incluses) | col_name BETWEEN 1.5 AND 10.5     |
| NOT BETWEEN … AND …   | Le nombre n’est pas compris dans la plage de deux valeurs (incluses) | col_name NOT BETWEEN 1 AND 10  |
| IN (…)                | Le numéro existe dans une liste                       | col_name IN (2, 4, 6)              |
| NOT IN (…)            | Le numéro n’existe pas dans une liste                | col_name NOT IN (1, 3, 5)         |

#### exemple :

__Trouvez le film avec une rangée de 6 id :__

```sql
SELECT title FROM movies
WHERE id = 6;
```
__Retrouvez les films sortis dans les années 2000 et 2010 année :__

```sql
SELECT title FROM movies
WHERE year BETWEEN 2000 AND 2010;
```

__Retrouvez les films non sortis dans les années 2000 et 2010 year__

```sql
SELECT title FROM movies
WHERE year NOT BETWEEN 2000 AND 2010;
``` 

__Retrouvez les 5 premiers films Pixar et leur sortie year__

```sql
SELECT title, year FROM movies 
WHERE YEAR <= 2003;
```

## Requêtes avec contraintes (Partie 2)

| Operator         | Condition (fr)                                                          | Example                                 |
|------------------|--------------------------------------------------------------------------|-----------------------------------------|
| =                | Comparaison exacte sensible à la casse (attention : simple égal)        | col_name = "abc"                        |
| != or <>         | Inégalité exacte sensible à la casse                                    | col_name != "abcd"                      |
| LIKE             | Comparaison insensible à la casse                                       | col_name LIKE "ABC"                      |
| NOT LIKE         | Inégalité insensible à la casse                                         | col_name NOT LIKE "ABCD"                 |
| %                | Sert à remplacer une suite de caractères (avec LIKE ou NOT LIKE)       | col_name LIKE "%AT%"<br>(correspond à "AT", "ATTIC", "CAT" ou même "BATS") |
| _                | Sert à remplacer un seul caractère (avec LIKE ou NOT LIKE)             | col_name LIKE "AN_"<br>(correspond à "AND", mais pas à "AN") |
| IN (…)           | La chaîne existe dans une liste                                         | col_name IN ("A", "B", "C")              |
| NOT IN (…)       | La chaîne n’existe pas dans une liste                                   | col_name NOT IN ("D", "E", "F")          |

#### exercice :

__Retrouvez tous les films Toy Story:__ 
```sql
SELECT title FROM movies
where title LIKE "%Toy Story%";
```
__Retrouvez tous les films réalisés par John Lasseter :__

```sql
SELECT title FROM movies
where Director LIKE "%John Lasseter%";
```

__Retrouvez tous les films (et réalisateurs) non réalisés par John Lasseter :__

```sql
SELECT title FROM movies
where Director NOT LIKE "%John Lasseter%";
```

__Retrouvez tous les films WALL-*__

```sql
SELECT title FROM movies
where title like"WALL-%";
```


## Filtrage et tri des résultats d’une requête

Dans de tels cas, SQL offre un moyen pratique d’ignorer les lignes qui ont une valeur de colonne en double à l’aide du mot-clé. __DISTINCT__

### Sélectionner une requête avec des résultats  :

```sql
SELECT DISTINCT column, another_column, …
FROM mytable
WHERE condition(s);
```
Étant donné que le mot-clé supprimera aveuglément les lignes en double, nous l’apprendrons dans une prochaine leçon Comment supprimer les doublons basés sur des colonnes spécifiques à l’aide du regroupement et de la clause. __DISTINCTGROUP BY__

### Classement des résultats

Pour vous aider, SQL fournit un moyen de trier vos résultats par une colonne donnée en ordre croissant ou décroissant à l’aide de la clause.__ORDER BY__

Sélectionner une requête avec des résultats ordonnés : 
```sql
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC;
``` 
### Limitation des résultats à un sous-ensemble
### Sélectionner une requête avec des lignes limitées :

Une autre clause qui est couramment utilisée avec la clause sont les clauses et qui sont une optimisation utile pour indiquer à la base de données le sous-ensemble des résultats qui vous intéressent.
réduira le nombre de lignes à renvoyer, et l’optionnel spécifiera où Commencez à compter le nombre de rangées.__ORDER BY LIMIT OFFSET LIMIT OFFSET__

```sql
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```
| Objectif                                                           | Clause SQL              | Explication                                                             |
| ------------------------------------------------------------------ | ----------------------- | ----------------------------------------------------------------------- |
| Supprimer les doublons                                             | `DISTINCT`              | Retourne uniquement les lignes uniques selon les colonnes sélectionnées |
| Trier les résultats en ordre croissant (A→Z ou 0→9)                | `ORDER BY colonne ASC`  | `ASC` = *Ascending* (croissant)                                         |
| Trier les résultats en ordre décroissant (Z→A ou 9→0)              | `ORDER BY colonne DESC` | `DESC` = *Descending* (décroissant)                                     |
| Limiter le nombre de lignes retournées                             | `LIMIT nombre`          | Exemple : `LIMIT 10` → retourne seulement 10 lignes                     |
| Ignorer un certain nombre de lignes avant d’afficher les résultats | `OFFSET nombre`         | Exemple : `OFFSET 10` → commence à partir de la 11ᵉ ligne               |

#### exercice :

__Liste de tous les réalisateurs de films Pixar (par ordre alphabétique), sans doublons:__

```sql 
SELECT distinct director FROM movies
ORDER BY director asc;
``` 

__Énumérez les quatre derniers films Pixar sortis (du plus récent au moins récent) :__ 

```sql
SELECT distinct title, year FROM movies
ORDER BY year desc
limit 4;
``` 
__Répertoriez les cinq premiers films Pixar par ordre alphabétique :__
```sql
SELECT distinct title FROM movies
ORDER BY title asc
limit 5;
```
__Énumérez les cinq prochains films Pixar classés par ordre alphabétique :__

```sql
