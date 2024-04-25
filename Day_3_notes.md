# Excercise-1

## Filtering the columns

### Find the title of each film

```sql
SELECT title FROM movies;
```

### Find the director of each film

```sql
SELECT director FROM movies;
```

### Find the title and director of each film

```sql
SELECT title,director FROM movies;
```

### Find the title and year of each film

```sql
SELECT title,year FROM movies;
```

### Find all the information about each film

```sql
SELECT * FROM movies;
```

# Excecise - 2

## Queries with constraints (Pt. 1)

### Find the movie with a row id of 6

```sql
SELECT title FROM movies where id=6;
```

### Find the movies released in the years between 2000 and 2010

```sql
SELECT title FROM movies where year between 2000 and 2010;
```

### Find the movies not released in the years between 2000 and 2010

```sql
SELECT title FROM movies where year not between 2000 and 2010;
```

### Find the first 5 Pixar movies and their release year

```sql
SELECT title,year FROM movies where id in (1,2,3,4,5);
```

# Excecise - 3

## Queries with constraints (Pt. 2)

### Find all the Toy Story movies 

```sql
SELECT title FROM movies where title like "%toy story%";
```

### Find all the movies directed by John Lasseter

```sql
SELECT title,director FROM movies where director="John Lasseter";
```

### Find all the movies (and director) not directed by John Lasseter

```sql
SELECT title,director FROM movies where director!="John Lasseter";
```

### Find all the WALL-* movies

```sql
SELECT title FROM movies where title like "%WALL-%";
```

# Excercise-4

## Filtering and sorting Query results

### List all directors of Pixar movies (alphabetically), without duplicates

``` sql
SELECT DISTINCT DIRECTOR FROM movies ORDER BY DIRECTOR;
```

### List the last four Pixar movies released (ordered from most recent to least)

```sql
SELECT * FROM MOVIES ORDER BY YEAR DESC LIMIT 4;
```

### List the first five Pixar movies sorted alphabetically

```sql
SELECT title FROM MOVIES ORDER BY title asc LIMIT 5;
```

### List the next five Pixar movies sorted alphabetically

```sql
SELECT title FROM MOVIES ORDER BY title asc LIMIT 5 OFFSET 5;
```

# EXCERCISE - 5

## Simple SELECT Queries

### List all the Canadian cities and their populations 

```sql
SELECT * FROM north_american_cities WHERE COUNTRY LIKE "CANADA";
```

### Order all the cities in the United States by their latitude from north to south

```sql
SELECT CITY
FROM north_american_cities 
WHERE COUNTRY LIKE "UNITED STATES" 
ORDER BY LATITUDE DESC;
```

### List all the cities west of Chicago, ordered from west to east

```sql
SELECT city
FROM north_american_cities
WHERE longitude < -87.629798
ORDER BY longitude;
```

### List the two largest cities in Mexico (by population)

```sql
SELECT *
FROM north_american_cities
WHERE COUNTRY LIKE "MEXICO"
ORDER BY POPULATION DESC
LIMIT 2;
```

### List the third and fourth largest cities (by population) in the United States and their population

```sql
SELECT *
FROM north_american_cities
WHERE COUNTRY LIKE "UNITED STATES"
ORDER BY POPULATION DESC
LIMIT 2 OFFSET 2;
```

# EXCERCISE - 6

## Multi-table queries with JOINs

### Find the domestic and international sales for each movie

```sql
SELECT *
FROM movies
INNER JOIN BOXOFFICE
ON MOVIES.ID = BOXOFFICE.MOVIE_ID;
```

### Show the sales numbers for each movie that did better internationally rather than domestically

```sql
SELECT *
FROM movies
INNER JOIN BOXOFFICE
ON MOVIES.ID = BOXOFFICE.MOVIE_ID
WHERE INTERNATIONAL_SALES > DOMESTIC_SALES;
```

### List all the movies by their ratings in descending order

```sql
SELECT TITLE
FROM movies
INNER JOIN BOXOFFICE
ON MOVIES.ID = BOXOFFICE.MOVIE_ID
ORDER BY RATING DESC;
```

# EXCERCISE - 7

## OUTER JOINs

### Find the list of all buildings that have employees

```sql
SELECT DISTINCT BUILDING
FROM employees
;
```

### Find the list of all buildings and their capacity

```sql
SELECT BUILDING_NAME,CAPACITY
FROM BUILDINGS;
```

### List all buildings and the distinct employee roles in each building (including empty buildings)

```sql
SELECT DISTINCT BUILDING_NAME , ROLE
FROM BUILDINGS
INNER JOIN EMPLOYEES 
ON BUILDINGS.BUILDING_NAME = EMPLOYEES.BUILDING;
```

# EXCERCISE - 8

## A short note on NULLs

### Find the name and role of all employees who have not been assigned to a building

```sql
SELECT  NAME,ROLE FROM employees WHERE BUILDING IS NULL;
```

### Find the names of the buildings that hold no employees

```sql
SELECT DISTINCT building_name
FROM buildings
LEFT JOIN employees
    ON building_name = employees.building
WHERE employees.building IS NULL;
```

# EXCERCISE - 9

## Queries with expressions

### List all movies and their combined sales in millions of dollars

```sql
SELECT DISTINCT TITLE , (DOMESTIC_SALES+INTERNATIONAL_SALES)/1000000 AS COMBINED_SALES 
FROM MOVIES 
INNER JOIN BOXOFFICE ON MOVIES.ID = BOXOFFICE.MOVIE_ID ;
```

### List all movies and their ratings in percent

```sql
SELECT DISTINCT TITLE , (RATING*10) AS RAT 
FROM MOVIES 
INNER JOIN BOXOFFICE ON MOVIES.ID = BOXOFFICE.MOVIE_ID ;
```

### List all movies that were released on even number years

```sql
SELECT DISTINCT TITLE
FROM MOVIES 
INNER JOIN BOXOFFICE ON MOVIES.ID = BOXOFFICE.MOVIE_ID 
WHERE YEAR %2==0;
```

# EXCERCISE - 10

## Queries with aggregates (Pt. 1)

### Find the longest time that an employee has been at the studio

```sql
SELECT ROLE,MAX(YEARS_EMPLOYED) AS TIME FROM employees;
```

### For each role, find the average number of years employed by employees in that role

```sql
SELECT *,AVG(YEARS_EMPLOYED) FROM employees GROUP BY ROLE;
```

### Find the total number of employee years worked in each building

```sql
SELECT *,sum(YEARS_EMPLOYED) FROM employees GROUP BY BUILDING;
```

# EXCERCISE - 11

## Queries with aggregates (Pt. 2)

### Find the number of Artists in the studio (without a HAVING clause)

```sql
SELECT COUNT(ROLE) FROM employees WHERE ROLE LIKE "ARTIST";
```

### Find the number of Employees of each role in the studio

```sql
SELECT *,COUNT(NAME) FROM employees group by role;
```

### Find the total number of years employed by all Engineers

```sql
SELECT SUM(YEARS_EMPLOYED) FROM EMPLOYEES GROUP BY ROLE HAVING ROLE LIKE "ENGINEER";
```

# EXCERCISE - 12

## Order of execution of a Query

### Find the number of movies each director has directed

```sql
SELECT DIRECTOR,COUNT(TITLE) FROM movies GROUP BY DIRECTOR;
```

### Find the total domestic and international sales that can be attributed to each director

```sql
SELECT DIRECTOR,SUM(DOMESTIC_SALES+INTERNATIONAL_SALES) FROM movies 
LEFT JOIN BOXOFFICE 
ON ID = MOVIE_ID 
GROUP BY DIRECTOR;
```

# EXCERCISE - 13

## Inserting rows

### Add the studio's new production, Toy Story 4 to the list of movies (you can use any director)

```sql
INSERT INTO MOVIES (ID , TITLE , DIRECTOR , YEAR , LENGTH_MINUTES) 
VALUES(15 , "Toy Story 4" , "John Lasseter" , 2024 , 120)
```

### Toy Story 4 has been released to critical acclaim! It had a rating of 8.7, and made 340 million domestically and 270 million internationally. Add the record to the BoxOffice table.

```sql
INSERT INTO BOXOFFICE(MOVIE_ID , RATING,DOMESTIC_SALES,INTERNATIONAL_SALES) 
VALUES (15,8.7,340000000,270000000)
```

# EXCERCISE - 14

## Updating rows

### The director for A Bug's Life is incorrect, it was actually directed by John Lasseter

```sql
UPDATE MOVIES 
SET DIRECTOR = "John Lasseter"
WHERE ID=2
```

### The year that Toy Story 2 was released is incorrect, it was actually released in 1999

```sql
UPDATE MOVIES 
SET YEAR = 1999
WHERE TITLE LIKE "TOY STORY 2"
```

### Both the title and director for Toy Story 8 is incorrect! The title should be "Toy Story 3" and it was directed by Lee Unkrich

```sql
UPDATE MOVIES 
SET TITLE = "Toy Story 3",director = "Lee Unkrich"
WHERE TITLE = "Toy Story 8"
```

# EXCERCISE - 15

## Deleting rows

### This database is getting too big, lets remove all movies that were released before 2005.

```sql
DELETE FROM MOVIES
WHERE YEAR<2005;
```

### Andrew Stanton has also left the studio, so please remove all movies directed by him.

```sql
DELETE FROM MOVIES WHERE DIRECTOR="Andrew Stanton";
```

# EXCERCISE - 16

## Creating tables

### Create a new table named Database with the following columns:
- Name A string (text) describing the name of the database
- Version A number (floating point) of the latest version of this database
- Download_count An integer count of the number of times this database was downloaded

- This table has no constraints.

```sql

```