# SUM and COUNT

**World Country Profile: Aggregate functions**

This tutorial is about aggregate functions such as `COUNT`, `SUM` and `AVG`. An aggregate function takes many values and delivers just one value. For example the function SUM would aggregate the values 2, 4 and 5 to deliver the single value 11.

|name | continent | area | population | gdp |
|:--|:--|:--|:--|:--|
|Afghanistan | Asia | 652230 | 25500100 | 20343000000 |
|Albania | Europe | 28748 | 2831741 | 12960000000 |
|Algeria | Africa | 2381741 | 37100000 | 188681000000 |
|Andorra | Europe | 468 | 78115 | 3712000000 |
|Angola | Africa | 1246700 | 20609294 | 100990000000 |
|...|...|...|...|...|

1. Total world population

Show the total population of the world.

```
world(name, continent, area, population, gdp)
```

```
SELECT SUM(population)
FROM world
```


2. List of continents
List all the continents - just once each.

```
SELECT DISTINCT continent 
FROM world
```

3. GDP of Africa
Give the total GDP of Africa

```
SELECT SUM(gdp) 
FROM world 
WHERE continent='Africa'
```

4. Count the big countries
How many countries have an area of at least 1000000

```
SELECT COUNT(name) 
FROM world
WHERE area >= 1000000
```

5. Baltic states population
What is the total population of ('Estonia', 'Latvia', 'Lithuania')

```
SELECT SUM(population) 
FROM world 
WHERE name IN ('Estonia','Latvia','Lithuania')
```


6. Counting the countries of each continent
For each continent show the continent and number of countries.

```
SELECT continent, COUNT(name) 
FROM world 
GROUP BY continent
```

7. Counting big countries in each continent
For each continent show the continent and number of countries with populations of at least 10 million.

```
SELECT continent, COUNT(name) 
FROM world 
WHERE population>10000000 
GROUP BY continent
```

8. Counting big continents
List the continents that have a total population of at least 100 million.

```
SELECT DISTINCT continent 
FROM world x 
WHERE 100000000 <
    (SELECT SUM(population) FROM world y WHERE x.continent=y.continent)
```

