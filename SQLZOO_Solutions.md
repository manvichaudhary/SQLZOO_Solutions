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


## SUM and COUNT

1.

SELECT SUM(population)
FROM world

2.

SELECT DISTINCT continent
FROM world

3.

SELECT SUM(gdp) 
FROM world
WHERE continent = 'Africa'

4.

SELECT COUNT(name)
FROM world
WHERE area >= 1000000

5.

SELECT SUM(population)
FROM world
WHERE name IN ('Estonia' , 'Latvia' , 'Lithuania')


6.

SELECT continent , COUNT(name)
FROM world
GROUP BY continent

7.

SELECT continent , COUNT(name)
FROM world
WHERE population >= 10000000
GROUP BY continent

8.

SELECT continent 
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000


## JOINS

1.

SELECT matchid , player FROM goal 
  WHERE teamid = 'GER'
  
2.

SELECT id,stadium,team1,team2
  FROM game
WHERE id = 1012

3.

SELECT player,teamid , stadium , mdate
  FROM game JOIN goal ON (id=matchid)
WHERE teamid = 'GER'


4.

SELECT g.team1 , g.team2 , goal.player 
FROM game AS g
JOIN goal
ON g.id = goal.matchid
WHERE goal.player LIKE 'Mario%'

5.

SELECT player, teamid,coach, gtime
  FROM goal 
JOIN eteam 
ON teamid = id 
 WHERE gtime<=10
 
 
6.

SELECT game.mdate , eteam.teamname
FROM game
JOIN eteam
ON game.team1 = eteam.id
WHERE eteam.coach = 'Fernando Santos'

7.

SELECT goal.player 
FROM goal
JOIN game
ON goal.matchid = game.id
WHERE stadium ='National Stadium, Warsaw'


8.

SELECT DISTINCT goal.player 
FROM goal
JOIN game
ON goal.matchid = game.id
WHERE (team1='GER' OR team2 ='GER') AND
teamid != 'GER'

9.

SELECT eteam.teamname , COUNT(*)
FROM eteam
JOIN goal
ON eteam.id = goal.teamid
GROUP BY eteam.teamname

10.

SELECT game.stadium , COUNT(*)
FROM game
JOIN goal
ON game.id = goal.matchid
GROUP BY game.stadium

11.

SELECT goal.matchid , game.mdate , COUNT(*)
FROM goal
JOIN game
ON goal.matchid = game.id
WHERE game.team1 = 'POL' OR game.team2 = 'POL'
GROUP BY goal.matchid , game.mdate

12.

SELECT matchid, mdate, COUNT(*) FROM goal
  JOIN game ON (matchid=id)
  WHERE teamid = 'GER'
  GROUP BY matchid, mdate
            
            
## MORE JOINS

1.

SELECT id , title
FROM movie
WHERE yr = 1962

2.

SELECT yr
FROM movie
WHERE title = 'Citizen Kane'

3.

SELECT id , title , yr
FROM movie
WHERE title LIKE'%Star Trek%'
ORDER BY yr

4.

SELECT id 
FROM actor
WHERE name = 'Glenn Close'

5.

SELECT id
FROM movie
WHERE title = 'Casablanca'

6.

SELECT DISTINCT actor.name
FROM actor
JOIN casting
ON actor.id = casting.actorid
WHERE casting.movieid = 11768

7.

SELECT actor.name
FROM casting
JOIN actor ON casting.actorid = actor.id
JOIN movie ON casting.movieid = movie.id
WHERE movie.title = 'Alien'

8.

SELECT movie.title
FROM casting
JOIN movie ON casting.movieid = movie.id 
JOIN actor ON casting.actorid = actor.id
WHERE actor.name ='Harrison Ford'

9.

SELECT movie.title
FROM casting
JOIN actor 
ON casting.actorid = actor.id
JOIN movie
ON casting.movieid = movie.id
WHERE actor.name = 'Harrison Ford' AND ord != 1

10.

SELECT movie.title , actor.name 
FROM casting 
JOIN movie 
ON casting.movieid = movie.id
JOIN actor
ON casting.actorid = actor.id
WHERE movie.yr = 1962 AND casting.ord = 1

11.

SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
        JOIN actor   ON actorid=actor.id
WHERE name='Rock Hudson'
GROUP BY yr
HAVING COUNT(title) > 2


12.

SELECT title , name
FROM movie JOIN casting ON (movieid=movie.id AND ord=1)
JOIN actor ON (actorid=actor.id)
WHERE movie.id IN (
      SELECT movieid FROM casting
      WHERE actorid IN (
      SELECT id FROM actor
      WHERE name = 'Julie Andrews'))
      
      
  13.
  
  SELECT actor.name 
FROM casting
JOIN actor
ON casting.actorid = actor.id
WHERE casting.ord = 1 
GROUP BY actor.name
HAVING COUNT(actor.name) >= 15
ORDER BY actor.name

14.

SELECT movie.title , COUNT(actorid) AS actors 
FROM movie
JOIN casting
ON movie.id = casting.movieid
WHERE yr = 1978
GROUP BY title
ORDER BY actors DESC , title

15.

SELECT distinct actor.name
FROM movie
JOIN casting
ON casting.movieid = movie.id
JOIN actor
ON actor.id = casting.actorid
where movie.id in (select movieid from casting join actor on id =actorid where 
actor.name = 'Art Garfunkel') and actor.name <> 'Art Garfunkel'



## USING NULL

1.

SELECT name
FROM teacher 
WHERE dept IS NULL

2.

SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)
           

3.

SELECT teacher.name , dept.name
FROM teacher
LEFT JOIN dept
ON teacher.dept = dept.id


4.

SELECT teacher.name , dept.name
FROM teacher
RIGHT JOIN dept
ON teacher.dept = dept.id

5.

SELECT teacher.name , COALESCE(teacher.mobile , '07986 444 2266') FROM teacher

6.

SELECT teacher.name , COALESCE(dept.name , 'None')
FROM teacher
LEFT JOIN dept
ON teacher.dept = dept.id

7.

SELECT COUNT(name) , COUNT(mobile)
FROM teacher

8.

SELECT  dept.name , COUNT(teacher.dept)
FROM teacher
RIGHT JOIN dept
ON teacher.dept = dept.id
GROUP BY dept.name

9.

SELECT teacher.name , CASE WHEN dept = 1 OR dept = 2 THEN 'Sci' ELSE 'Art' END AS tech_foll
FROm teacher

10.

SELECT name , (CASE WHEN dept = 1 OR dept = 2 THEN 'Sci'
WHEN dept = 3 THEN 'Art' ELSE 'None' END) AS dept_123
FROM teacher




      
      
Stay Tuned for more!!
