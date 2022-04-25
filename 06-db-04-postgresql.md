### Домашнее задание к занятию "6.4. PostgreSQL"

#### Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

`docker run --rm --name postgresql-docker -e POSTGRES_PASSWORD=netology -v my_data:/var/lib/postgresql/data -p 5432:5432 -d postgres:13`

![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/61676bf3c063715032400749c8f30bc3a5671a4e/1.jpg?raw=true)

Подключитесь к БД PostgreSQL используя `psql`.
![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/61676bf3c063715032400749c8f30bc3a5671a4e/2.jpg?raw=true)

Воспользуйтесь командой `\?` для вывода подсказки по имеющимся в `psql` управляющим командам.

**Найдите и приведите** управляющие команды для:
- вывода списка БД
![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/61676bf3c063715032400749c8f30bc3a5671a4e/3.jpg?raw=true)

- подключения к БД
`postgres-# \conninfo
You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".`
- вывода списка таблиц
![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/61676bf3c063715032400749c8f30bc3a5671a4e/4.jpg?raw=true)
- вывода описания содержимого таблиц
![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/61676bf3c063715032400749c8f30bc3a5671a4e/5.jpg?raw=true)
- выхода из psql
`postgres-# \q
root@71fa062f1696:/# `