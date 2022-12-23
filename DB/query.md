# Topic 3
## Postgres
1)
```sql
SELECT count(*) FROM STOPS
WHERE latitude BETWEEN 59.965 AND 60.03 AND
longitude BETWEEN 30.276 and 30.352
```

2)
```sql
SELECT DISTINCT route_number FROM TRACK 
WHERE CARRIER_BOARD_NUM = 1320
```

3)
```sql
SELECT COUNT(DISTINCT CARRIER_BOARD_NUM) FROM TRACK
WHERE ROUTE_NUMBER = 14
```

4)
```sql
SELECT MIN(DISTANCE_BACK) FROM ROUTE_BY_STOPS
WHERE ROUTE_NUMBER = 46
```

5)
```sql
SELECT sum(DISTANCE_BACK) FROM ROUTE_BY_STOPS
WHERE id_direction = 2 AND ROUTE_NUMBER = 49 AND STOP_NUMBER BETWEEN 6 AND 10
```

6)
```sql
SELECT LATITUDE, LONGITUDE from STOPS 
WHERE ID_STOP IN 
(SELECT ID_STOP FROM ROUTE_BY_STOPS WHERE STOP_NUMBER = 19 and ID_DIRECTION = 1 and ROUTE_NUMBER = 191) 
```

7)
```sql
SELECT sum(DISTANCE_BACK) as sum_dist, ROUTE_NUMBER FROM ROUTE_BY_STOPS
WHERE ID_DIRECTION = 2 and id_vehicle = 0
GROUP BY ROUTE_NUMBER
HAVING sum(DISTANCE_BACK) between 10550 and 13780
```

# Topic 4

1)
-- Определите, сколько автобусов пройдет остановку УЛ. ЧАПЫГИНА [46А, 1]
-- с идентификатором 2394 в интервале [22:00, 23:00) 10 сентября 2019 года.
```sql
SELECT count(*)
FROM STOPS
JOIN TRACK 
ON STOPS.ID_STOP = TRACK.ID_STOP
WHERE STOP_NAME LIKE 'УЛ. ЧАПЫГИНА%'
AND TO_CHAR(STOP_TIME_REAL, 'DD/MM/YYYY') = '10/09/2019'
AND TO_CHAR(STOP_TIME_REAL, 'HH24:MI') BETWEEN '22:00' AND '22:59'
```
2)
-- Напишите запрос к базе Общественный транспорт и определите, сколько единиц общественного транспорта 
-- (с различными бортовыми номерами) работали на автобусном маршруте 49 10 сентября 2019 года в интервале [9:00, 10:00)?

```sql
SELECT COUNT(DISTINCT CARRIER_BOARD_NUM)
FROM TRACK
WHERE ROUTE_NUMBER = 49
AND ID_VEHICLE = 0
AND TO_CHAR(STOP_TIME_REAL, 'DD-MM-YYYY') = '10-09-2019'
AND TO_CHAR(STOP_TIME_REAL, 'HH24:MI') BETWEEN '09:00' AND '09:59'
```

3)
Давайте представим, что Вы стоите рядом с некоторой достопримечательностью Санкт-Петербурга, например Музей Эрарта, с заданными координатами (59.9322357, 30.2511635). Вычислите количество остановок, которые находятся от Вас на расстоянии не более 350 метров.
Координаты остановок находятся в таблице STOPS. Для вычисления сферического расстояния в базе данных Общественный Транспорт была создана функция CoordinateDistance. Спецификация функции CoordinateDistance (Latitude1, Longitude1, Latitude2, Longitude2)

```sql
SELECT count(*)
FROM STOPS
WHERE CoordinateDistance(59.9322357, 30.2511635, Latitude, Longitude) <= 350
```

# Topic 5
## REDIS
1. Ключу user_list соответствует список, составленный из посетителей сайта. Напишите запрос, который определит количество посетителей сайта.
	```sql
	LLEN user_list
	```

2. Ключам friends:Elena и friends:Boris соответствуют множества, содержащие сведения о друзьях Елены и Бориса. Напишите запрос, который сформирует (и выведет) множество всех друзей Елены и Бориса.
	```sql
	SUNION friends:Elena friends:Boris
	```
3. Ключу user_r_set было сопоставлено ранжированное множество с помощью следующей команды: ZADD user_r_set 12 Tom 5 Viktor 9 Sergey 8 Ivan 1 Alex 14 Serafima 13 Dan
Для какого из элементов ранжированного множества команда ZRANK выдаст значение 3?

	SERGEY


## MONGO db

1. Сколько в коллекции student документов, у которых значение поля age больше 20?
	```sql
	db.student.find({age: {$gt: 20}}).count() 
	```
2. Укажите имена студентов (из коллекции student), у которых возраст (поле age) равен 24.
	```sql
	db.student.find({age: {$eq: 24}}) 
	```
3. Сколько станций на линии "Piccadilly" в метро Лондона (коллекция UNDERGROUND).
	```sql
	db.UNDERGROUND.find({Line: /Piccadilly/i}).count()
	```

## Cassandra 

1. Как называется мультфильм, для которого идентификатор (поле cartoon_id) равен 6.
	```sql
	USE CARTOON;                                                                                                            
	SELECT CARTOON_NAME FROM CARTOON_BY_ID WHERE CARTOON_ID = 6; 
	```
2. Сколько просмотров мультфильмов соответствует режиссеру, для которого идентификатор (поле director_id) равен 4.
	```sql
	USE DIRECTOR;
	SELECT SUM(VIEWS) FROM CARTOON_BY_DIRECTOR_ID WHERE DIRECTOR_ID = 4;
	```
3. Сколько в базе мульфильмов, для которых выполнено условие country = 'France/UK'?
	```sql
	USE CARTOON;
	SELECT COUNT(*) FROM CARTOON_BY_COUNTRY WHERE COUNTRY = 'France/UK';
	```

## Neo4j

1. Сколько студентов записались (отношение :learn) на изучение курса Discrete Mathematics?
	```sql
	MATCH (c:course{name: 'Discrete Mathematics'}) <- [:learn]-(learn) RETURN learn.name
	```
2. Выведите список студентов, записавшихся на курс Databases.
	```sql
	MATCH (c:course{name: 'Databases'}) <- [:learn]-(learn) RETURN learn.name
	```
