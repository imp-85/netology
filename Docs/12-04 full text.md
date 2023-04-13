mysql> SELECT s.staff_id AS Id, s.first_name NAME, s.last_name AS LAST_NAME, c.city_id AS ID, c.city AS CITY, 
    -> COUNT(c2.customer_id) AS Number_of_Buyers
    -> FROM staff s
    -> JOIN address a ON a.address_id = s.address_id
    -> JOIN city c ON c.city_id = a.city_id
    -> JOIN store s2 ON s2.store_id = s.store_id
    -> JOIN customer c2 ON c2.store_id = s2.store_id 
    -> GROUP BY s.first_name , s.last_name , c.city , s.staff_id , c.city_id 
    -> HAVING Number_of_Buyers > 300;
+----+------+-----------+-----+------------+------------------+
| Id | NAME | LAST_NAME | ID  | CITY       | Number_of_Buyers |
+----+------+-----------+-----+------------+------------------+
|  1 | Mike | Hillyer   | 300 | Lethbridge |              326 |
+----+------+-----------+-----+------------+------------------+
1 row in set (0,00 sec)

mysql> SELECT COUNT(film_id) 
    -> FROM film
    -> WHERE `length` > (SELECT AVG(`length`)  FROM film);
+----------------+
| COUNT(film_id) |
+----------------+
|            489 |
+----------------+
1 row in set (0,01 sec)

mysql> SELECT YEAR (payment_date) , MONTH (payment_date), COUNT(rental_id) , SUM(amount)
    -> FROM payment p
    -> GROUP BY YEAR (payment_date), MONTH (payment_date)
    -> ORDER BY SUM(amount) DESC 
    -> LIMIT 1;
+---------------------+----------------------+------------------+-------------+
| YEAR (payment_date) | MONTH (payment_date) | COUNT(rental_id) | SUM(amount) |
+---------------------+----------------------+------------------+-------------+
|                2005 |                    7 |             6709 |    28368.91 |
+---------------------+----------------------+------------------+-------------+
1 row in set (0,03 sec)

mysql> 
