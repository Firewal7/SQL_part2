# Домашнее задание к занятию 12.4 "Реляционные базы данных: SQL. Часть 2"
# Софьин Роман

# Задача 1

```sql
SELECT DISTINCT s.first_name, s.last_name, c.city, (select count(store_id)
from customer c2
where c2.store_id = (
	select store_id
	from customer c2
	having count(store_id) > 300 
	)) as count_of_customers
FROM staff s
JOIN store s2 ON s2.manager_staff_id = s.staff_id
JOIN customer c2 ON c2.store_id = s2.store_id
JOIN address a ON a.address_id = s2.address_id
JOIN city c ON c.city_id = a.city_id
WHERE s.staff_id = (
SELECT s2.manager_staff_id
FROM store s2
WHERE s2.store_id = (
select c2.store_id
from customer c2
having count(store_id) > 300)
) AND c2.store_id = (select store_id
from customer c2
having count(store_id) > 300) AND c.city_id = (SELECT city_id
FROM address a
WHERE a.address_id = (
SELECT s2.address_id
FROM store s2
WHERE s2.store_id = (
select c2.store_id
from customer c2
having count(store_id) > 300)));
```

![задача 1](https://raw.githubusercontent.com/Firewal7/SQL2/main/2.1.png)


# Задача 2

```sql
SELECT COUNT(length)
FROM film
WHERE length > (SELECT AVG(length) from film);
```

![задача 2](https://raw.githubusercontent.com/Firewal7/SQL2/main/2.2.png)

# Задача 3

```sql
SELECT month, (SELECT COUNT(rental_date) count
FROM rental
WHERE MONTH(rental_date) = (SELECT month
FROM (SELECT MONTH(payment_date) month, SUM(amount) sum
FROM payment
GROUP BY MONTH(payment_date)) AS Query
WHERE sum = (SELECT MAX(sum)
FROM (SELECT MONTH(payment_date) month, SUM(amount) sum
FROM payment
GROUP BY MONTH(payment_date)) AS Query2))) as count_of_rental
FROM (SELECT MONTH(payment_date) month, SUM(amount) sum
FROM payment
GROUP BY MONTH(payment_date)) AS Query
WHERE sum = (SELECT MAX(sum)
FROM (SELECT MONTH(payment_date) month, SUM(amount) sum
FROM payment
GROUP BY MONTH(payment_date)) AS Query2);
```

![задача 3](https://raw.githubusercontent.com/Firewal7/SQL2/main/2.3.png)