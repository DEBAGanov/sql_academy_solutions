# sql_academy_solutions
Решение задач по SQL. Тренажер [sql-academy.org](https://sql-academy.org/ru/trainer)

Нажмите ★, если вам нравится проект. Ваш вклад сердечно ♡ приветствуется.

Если вам интересно мое резюме: https://github.com/DEBAGanov








`Задание 1: Вывести имена всех когда-либо обслуживаемых пассажиров авиакомпаний`

```sql
SELECT name from Passenger;
```

`Задание 2: Вывести названия всеx авиакомпаний`
```sql
SELECT name FROM Company;
```

`Задание 3: Вывести все рейсы, совершенные из Москвы`
```sql
SELECT * FROM Trip
WHERE town_from = 'Moscow';
```

`Задание 4: Вывести имена людей, которые заканчиваются на "man"`

```sql
SELECT name FROM Passenger
WHERE name LIKE '%man';
```

`Задание 5: Вывести количество рейсов, совершенных на TU-134`
```sql
SELECT DISTINCT COUNT('plane') AS count FROM Trip
WHERE plane LIKE 'TU-134';
```

`Задание 6: Какие компании совершали перелеты на Boeing`
```sql
SELECT Company.name FROM Trip
LEFT JOIN Company
ON Company.id = Trip.company
WHERE plane = 'Boeing'
GROUP BY company;
```


`Задание 7: Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)`
```sql
SELECT plane FROM Trip
WHERE town_to = 'Moscow'
GROUP BY plane;
```


Задание 8: В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?
```sql
SELECT town_to, TIMEDIFF(time_in, time_out) AS flight_time
FROM Trip
WHERE town_from = 'Paris';
```


`Задание 9: Какие компании организуют перелеты из Владивостока (Vladivostok)?`
```sql
SELECT name FROM Company AS c
LEFT JOIN Trip AS t
ON c.id = t.company
WHERE t.town_from = 'Vladivostok';
```

`Задание 10: Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.`
```sql
SELECT * FROM Trip
WHERE time_out BETWEEN '1900-01-01T10:00:00.000Z' AND '1900-01-01T14:00:00.000Z';
```


`Задание 11: Вывести пассажиров с самым длинным именем`
```sql
SELECT name FROM Passenger
ORDER BY LENGTH(name) DESC LIMIT 1;
```


`Задание 12: Вывести id и количество пассажиров для всех прошедших полётов`
```sql
SELECT trip, COUNT(passenger) AS count
FROM Pass_in_trip
GROUP BY trip;
```


`Задание 13: Вывести имена людей, у которых есть полный тёзка среди пассажиров`
```sql
SELECT name
FROM Passenger
GROUP BY name
HAVING COUNT(*) > 1;
```


`Задание 14: В какие города летал Bruce Willis`
```sql
SELECT t.town_to
FROM Trip AS t
JOIN Pass_in_trip AS pit
ON t.id = trip
JOIN Passenger AS p
ON p.id = passenger
WHERE name = 'Bruce Willis';
```


`Задание 15: Во сколько Стив Мартин (Steve Martin) прилетел в Лондон (London)`
```sql
SELECT t.time_in
FROM Trip AS t
JOIN Pass_in_trip AS pit
ON t.id = trip
JOIN Passenger AS p
ON p.id = passenger
WHERE name = 'Steve Martin' AND town_to = 'London';
```


`Задание 16: Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.`
```sql
SELECT p.name, COUNT(passenger) AS count FROM Trip AS t
JOIN Pass_in_trip AS pit
ON t.id = trip
JOIN Passenger AS p
ON p.id = passenger
GROUP BY p.name
HAVING count >= 1
ORDER BY count DESC, p.name ASC;
```

`Задание 17: Определить, сколько потратил в 2005 году каждый из членов семьи`
```sql
SELECT member_name, status, SUM(unit_price * amount) as costs FROM Payments AS p
JOIN FamilyMembers AS fm
ON p.family_member = fm.member_id
WHERE date LIKE '2005%'
GROUP BY family_member;
```

`Задание 18: Узнать, кто старше всех в семьe`
```sql
SELECT member_name FROM FamilyMembers
WHERE birthday = (SELECT MIN(birthday) FROM FamilyMembers);
```


`Задание 19: Определить, кто из членов семьи покупал картошку (potato)`
```sql
SELECT status FROM FamilyMembers AS fm
JOIN Payments AS p
ON fm.member_id = p.family_member
JOIN Goods AS g
ON p.good = g.good_id
WHERE good_name LIKE 'potato' GROUP BY status;
```


`Задание 20: Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму`
```sql
SELECT status, member_name, SUM(unit_price*amount) AS costs FROM FamilyMembers AS fm
JOIN Payments AS p
ON fm.member_id = p.family_member
JOIN Goods AS g
ON p.good = g.good_id
JOIN GoodTypes as gp
ON g.type = gp.good_type_id
WHERE good_type_name = 'entertainment'
GROUP BY family_member;
```


`Задание 21: Определить товары, которые покупали более 1 раза`
```sql
SELECT good_name FROM Payments AS p
JOIN Goods as g
ON p.good = g.good_id
GROUP BY good
HAVING COUNT(good_name) > 1;
```


`Задание 22: Найти имена всех матерей (mother)`
```sql
SELECT member_name FROM FamilyMembers
WHERE status = 'mother';
```


`Задание 23: Найдите самый дорогой деликатес (delicacies) и выведите его цену`
```sql
SELECT good_name, unit_price FROM Payments AS p
JOIN Goods AS g
ON p.good = g.good_id
JOIN GoodTypes as gp
ON g.type = gp.good_type_id
WHERE good_type_name = 'delicacies'
LIMIT 1;
```


`Задание 24: Определить кто и сколько потратил в июне 2005`
```sql
SELECT member_name, SUM(unit_price*amount) as costs FROM Payments as p
JOIN FamilyMembers as fm
ON p.family_member = fm.member_id
WHERE date LIKE '2005-06%'
GROUP BY member_name;
```


`Задание 25: Определить, какие товары имеются в таблице Goods, но не покупались в течение 2005 года`
```sql
SELECT good_name FROM Goods
LEFT JOIN Payments ON
 Goods.good_id = Payments.good
 AND YEAR(Payments.date) = 2005
WHERE  Payments.good IS NULL
GROUP BY good_id;
```


ЕЩЕ ОДНО РЕШЕНИЕ:
```sql
SELECT good_name, good_id, good, date FROM Goods as g
LEFT OUTER JOIN Payments as p
ON g.good_id = p.good
WHERE date IS NULL OR date NOT LIKE '2005%'
ORDER BY good;
```


`Задание 26: Определить группы товаров, которые не приобретались в 2005 году ГРУППЫ, ТОВАРЫ, КОГДА ПРИОБРЕТАЛИСЬ:`

```sql
SELECT good_type_name, good_name, good_id, good, payment_id, date FROM Goods
JOIN Payments ON  Goods.good_id = Payments.good
JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type;
```sql

РЕШЕНИЕ:
```sql
SELECT good_type_name FROM GoodTypes
WHERE good_type_id NOT IN (SELECT good_type_id FROM Goods
JOIN Payments ON  Goods.good_id = Payments.good AND YEAR(date) = 2005
JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type);
```

`Задание 27: Узнать, сколько потрачено на каждую из групп товаров в 2005 году. Вывести название группы и сумму`
```sql
SELECT good_type_name, SUM(amount*unit_price) AS costs FROM GoodTypes
JOIN Goods ON good_type_id = type
JOIN Payments ON good = good_id AND YEAR(date) = 2005
GROUP BY good_type_name;
```


`Задание 28: Сколько рейсов совершили авиакомпании с Ростова (Rostov) в Москву (Moscow) ?`
```sql
SELECT COUNT(id) AS count FROM Trip
WHERE town_from = 'Rostov' AND town_to = 'Moscow';
```


`Задание 29: Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134`
```sql
SELECT DISTINCT name FROM Passenger
JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
JOIN Trip ON Pass_in_trip.trip = Trip.id
WHERE plane = 'TU-134' AND town_to = 'Moscow';
```


`Задание 30: Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности.`
```sql
SELECT trip, COUNT(passenger) AS count FROM Passenger
JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
JOIN Trip ON Pass_in_trip.trip = Trip.id
GROUP BY trip ORDER BY count DESC;
```


`Задание 31: Вывести всех членов семьи с фамилией Quincey.`
```sql
SELECT * FROM FamilyMembers
WHERE member_name LIKE '%Quincey';
```


`Задание 32: Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.`
```sql
SELECT FLOOR(AVG(FLOOR(DATEDIFF(NOW(), birthday)/365))) AS age
FROM FamilyMembers;
```


`Задание 33: Найдите среднюю стоимость икры. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar).`
```sql
SELECT AVG(unit_price) AS cost FROM Payments
JOIN Goods ON good=good_id
WHERE good_name = 'red caviar' OR good_name = 'black caviar';
```


`Задание 34: Сколько всего 10-ых классов?`
```sql
SELECT COUNT(name) AS count
FROM Class WHERE name LIKE '10%';
```

`Задание 35: Сколько различных кабинетов школы использовались 2.09.2019 в образовательных целях ?`
```sql
SELECT DISTINCT COUNT(classroom) AS count FROM Schedule
WHERE date LIKE '2019-09-02%';
```


`Задание 36: Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?`
```sql
SELECT * FROM Student
WHERE address LIKE '%Pushkina%';
```


`Задание 37: Сколько лет самому молодому обучающемуся ?`
```sql
SELECT ROUND(MIN(DATEDIFF(NOW(), birthday)/365)) AS year FROM Student;
SELECT FLOOR(MIN(DATEDIFF(NOW(), birthday)/365)) AS year FROM Student;
```

`Задание 38: Сколько Анн (Anna) учится в школе ?`
```sql
SELECT COUNT(1) As count
FROM  Student
WHERE first_name LIKE 'Anna';
```


`Задание 39: Сколько обучающихся в 10 B классе ?`
1)
```sql
SELECT COUNT(class) AS count FROM Student_in_class
JOIN Class ON Class.id=class WHERE name LIKE '10 B';
```
2)
```sql
SELECT COUNT(class) AS count FROM Student_in_class
JOIN Class ON Class.id=class AND name = '10 B';
```

`Задание 40: Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.) ?`
```sql
SELECT DISTINCT(Subject.name) AS subjects FROM Subject
JOIN Schedule ON Subject.id=Schedule.subject
JOIN Teacher ON Teacher.id=Schedule.teacher AND last_name='Romashkin';
```


`Задание 41: Во сколько начинается 4-ый учебный предмет по расписанию ?`
```sql
SELECT start_pair FROM Timepair WHERE id = 4;
SELECT start_pair FROM Timepair LIMIT 3, 1;
SELECT start_pair FROM Timepair LIMIT 1 OFFSET 3;
```


`Задание 42: Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет ?`
```sql
SELECT DISTINCT TIMEDIFF((SELECT end_pair FROM Timepair WHERE id = 4),
(SELECT start_pair FROM Timepair WHERE id = 2)) as time FROM Timepair;
```


`Задание 43: Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Остортируйте преподавателей по фамилии.`
```sql
SELECT last_name FROM Teacher
JOIN Schedule ON Teacher.id=Schedule.teacher
JOIN Subject ON Subject.id=Schedule.subject
WHERE Subject.name='Physical Culture' ORDER BY last_name ASC;
```


`Задание 44: Найдите максимальный возраст (колич. лет) среди обучающихся 10 классов ?`
```sql
SELECT FLOOR(MAX((DATEDIFF(NOW(), birthday)/365))) AS max_year FROM Student
JOIN Student_in_class ON Student.id=Student_in_class.student
JOIN Class ON Class.id=Student_in_class.class WHERE Class.name LIKE '10%';
```

`Задание 45: Какой(ие) кабинет(ы) пользуются самым большим спросом?`
```sql
SELECT classroom, COUNT(classroom) as count  FROM Schedule
GROUP BY classroom
HAVING COUNT(*) > 4
ORDER BY COUNT(*) DESC; - какие кабинеты в топе?
```



`Задание 46: В каких классах введет занятия преподаватель "Krauze" ?`
```sql
SELECT DISTINCT name FROM Class
JOIN Schedule ON Class.id=Schedule.class
JOIN Teacher ON Teacher.id=Schedule.teacher
WHERE last_name = 'Krauze';
```



`Задание 47: Сколько занятий провел Krauze 30 августа 2019 г.?`
```sql
SELECT COUNT(teacher) AS count FROM Schedule
JOIN Teacher ON Teacher.id=Schedule.teacher AND last_name = 'Krauze'
WHERE date LIKE '2019-08-30%';
```


`Задание 48: Выведите заполненность классов в порядке убывания`
```sql
SELECT name, COUNT(class) as count FROM Class
JOIN Student_in_class ON Class.id=Student_in_class.class
GROUP BY name ORDER BY COUNT(*) DESC;
```


`Задание 49: Какой процент обучающихся учится в 10 A классе ?`
```sql
SELECT (COUNT(*)*100/(SELECT COUNT(Student.id) as count FROM Student
JOIN Student_in_class ON Student.id=Student_in_class.student)) AS percent
FROM Student_in_class
JOIN Class ON Class.id=Student_in_class.class AND name = '10 A';
```


`Задание 50: Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону.`
```sql
SELECT FLOOR((COUNT(*)*100/(SELECT COUNT(Student.id) as count
FROM Student
JOIN Student_in_class ON Student.id=Student_in_class.student))) AS percent FROM Student
WHERE YEAR(birthday) = 2000;
```


`Задание 51: Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods).`

```sql
INSERT INTO Goods(good_id, good_name, type) VALUES (17, 'Cheese', 2);
```


`Задание 52: Добавьте в список типов товаров (GoodTypes) новый тип "auto".`
```sql
INSERT INTO GoodTypes(good_type_id, good_type_name) VALUES (9, 'auto');
```


`Задание 53: Измените имя "Andie Quincey" на новое "Andie Anthony".`
```sql
UPDATE FamilyMembers SET member_name='Andie Anthony' WHERE member_id=3;
```

`Задание 54: Удалить всех членов семьи с фамилией "Quincey".`
```sql
DELETE FROM FamilyMembers WHERE member_name LIKE '%Quincey';
```


`Задание 55: Удалить компании, совершившие наименьшее количество рейсов.`

```sql
SELECT name, COUNT(company) as company FROM Trip
JOIN Company ON Company.id=Trip.company GROUP BY name;
DELETE FROM Company WHERE id = 4;
DELETE FROM Company WHERE id = 3;
DELETE FROM Company WHERE id = 2;
```

`Задание 56: Удалить все перелеты, совершенные из Москвы (Moscow).`
```sql
DELETE FROM Trip WHERE town_from LIKE '%Moscow';
```

`Задание 57: Перенести расписание всех занятий на 30 мин. вперед.`
```sql
UPDATE Timepair SET start_pair = DATE_ADD(start_pair, INTERVAL 30 MINUTE);
UPDATE Timepair SET end_pair = DATE_ADD(end_pair, INTERVAL 30 MINUTE);
```

`Задание 58: Добавить отзыв с рейтингом 5 на жилье, находящиеся по адресу "11218, Friel Place, New York", от имени "George Clooney"`
```sql
SELECT Users.name, Reservations.* FROM Reservations
JOIN Rooms ON Rooms.id=Reservations.room_id
JOIN Users ON Users.id=Reservations.user_id
WHERE address = '11218, Friel Place, New York'

INSERT INTO Reviews (id, reservation_id, rating) VALUES (23, 2, 5);
```

`Задание 59: Вывести пользователей,указавших Белорусский номер телефона ? Телефонный код Белоруссии +375.`
```sql
SELECT * FROM Users WHERE phone_number LIKE '+375%';
```


`Задание 60: Выведите идентификаторы преподавателей, которые хотя бы один раз за всё время преподавали в каждом из одиннадцатых классов.`
```sql
SELECT teacher FROM Schedule
JOIN Teacher ON Teacher.id=Schedule.teacher
JOIN Subject ON Subject.id=Schedule.subject
JOIN Class ON Class.id=Schedule.class
WHERE Class.name IN ('11 A', '11 B')
GROUP BY teacher HAVING COUNT(teacher)>=1
ORDER BY teacher;
```


`Задание 61: Выведите список комнат, которые были зарезервированы в течение 12 недели 2020 года.`
```sql
SELECT Rooms.*  FROM Rooms
JOIN Reservations ON Rooms.id=Reservations.room_id AND YEAR(start_date)=2020 AND YEAR(end_date)=2020
WHERE WEEK(start_date, 1)=12 OR WEEK(end_date, 1)=12;
```


`Задание 62: Вывести в порядке убывания популярности доменные имена 2-го уровня, используемые пользователями для электронной почты. Полученный результат необходимо дополнительно отсортировать по возрастанию названий доменных имён.`
```sql
SELECT SUBSTRING_INDEX(email, '@', -1) as domain, count(*) AS count FROM Users
GROUP BY domain
ORDER BY count DESC, domain ASC;
```

`Задание 63: Выведите отсортированный список (по возрастанию) имен студентов в виде Фамилия.И.О.`
```sql
SELECT CONCAT(last_name, '.', LEFT(first_name, 1), '.', LEFT(middle_name, 1), '.') AS name FROM Student ORDER BY first_name ASC;
```

`Задание 64: Выведите имена всех пар пассажиров, летевших вместе на одном рейсе два или более раз, и количество таких совместных рейсов. В passengerName1 разместите имя пассажира с наименьшим идентификатором.`




`Задание 65: Необходимо вывести рейтинг для комнат, которые хоть раз арендовали, как среднее значение рейтинга отзывов округленное до целого вниз.`
```sql
SELECT room_id, FLOOR(AVG(rating)) AS rating FROM Reservations
JOIN Reviews ON Reviews.reservation_id=Reservations.id
GROUP BY room_id;
```


`Задание 66: Вывести список комнат со всеми удобствами (наличие ТВ, интернета, кухни и кондиционера), а также общее количество дней и сумму за все дни аренды каждой из таких комнат.`
```sql
SELECT home_type, address, COALESCE(SUM(DATEDIFF(end_date, start_date)), 0) as days, COALESCE(SUM(Reservations.total), 0) AS total_fee FROM Reservations
RIGHT JOIN Rooms ON Rooms.id=Reservations.room_id
WHERE has_tv !=0 AND has_internet !=0 AND has_kitchen !=0 AND has_air_con !=0
GROUP BY address, home_type;
```

