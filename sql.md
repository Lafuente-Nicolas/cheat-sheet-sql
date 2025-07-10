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
SELECT Title FROM movies
ORDER BY Title asc
limit 5 offset 5;
```

## Révision SQL : requêtes SELECT simples

#### exercice : 

__Énumérer toutes les villes canadiennes et leurs populations__

```sql
SELECT city, population FROM north_american_cities
where country = "Canada";
```

__Classez toutes les villes des États-Unis par leur latitude du nord au sud__
```sql
SELECT city FROM north_american_cities
where country = "United States"
order by latitude desc
```

__Énumérez toutes les villes à l’ouest de Chicago, classées d’ouest en est__
```sql
SELECT city, longitude FROM north_american_cities
WHERE longitude < -87.629798
ORDER BY longitude ASC;
```

__Énumérer les deux plus grandes villes du Mexique (par population)__
```sql
SELECT city FROM north_american_cities
where country = "Mexico"
order by population desc
limit 2;
``` 
__Énumérez les troisième et quatrième plus grandes villes (par population) des États-Unis et leur population__
```sql
SELECT city, population FROM north_american_cities
where country = "United States"
order by population desc
limit 2 offset 2;
```
## Requêtes multi-tables avec JOIN

### Requêtes multi-tables avec jointures

Les tables qui partagent des informations sur une seule entité doivent disposer d’une clé primaire qui identifie cette entité de manière unique dans la base de données.

En utilisant la clause dans une requête, nous pouvons combiner les données de ligne de deux tables distinctes à l’aide de cette clé unique. La première des jointures que nous allons présenter est le __.JOININNER JOIN__
```sql
Sélectionner une requête avec INNER JOIN sur plusieurs tables
SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table 
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```
#### exercice :

__Retrouvez les ventes nationales et internationales de chaque film__

```sql 
SELECT title, domestic_sales, international_sales FROM movies
JOIN boxoffice ON movies.id = boxoffice.movie_id;
```

__Montrez les chiffres de vente de chaque film qui a mieux marché à l’international plutôt qu’au pays__

```sql
SELECT title, domestic_sales, international_sales FROM movies
JOIN boxoffice ON movies.id = boxoffice.movie_id
WHERE international_sales > domestic_sales;
```
__Listez tous les films par leurs notes par ordre décroissant__
```sql
SELECT title, rating FROM movies
JOIN boxoffice ON movies.id = boxoffice.movie_id
order by rating desc
```

## JOINTURES EXTERNES

Si les deux tables ont des données asymétriques, ce qui peut facilement se produire lorsque les données sont saisies dans des étapes, alors nous devrions utiliser un , ou au contraire pour nous assurer que Les données dont vous avez besoin ne sont pas laissées en dehors des résultats. __LEFT JOINRIGHT JOINFULL JOIN__
```sql
Sélectionner une requête avec des jointures GAUCHE/DROITE/COMPLÈTE sur plusieurs tables
SELECT column, another_column, …
FROM mytable
INNER/LEFT/RIGHT/FULL JOIN another_table 
    ON mytable.id = another_table.matching_id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```
Lorsque vous utilisez l’une de ces nouvelles jointures, vous devrez probablement écrire une logique supplémentaire pour gérer s dans le résultat et les contraintes (nous y reviendrons dans la leçon suivante). __NULL__

#### exercice :

__Retrouvez la liste de tous les bâtiments qui ont des employés :__
```sql
SELECT DISTINCT building FROM employees;
```

__Retrouvez la liste de tous les bâtiments et leur capacité__
```sql 
SELECT * FROM buildings;
``` 
__Énumérez tous les bâtiments et les rôles distincts des employés dans chaque bâtiment (y compris les bâtiments vides)__`

```sql
SELECT DISTINCT building_name, role FROM buildings 
LEFT JOIN employees ON building_name = building;
```

## Une courte note sur les valeurs NULL

```sql 
Sélectionner une requête avec des contraintes sur les valeurs NULL
SELECT column, another_column, …
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

__Trouver le nom et le rôle de tous les employés qui n’ont pas été affectés à un bâtiment__

```sql
SELECT name, role FROM employees
where building is null;
```
__Trouvez le nom des bâtiments qui n’emploient aucun employé__

```sql
select building_name from buildings
left join Employees on building_name = building 
where building is null
```
##  Requêtes avec des expressions

Ces expressions peuvent utiliser des et des fonctions de chaîne de caractères ainsi que de l’arithmétique de base pour transformer les valeurs lors de l’exécution de la requête, comme le montre cet exemple de physique.

```sql
Exemple de requête avec des expressions
SELECT particle_speed / 2.0 AS half_particle_speed
FROM physics_data
WHERE ABS(particle_position) * 10.0 > 500;
```
L’utilisation d’expressions peut faire gagner du temps et un post-traitement supplémentaire des données de résultat, mais peut également rendre plus difficile à lire, nous recommandons donc que lorsque des expressions sont utilisées dans la partie de la qu’on leur attribue également un alias descriptif à l’aide du mot-clé. __SELECTAS__
```sql
Sélectionner une requête avec des alias d’expression
SELECT col_expression AS expr_description, …
FROM mytable;
```
En plus des expressions, les colonnes régulières et même les tables peuvent également avoir des alias pour les rendre plus faciles pour référencer dans la sortie et dans le cadre de la simplification de requêtes plus complexes.
```sql
Exemple de requête avec des alias de nom de colonne et de table
SELECT column AS better_column_name, …
FROM a_long_widgets_table_name AS mywidgets
INNER JOIN widget_sales
  ON mywidgets.id = widget_sales.widget_id;
  ```
#### exercice : 

__Énumérez tous les films et leurs ventes combinées en millions de dollars :__

```sql
SELECT title, (domestic_sales + international_sales) / 1000000 AS millions
FROM movies
JOIN boxoffice ON movies.id = boxoffice.movie_id;
```

__Listez tous les films et leurs notes en pourcentage__
```sql 
SELECT title, Rating *10 AS percent
FROM movies
JOIN boxoffice ON movies.id = boxoffice.movie_id;
```
__Énumérez tous les films sortis les années paires__
```sql
SELECT title, year as even_years  FROM movies
WHERE year % 2 = 0;
```
## Requêtes avec des agrégats

SQL prend également en charge l’utilisation de Expressions d’agrégation (ou fonctions) qui vous permettent de résumer des informations sur un groupe de lignes de données. Avec la base de données Pixar que vous utilisez, des fonctions d’agrégation peuvent être __utilisées__ pour __répondre aux des questions__ telles que « Combien de films Pixar a-t-il produits ? » ou « Quel est le film Pixar le plus rentable ? chaque année ?`

```sql
Sélectionner une requête avec des fonctions d’agrégation sur toutes les lignes
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression;
```
| Fonction               | Description                                                                                                       |
|------------------------|-------------------------------------------------------------------------------------------------------------------|
| `COUNT(*)`, `COUNT(colonne)` | Fonction courante utilisée pour compter le nombre de lignes dans le groupe si aucun nom de colonne n’est spécifié. Sinon, compte le nombre de lignes du groupe avec des valeurs non NULL dans la colonne spécifiée. |
| `MIN(colonne)`         | Recherche la plus petite valeur numérique dans la colonne spécifiée pour toutes les lignes du groupe.             |
| `MAX(colonne)`         | Recherche la valeur numérique la plus élevée dans la colonne spécifiée pour toutes les lignes du groupe.          |
| `AVG(colonne)`         | Recherche la valeur numérique moyenne dans la colonne spécifiée pour toutes les lignes du groupe.                 |
| `SUM(colonne)`         | Recherche la somme de toutes les valeurs numériques dans la colonne spécifiée pour les lignes du groupe.          |

### Fonctions d’agrégation groupées

En plus d’agréger toutes les lignes, vous pouvez appliquer les fonctions d’agrégation à des groupes individuels de données au sein de ce groupe (c’est-à-dire les ventes au box-office pour les comédies par rapport aux films d’action).
Cela créerait alors autant de résultats qu’il y a de groupes uniques définis par la clause. __GROUP BY__
```sql
Sélectionner une requête avec des fonctions d’agrégation sur des groupes
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression
GROUP BY column;
```

#### exercice

__Trouvez la plus longue période d’ancienneté d’un employé dans le studio__
```sql 
SELECT	MAX(years_employed) FROM employees;
```
