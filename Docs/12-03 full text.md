mysql> SELECT DISTINCT district from address WHERE district LIKE 'k%a' AND district  NOT LIKE '% %';
+-----------+
| district  |
+-----------+
| Kanagawa  |
| Kalmykia  |
| Kaduna    |
| Karnataka |
| Kütahya   |
| Kerala    |
| Kitaa     |
+-----------+
7 rows in set (0,00 sec)

mysql> SELECT payment_id, payment_date, amount FROM payment 
    -> WHERE payment_date BETWEEN '2005-06-15 00:00:00' AND '2005-06-18 23:59:59' AND amount > 10.00
    -> ORDER BY payment_date DESC;
+------------+---------------------+--------+
| payment_id | payment_date        | amount |
+------------+---------------------+--------+
|      12888 | 2005-06-18 08:33:23 |  10.99 |
|       8272 | 2005-06-17 23:51:21 |  11.99 |
|       7017 | 2005-06-17 18:09:04 |  10.99 |
|      15313 | 2005-06-17 04:05:12 |  10.99 |
|      13892 | 2005-06-16 14:52:02 |  10.99 |
|      14620 | 2005-06-15 18:30:46 |  10.99 |
|        908 | 2005-06-15 09:46:33 |  10.99 |
+------------+---------------------+--------+
7 rows in set (0,02 sec)

mysql> SELECT rental_id, rental_date _date
    -> from rental
    -> order by rental_date DESC
    -> limit 5;
+-----------+---------------------+
| rental_id | _date               |
+-----------+---------------------+
|     11739 | 2006-02-14 15:16:03 |
|     14616 | 2006-02-14 15:16:03 |
|     11676 | 2006-02-14 15:16:03 |
|     15966 | 2006-02-14 15:16:03 |
|     13486 | 2006-02-14 15:16:03 |
+-----------+---------------------+
5 rows in set (0,00 sec)

mysql> SELECT LOWER(first_name) , REPLACE(first_name , 'LL','pp')
customer    -> from customer
    -> WHERE first_name = 'Kelly' OR first_name = 'Willie';
+-------------------+---------------------------------+
| LOWER(first_name) | REPLACE(first_name , 'LL','pp') |
+-------------------+---------------------------------+
| kelly             | KEppY                           |
| willie            | WIppIE                          |
| willie            | WIppIE                          |
| kelly             | KEppY                           |
+-------------------+---------------------------------+
4 rows in set (0,00 sec)

mysql>
