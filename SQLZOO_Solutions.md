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

Some simple queries:

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

Solutions of SELECT from WORLD SQLZOO :
Some simple queries to get you started

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


## SELECT FROM NOBEL TUTORIAL

1.

SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
 
2.

SELECT winner
FROM nobel
WHERE yr = 1962 AND subject = 'Literature'

3.

SELECT yr , subject
FROM nobel
WHERE winner = 'Albert Einstein'

4.

SELECT winner
FROM nobel
WHERE subject = 'peace' AND yr >= 2000

5.

SELECT *
FROM nobel
WHERE subject = 'Literature' AND yr BETWEEN 1980 AND 1989

6.

SELECT * FROM nobel
WHERE subject = 'Peace' AND winner IN ('Barack Obama' , 'Jimmy Carter' , 'Woodrow Wilson' , 'Theodore Roosevelt')

7.

SELECT winner
FROM nobel
WHERE winner LIKE 'John%'

8.

SELECT yr , subject , winner
FROM nobel
WHERE (yr = 1984 AND subject = 'chemistry') OR (yr = 1980 AND subject = 'physics')


9.

SELECT yr , subject , winner
FROM nobel
WHERE yr = 1980 AND NOT subject IN ('chemistry' , 'medicine')

10.

SELECT yr , subject , winner 
FROM nobel 
WHERE (subject = 'Literature' AND yr >= 2004) OR ( subject ='Medicine' AND yr < 1910 )


11.

SELECT *
FROM nobel
WHERE winner LIKE 'PETER GR%'

12.

SELECT *
FROM nobel
WHERE winner LIKE 'EUGENE O%'

13.

SELECT winner , yr, subject
FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC , winner

14.

SELECT winner, subject
  FROM nobel
 WHERE yr=1984 
 ORDER BY  subject IN ('Chemistry' , 'physics') ,subject , winner 
 
 
 ## SELECT IN SELECT
 
 1.
 
 SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
      
2.

SELECT name 
FROM world
WHERE continent = 'Europe' AND gdp/population > (SELECT gdp/population FROM world WHERE name = 'United Kingdom') 

3.

SELECT name , continent 
FROM world 
WHERE continent IN (SELECT continent 
FROM world 
WHERE name IN ('Argentina' , 'Australia'))
ORDER BY name

4.

SELECT name , population
FROM world
WHERE population > (SELECT population 
FROM world
WHERE name = 'United Kingdom') AND population < (SELECT population FROM world WHERE name = 'Germany')

5.

SELECT name, CONCAT(ROUND(population/(SELECT population FROM world
                          WHERE name = 'Germany')*100,0),'%')
             FROM world WHERE continent = 'Europe'
             


6.

SELECT name
FROM world
WHERE gdp > ALL(SELECT gdp
                FROM world 
                WHERE continent = 'Europe' AND gdp > 0)
                
 7.
 
 
 SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)


8.

SELECT continent , name
FROM world x
WHERE name <= ALL(SELECT name
FROM world y
WHERE y.continent = x.continent) 

9.

SELECT name , continent , population
FROM world x
WHERE 25000000  >= ALL(SELECT population FROM world y WHERE y.continent = x.continent AND y.population >0)

10

SELECT name , continent
FROM world x
WHERE population > ALL(SELECT 3*population FROM world y WHERE y.continent = x.continent AND y.name != x.name)
            

Stay Tuned for more!!
