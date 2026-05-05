# Домашнее задание к занятию «SQL. Часть 2»

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```
SELECT 
    CONCAT(s.first_name, ' ', s.last_name) AS employee_name,
    c.city AS store_city,
    COUNT(cust.customer_id) AS total_customers
FROM store st
JOIN staff s ON st.store_id = s.store_id
JOIN address a ON st.address_id = a.address_id
JOIN city c ON a.city_id = c.city_id
JOIN customer cust ON st.store_id = cust.store_id
GROUP BY st.store_id, s.first_name, s.last_name, c.city
HAVING COUNT(cust.customer_id) > 300;
```


### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```
SELECT COUNT(*) AS films_longer_than_avg
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```
SELECT 
    DATE_FORMAT(p.payment_date, '%Y-%m-01') AS payment_month,
    SUM(p.amount) AS total_amount,
    COUNT(DISTINCT p.rental_id) AS total_rentals
FROM payment p
GROUP BY DATE_FORMAT(p.payment_date, '%Y-%m-01')
ORDER BY total_amount DESC
LIMIT 1;
```
