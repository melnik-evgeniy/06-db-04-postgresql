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