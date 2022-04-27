### Домашнее задание к занятию "6.4. PostgreSQL"

#### Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

```bash
docker run --rm --name postgresql-docker -e POSTGRES_PASSWORD=netology -v my_data:/var/lib/postgresql/data -p 5432:5432 -d postgres:13
```

![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/61676bf3c063715032400749c8f30bc3a5671a4e/1.jpg?raw=true)

Подключитесь к БД PostgreSQL используя `psql`.
![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/0d47e6bc441e348a77f30f58af88f8412fe81f1d/2.jpg?raw=true)

Воспользуйтесь командой `\?` для вывода подсказки по имеющимся в `psql` управляющим командам.

**Найдите и приведите** управляющие команды для:
- вывода списка БД
![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/0d47e6bc441e348a77f30f58af88f8412fe81f1d/3.jpg?raw=true)

- подключения к БД
```bash
postgres-# \conninfo
You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".
```
- вывода списка таблиц
![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/0d47e6bc441e348a77f30f58af88f8412fe81f1d/4.jpg?raw=true)
- вывода описания содержимого таблиц
![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/0d47e6bc441e348a77f30f58af88f8412fe81f1d/5.jpg?raw=true)
- выхода из psql
```bash
postgres-# \q
root@71fa062f1696:/# 
```

#### Задача 2

Используя `psql` создайте БД `test_database`.

![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/071426a658d0ae07150a78caf0db7c4ecd233de8/6.jpg?raw=true)
Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-04-postgresql/test_data).

Восстановите бэкап БД в `test_database`.
![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/93e0c9f378c25cd826ee25450ca5fa8ac5e87e29/7.jpg?raw=true)
Перейдите в управляющую консоль `psql` внутри контейнера.
```bash
root@71fa062f1696:/# psql -U postgres
psql (13.6 (Debian 13.6-1.pgdg110+1))
Type "help" for help.

postgres=# 
```
Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.
![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/da20e85959bc2407a06039c1146efca9c0579aa0/8.jpg?raw=true)
Используя таблицу [pg_stats](https://postgrespro.ru/docs/postgresql/12/view-pg-stats), найдите столбец таблицы `orders` 
с наибольшим средним значением размера элементов в байтах.

```bash
test_database=# SELECT avg_width FROM pg_stats WHERE tablename='orders';
 avg_width
-----------
         4
        16
         4
(3 rows)
```

#### Задача 3

Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и
поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили
провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.
```pgsql
test_database=# CREATE TABLE orders_more_499_price (CHECK (price > 499)) INHERITS (orders);
CREATE TABLE
test_database=# INSERT INTO orders_more_499_price SELECT * FROM orders WHERE price > 499;
INSERT 0 3
test_database=# CREATE TABLE orders_less_499_price (CHECK (price <= 499)) INHERITS (orders);
CREATE TABLE
test_database=# INSERT INTO orders_LESS_499_price SELECT * FROM orders WHERE price <= 499;
INSERT 0 5
test_database=# DELETE FROM ONLY orders;
DELETE 8
test_database=# 
test_database=# \dt
                 List of relations
 Schema |         Name          | Type  |  Owner   
--------+-----------------------+-------+----------
 public | orders                | table | postgres
 public | orders_less_499_price | table | postgres
 public | orders_more_499_price | table | postgres
(3 rows)
```
Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?
Да, прописав правила, например:
```pgsql
CREATE RULE orders_insert_to_more AS ON INSERT TO orders WHERE ( price > 499 ) DO INSTEAD INSERT INTO orders_more_499_price VALUES (NEW.*);
CREATE RULE orders_insert_to_less AS ON INSERT TO orders WHERE ( price <= 499 ) DO INSTEAD INSERT INTO orders_less_499_price VALUES (NEW.*);
```
#### Задача 4

Используя утилиту `pg_dump` создайте бекап БД `test_database`.

```bash
test_database=# export PGPASSWORD=netology && pg_dump -h localhost -U postgres test_database > /tmp/test_database_backup.sql
```
Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?

Использовать UNIQUE
```bash
title character varying(80) NOT NULL UNIQUE,
```