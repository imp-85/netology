pantyukhin@netology:~$ sudo systemctl status mysql
[sudo] password for pantyukhin:
● mysql.service - MySQL Community Server
     Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2023-04-10 17:32:45 +05; 17min ago
    Process: 677 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=exited, status=0/SUCCESS)
   Main PID: 724 (mysqld)
     Status: "Server is operational"
      Tasks: 38 (limit: 4616)
     Memory: 430.9M
     CGroup: /system.slice/mysql.service
             └─724 /usr/sbin/mysqld

апр 10 17:32:44 netology systemd[1]: Starting MySQL Community Server...
апр 10 17:32:45 netology systemd[1]: Started MySQL Community Server.
pantyukhin@netology:~$ mysql
ERROR 1045 (28000): Access denied for user 'pantyukhin'@'localhost' (using password: NO)
pantyukhin@netology:~$ sudo mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.32-0ubuntu0.20.04.2 (Ubuntu)

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE USER 'sys_temp'
    ->
    ->
    -> 234
    -> 1234
1234
^C
mysql> SELECT User,Host FROM mysql.user
    -> ;
+------------------+-----------+
| User             | Host      |
+------------------+-----------+
| debian-sys-maint | localhost |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+
5 rows in set (0,00 sec)

mysql> CREATE USER 'sys_temp';
Query OK, 0 rows affected (0,06 sec)

mysql> SELECT User,Host FROM mysql.user
    -> ;
+------------------+-----------+
| User             | Host      |
+------------------+-----------+
| sys_temp         | %         |
| mysql.infoschema | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+
6 rows in set (0,00 sec)

mysql> DELETE USER 'sys_temp';
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''sys_temp'' at line 1
mysql> DROP User 'sys_temp';
Query OK, 0 rows affected (0,04 sec)

mysql> CREATE USER 'sys_temp'@'localhost' IDENTIFIED BY '12345678';
Query OK, 0 rows affected (0,01 sec)

mysql> SELECT User,Host FROM mysql.user;
+------------------+-----------+
| User             | Host      |
+------------------+-----------+
| debian-sys-maint | localhost |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
| sys_temp         | localhost |
+------------------+-----------+
6 rows in set (0,00 sec)

mysql> client_loop: send disconnect: Connection reset
PS C:\Users\IMP85> ssh pantyukhin@192.168.0.170
ssh: connect to host 192.168.0.170 port 22: Connection timed out
PS C:\Users\IMP85> ssh pantyukhin@192.168.0.170
pantyukhin@192.168.0.170's password: 
Welcome to Ubuntu 20.04.5 LTS (GNU/Linux 5.15.0-69-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

181 updates can be applied immediately.
139 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

New release '22.04.2 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

Your Hardware Enablement Stack (HWE) is supported until April 2025.
Last login: Mon Apr 10 17:35:10 2023 from 192.168.0.168
pantyukhin@netology:~$ mysql
ERROR 1045 (28000): Access denied for user 'pantyukhin'@'localhost' (using password: NO)
pantyukhin@netology:~$ sudo mysql
[sudo] password for pantyukhin: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.32-0ubuntu0.20.04.2 (Ubuntu)

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW GRANTS FOR 'sys_temp'@'localhost';
+----------------------------------------------+
| Grants for sys_temp@localhost                |
+----------------------------------------------+
| GRANT USAGE ON *.* TO `sys_temp`@`localhost` |
+----------------------------------------------+
1 row in set (0,00 sec)

mysql> GRANT ALL PRIVILEGES TO 'sys_temp'@'localhost';
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'TO 'sys_temp'@'localhost'' at line 1
mysql> GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'localhost';
Query OK, 0 rows affected (0,02 sec)

mysql> SHOW GRANTS FOR 'sys_temp'@'localhost';
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Grants for sys_temp@localhost





                   |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, SHUTDOWN, PROCESS, FILE, REFERENCES, INDEX, ALTER, SHOW DATABASES, SUPER, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER, CREATE TABLESPACE, CREATE ROLE, DROP ROLE ON *.* TO `sys_temp`@`localhost`


                   |
| GRANT APPLICATION_PASSWORD_ADMIN,AUDIT_ABORT_EXEMPT,AUDIT_ADMIN,AUTHENTICATION_POLICY_ADMIN,BACKUP_ADMIN,BINLOG_ADMIN,BINLOG_ENCRYPTION_ADMIN,CLONE_ADMIN,CONNECTION_ADMIN,ENCRYPTION_KEY_ADMIN,FIREWALL_EXEMPT,FLUSH_OPTIMIZER_COSTS,FLUSH_STATUS,FLUSH_TABLES,FLUSH_USER_RESOURCES,GROUP_REPLICATION_ADMIN,GROUP_REPLICATION_STREAM,INNODB_REDO_LOG_ARCHIVE,INNODB_REDO_LOG_ENABLE,PASSWORDLESS_USER_ADMIN,PERSIST_RO_VARIABLES_ADMIN,REPLICATION_APPLIER,REPLICATION_SLAVE_ADMIN,RESOURCE_GROUP_ADMIN,RESOURCE_GROUP_USER,ROLE_ADMIN,SENSITIVE_VARIABLES_OBSERVER,SERVICE_CONNECTION_ADMIN,SESSION_VARIABLES_ADMIN,SET_USER_ID,SHOW_ROUTINE,SYSTEM_USER,SYSTEM_VARIABLES_ADMIN,TABLE_ENCRYPTION_ADMIN,XA_RECOVER_ADMIN ON *.* TO `sys_temp`@`localhost` |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
2 rows in set (0,00 sec)

mysql> quit
Bye
pantyukhin@netology:~$ sudo mysql -u sys_temp -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 12
Server version: 8.0.32-0ubuntu0.20.04.2 (Ubuntu)

Copyright (c) 2000, 2023, Oracle and/or its affiliates.  

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
ERROR 1396 (HY000): Operation ALTER USER failed for 'sys_test'@'localhost'
mysql> ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY '12345678';
ERROR 1396 (HY000): Operation ALTER USER failed for 'sys_test'@'localhost'
mysql> ALTER USER 'sys_temp'@'localhost' IDENTIFIED WITH mysql_native_password BY '12345678';
Query OK, 0 rows affected (0,05 sec)

mysql> source /home/pantyukhin/sakila-schema.sql
ERROR:
Failed to open file '/home/pantyukhin/sakila-schema.sql', error: 2
mysql> source /home/pantyukhin/sakila-db/sakila-schema.sql
Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected, 1 warning (0,01 sec)

Query OK, 1 row affected (0,01 sec)

Database changed
Query OK, 0 rows affected (0,05 sec)

Query OK, 0 rows affected (0,09 sec)

Query OK, 0 rows affected (0,03 sec)

Query OK, 0 rows affected (0,08 sec)

Query OK, 0 rows affected (0,04 sec)

Query OK, 0 rows affected (0,04 sec)

Query OK, 0 rows affected (0,04 sec)

Query OK, 0 rows affected (0,04 sec)

Query OK, 0 rows affected (0,04 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,16 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,02 sec)

Query OK, 0 rows affected (0,04 sec)

Query OK, 0 rows affected (0,04 sec)

Query OK, 0 rows affected (0,04 sec)

Query OK, 0 rows affected (0,05 sec)

Query OK, 0 rows affected (0,05 sec)

Query OK, 0 rows affected (0,04 sec)

Query OK, 0 rows affected (0,02 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,01 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,00 sec)

Query OK, 0 rows affected (0,00 sec)

mysql> SHOW FULL TABLES;
+----------------------------+------------+
| Tables_in_sakila           | Table_type |
+----------------------------+------------+
| actor                      | BASE TABLE |
| actor_info                 | VIEW       |
| address                    | BASE TABLE |
| category                   | BASE TABLE |
| city                       | BASE TABLE |
| country                    | BASE TABLE |
| customer                   | BASE TABLE |
| customer_list              | VIEW       |
| film                       | BASE TABLE |
| film_actor                 | BASE TABLE |
| film_category              | BASE TABLE |
| film_list                  | VIEW       |
| film_text                  | BASE TABLE |
| inventory                  | BASE TABLE |
| language                   | BASE TABLE |
| nicer_but_slower_film_list | VIEW       |
| payment                    | BASE TABLE |
| rental                     | BASE TABLE |
| sales_by_film_category     | VIEW       |
| sales_by_store             | VIEW       |
| staff                      | BASE TABLE |
| staff_list                 | VIEW       |
| store                      | BASE TABLE |
+----------------------------+------------+
23 rows in set (0,00 sec)
