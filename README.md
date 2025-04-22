# My training SQL queries
## SQL_academy_trainer
https://sql-academy.org/ru/trainer

`1. Вывести имена всех людей, которые есть в базе данных авиакомпаний.`
```
SELECT name
FROM Passenger
```

`2. Вывести названия всеx авиакомпаний.`
```
SELECT name
FROM Company
```

`3. Вывести все рейсы, совершенные из Москвы.`
```
SELECT *
FROM Trip
WHERE town_from = "Moscow"
```

`4. Вывести имена людей, которые заканчиваются на "man".`
```
SELECT name
FROM Passenger
WHERE RIGHT(name, 3) = "man"
```

`5. Вывести количество рейсов, совершенных на TU-134.`
```
SELECT COUNT(plane) count
FROM Trip
GROUP BY plane
HAVING plane = "TU-134"
```
   
`6. Какие компании совершали перелеты на Boeing.`
```
SELECT DISTINCT name
FROM Trip
JOIN Company ON Company.id = Trip.company
WHERE Trip.plane = "Boeing"
```

`7. Вывести все названия самолётов, на которых можно улететь в Москву (Moscow).`
```
SELECT DISTINCT plane
FROM Trip
WHERE town_to = "Moscow"
```

`8. В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?`
```
SELECT DISTINCT town_to,
TIMEDIFF(time_in, time_out) as flight_time
FROM Trip
WHERE town_from = "Paris"
```

`9. Какие компании организуют перелеты из Владивостока (Vladivostok)?`
```
SELECT DISTINCT name
FROM Trip
JOIN Company ON Trip.company = Company.id
WHERE town_from = "Vladivostok"
```

`10. Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.`
```
SELECT *
FROM Trip
WHERE time_out BETWEEN 19000101100000 AND 19000101140000
```
    
`11. Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.`
```
SELECT name FROM Passenger
WHERE LENGTH(name) = (
  SELECT MAX(LENGTH(name)) FROM Passenger)
```

`12. Выведите идентификаторы всех рейсов и количество пассажиров на них. Обратите внимание, что на каких-то рейсах пассажиров может не быть. В этом случае выведите число "0".`
```
SELECT t.id, COUNT(pit.passenger) as count
FROM Trip t
	LEFT JOIN Pass_in_trip pit ON pit.trip = t.id
GROUP BY t.id
```

`13. Вывести имена людей, у которых есть полный тёзка среди пассажиров.`
```
SELECT DISTINCT  name FROM Passenger
WHERE name IN (SELECT name FROM Passenger p 
WHERE Passenger.id != p.id)
```

`14. В какие города летал Bruce Willis.`
```
SELECT town_to FROM Trip
 JOIN Pass_in_trip ON Pass_in_trip.trip = Trip.id
WHERE Pass_in_trip.passenger = ANY(
	SELECT id FROM Passenger
   WHERE name = 'Bruce Willis')
```

`15. Выведите идентификатор пассажира Стив Мартин (Steve Martin) и дату и время его прилёта в Лондон (London).`
```
SELECT DISTINCT Pass_in_trip.passenger id, time_in FROM Trip
  JOIN Pass_in_trip ON Pass_in_trip.trip = Trip.id
WHERE Pass_in_trip.passenger = ANY(
    SELECT DISTINCT id FROM Passenger
      WHERE name = 'Steve Martin' AND town_to = 'London')
```

`16. Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.`
```
SELECT name, COUNT(Pass_in_trip.trip) count FROM Passenger
JOIN Pass_in_trip ON Pass_in_trip.passenger = Passenger.id
GROUP BY name
ORDER BY count DESC, name
```

`18. Выведите имя самого старшего человека. Если таких несколько, то выведите их всех.`
```
SELECT member_name
FROM FamilyMembers
WHERE birthday = (SELECT MIN(birthday) FROM FamilyMembers)
```

`19. Определить, кто из членов семьи покупал картошку (potato).`
```
SELECT DISTINCT status FROM FamilyMembers
  JOIN Payments ON Payments.family_member = FamilyMembers.member_id
  JOIN Goods ON Goods.good_id = Payments.good
WHERE Goods.good_name = 'potato'
```
    
`20. Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму.`
```
SELECT status, member_name, SUM(unit_price * amount) costs
FROM FamilyMembers
  JOIN Payments ON Payments.family_member = FamilyMembers.member_id
  JOIN Goods ON Goods.good_id = Payments.good
  JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type
WHERE good_type_name = 'entertainment'
GROUP BY status, member_name
```

`22. Найти имена всех матерей (mother).`
```
SELECT member_name FROM FamilyMembers
WHERE status = 'mother'
```

    
