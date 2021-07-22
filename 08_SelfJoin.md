# Self JOIN

#### Edinburgh Buses
Details of the database Looking at the data [here](https://sqlzoo.net/wiki/Edinburgh_Buses.).

```
stops(id, name)
route(num, company, pos, stop)
```

|stops|
|--|
|id|
|name|

|route|
|--|
|num|
|company|
|pos|
|stop|

1. How many stops are in the database.

```
SELECT COUNT(id) 
FROM stops
```

2. Find the id value for the stop 'Craiglockhart'

```
SELECT id 
FROM stops 
WHERE name='Craiglockhart'
```

3. Give the id and the name for the stops on the '4' 'LRT' service.

```
SELECT stops.id, stops.name 
FROM stops LEFT JOIN route ON stops.id=route.stop 
WHERE num='4' AND company = 'LRT'
```

4. Routes and stops

The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these **stops** have a count of 2. Add a HAVING clause to restrict the output to these two routes.

```
SELECT company, num, COUNT(*)
FROM route 
WHERE stop=149 OR stop=53
GROUP BY company, num
HAVING count(*) > 1
```

5. Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.

```
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON (a.company=b.company AND a.num=b.num)
WHERE a.stop=53 and b.stop = 149
```

6. The query shown is similar to the previous one, however by joining two copies of the **stops** table we can refer to **stops** by **name** rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'

```
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON (a.company=b.company AND a.num=b.num)
    JOIN stops stopa ON (a.stop=stopa.id)
        JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' and stopb.name='London Road'
```

7. Using a self join

Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')

```
SELECT DISTINCT a.company, a.num 
FROM route a JOIN route b ON (a.company=b.company AND a.num=b.num)
    JOIN stops stopa ON (a.stop=stopa.id)
        JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Haymarket' AND stopb.name='Leith'; 
```

8. Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'

```
SELECT DISTINCT a.company, a.num 
FROM route a JOIN route b ON (a.company=b.company AND a.num=b.num)
    JOIN stops stopa ON (a.stop=stopa.id)
        JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' AND stopb.name='Tollcross';
```

9. Give a distinct list of the **stops** which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.

```
SELECT stopb.name, a.company, a.num 
FROM route a JOIN route b ON (a.company=b.company AND a.num=b.num)
    JOIN stops stopa ON (a.stop=stopa.id)
        JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' AND a.company = 'LRT'
```

10. Find the routes involving two buses that can go from Craiglockhart to Lochend.
Show the bus no. and company for the first bus, the name of the stop for the transfer,
and the bus no. and company for the second bus.

```
SELECT r1.num, r1.company,  s1.name, r4.num, r4.company
FROM route r1 JOIN route r2 ON r1.num=r2.num AND r1.company=r2.company
    JOIN stops s1 ON r2.stop=s1.id
        JOIN route r3 ON s1.id=r3.stop
            JOIN route r4 ON r3.num=r4.num AND r3.company=r4.company
WHERE r1.stop=(SELECT id FROM stops WHERE name='Craiglockhart')
    AND r4.stop=(SELECT id FROM stops WHERE name='Lochend')
```

Hint [here](https://hackernoon.com/learning-self-join-queries-with-sqlzoo-xc163ue7)

## Quiz

1. Select the code that would show it is possible to get from Craiglockhart to Haymarket

```
SELECT DISTINCT a.name, b.name
FROM stops a JOIN route z ON a.id=z.stop
    JOIN route y ON y.num = z.num
        JOIN stops b ON y.stop=b.id
WHERE a.name='Craiglockhart' AND b.name ='Haymarket'
```

2. Select the code that shows the stops that are on route.num '2A' which can be reached with one bus from Haymarket?

```
SELECT S2.id, S2.name, R2.company, R2.num
FROM stops S1, stops S2, route R1, route R2
WHERE S1.name='Haymarket' AND S1.id=R1.stop
    AND R1.company=R2.company AND R1.num=R2.num
        AND R2.stop=S2.id AND R2.num='2A'
```

3. Select the code that shows the services available from Tollcross?

```
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON (a.company=b.company AND a.num=b.num)
    JOIN stops stopa ON (a.stop=stopa.id)
        JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Tollcross'
```

##### *keep trying, knowledge is awesome*  :facepunch: