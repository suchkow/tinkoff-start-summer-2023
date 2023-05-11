# 11 задание
---
![](https://edu.tinkoff.ru/files/612e3976-51b4-4aa8-825e-d0d92867aa17)

1. Напишите запрос, с помощью которого можно найти дубли в поле email из таблицы Staff.

```sql
WITH dublicates AS (
	SELECT staff_id, email, COUNT(*)
	FROM staff
	GROUP BY staff_id, email
	HAVING count(*) > 1
)

SELECT staff.*
FROM staff
JOIN dublicates ON staff.staff_id = dublicates.staff_id
AND staff.email = dub.email
ORDER BY staff.email;
```

2. Напишите запрос, с помощью которого можно определить возраст каждого сотрудника из таблицы Staff на момент запроса.

```sql
SELECT name, (DATE_PART('year', CURRENT_DATE) - DATE_PART('year', birthday)) -
			 (RIGHT(CURRENT_DATE, 5 < RIGHT(birthday, 5)) AS age
FROM staff
ORDER BY name;
```

3. Напишите запрос, с помощью которого можно определить должность (jobtitles.name) со вторым по величине уровнем зарплаты.

```sql
WITH second_max_salary AS (
	SELECT staff_id, MAX(salary)
	FROM staff
	WHERE salary < (SELECT MAX(salary) FROM staff)
)

SELECT name
FROM jobtitles
WHERE jobtitle_id = second_max_salary.staff_id;
```