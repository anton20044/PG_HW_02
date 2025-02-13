| **<br/>Лабораторная работа №2 по курсу "PostgreSQL для администраторов баз данных и разработчиков"<br/>"Установка и настройка PostgteSQL в контейнере Docker"<br/>**|
|---|

<br/>

## Задание:
### Установить и настроить PostgteSQL в контейнере Docker

<br/>

## Решение:

* Установил docker и docker compose на ОС Ubuntu 22.04
* Запустил docker образ itnami/postgresql:16
* Зашел в контейнер, запустил команду id, получил uid пользователя под которым работает СУБД в контейнере, он 1001
* Остановил и удалил контейнер
* Создал пользователя в хостовой ОС postgres с uid 1001
* В хостовой ОС создал каталог /var/lib/postgres, выдал права на каталог (770) пользователю postgres, созданному на предыдущем шаге (иначе контейнер не смог бы запуститься, т.к. отсутствовали бы права на запись в каталог)
* Создал файл для docker compose (postgres.yml)
* Запустил создание контейнера 
```bash
docker compose -f postgres.yml up -d 
```
![docker](image/dz02_docker_ps.jpg)

* Создал переменную PSQL_CONNECTION_STRING в хостовой ОС
```bash
export PSQL_CONNECTION_STRING=postgres://postgres:postgres@192.168.50.126:5432/dz02?sslmode=disable
```
* Запустил контейнер с psql, передал ему переменную PSQL_CONNECTION_STRING
```bash
docker run -ti --rm alpine/psql $PSQL_CONNECTION_STRING
```
* Создал в БД таблицу из ДЗ №1 и вставил данные, сделал выборку
![docker](image/dz02_insert_data.jpg)

* Подключился к контейнеру с рабочей станции, данные доступны
![docker](image/dz02_select_on_workstation.jpg)

* Удалил контейнер, создал его заново и подключился. Сделал выборку данных, данные доступны
![docker](image/dz02_recreate_container.jpg)