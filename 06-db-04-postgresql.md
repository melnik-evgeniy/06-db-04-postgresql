### Домашнее задание к занятию "6.4. PostgreSQL"

#### Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

`docker run --rm --name postgresql-docker -e POSTGRES_PASSWORD=netology -v my_data:/var/lib/postgresql/data -p 5432:5432 -d postgres:13`

![](https://github.com/melnik-evgeniy/06-db-04-postgresql/blob/61676bf3c063715032400749c8f30bc3a5671a4e/1.jpg?raw=true)

Подключитесь к БД PostgreSQL используя psql.