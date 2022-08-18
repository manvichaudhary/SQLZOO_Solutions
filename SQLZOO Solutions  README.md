# SQLZOO_Solutions
I have compiled the solutions for SQLZOO tutorial

## Sections:
### SELECT basics
### SELECT from WORLD
### SELECT from NOBEL
### SELECT in SELECT
### SUM and COUNT
### JOIN
### More JOIN
### Using NULL
### Self JOIN


## SELECT basics:

Solutions of SELECT basics in SQLZOO:

1.

SELECT population FROM world
  WHERE name = 'Germany'
  
2.

SELECT name, population FROM world
  WHERE name IN ('Sweden' , 'Norway' , 'Denmark');
  
3.

SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
 
 


## SELECT from WORLD:


Some simple queries to get you started:
Solutions of SELECT FROM WORLD table from SQLZOO 

1. 

SELECT name, continent, population FROM world


2.

SELECT name 
FROM world
WHERE population >= 200000000

3.

SELECT name , gdp/population AS per_capita_gdp
FROM world
WHERE population >= 200000000

4. 

SELECT name , (population/1000000) AS population_in_millions
FROM world
WHERE continent ='South America'

5.

SELECT name , population
FROM world
WHERE name IN ('France' , 'Germany' , 'Italy')


6.

SELECT name 
FROM world
WHERE name LIKE '%United%'

7. 

SELECT name , population , area
FROM world
WHERE area > 3000000 OR population > 250000000

8.

SELECT name , population , area 
FROM ( SELECT name , population , area , 
CASE WHEN population > 250000000 THEN 1 ELSE 0 END AS pop_is_big ,
CASE WHEN area > 3000000 THEN 1 ELSE 0 END AS area_is_big
FROM world)Derviedareapop
WHERE ((pop_is_big = 1 OR area_is_big = 1) AND NOT ( pop_is_big = 1 AND area_is_big = 1))

9.

SELECT name , ROUND(population / 1000000 , 2) AS pop_million , ROUND(gdp/1000000000 , 2) AS pop_billion
FROM world
WHERE continent = 'South America' 

10.

SELECT name , ROUND(gdp/population , -3) AS pop_per_capita
FROM world
WHERE gdp > 1000000000000

11.

SELECT name , capital
FROM world
WHERE LENGTH(name) = LENGTH(capital)

12.

SELECT name , capital 
FROM world
WHERE LEFT(name , 1) = LEFT(capital , 1) AND name <> capital 

13.

SELECT name
FROM world
WHERE name LIKE '%a%' AND name LIKE '%e%' AND name LIKE '%i%' AND name LIKE '%o%' AND name LIKE '%u%' AND NOT ( name LIKE '% %')


