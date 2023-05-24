# SYS-21_6-04
Docker. Часть 2
## Задание 1
Напишите ответ в свободной форме, не больше одного абзаца текста.

Установите Docker Compose и опишите, для чего он нужен и как может улучшить вашу жизнь.
### Ответ:
![Screenshot at 2023-05-22 07-11-00](https://github.com/Roman-Teterevlev/SYS-21_6-04/assets/132853752/42427001-9973-4488-893d-928c070e115d)
Docker Compose — это инструмент для описания многоконтейнерных приложений. С его помощью можно собрать один файл, в котором описываются все контейнеры. Еще Docker Compose позволяет собирать, останавливать и запускать файлы одной командой.
## Задание 2
Выполните действия и приложите текст конфига на этом этапе.

Создайте файл docker-compose.yml и внесите туда первичные настройки:
- version;
- services;
- networks.

При выполнении задания используйте подсеть 172.22.0.0. Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw.
### Ответ:
![Screenshot at 2023-05-22 22-34-20](https://github.com/Roman-Teterevlev/SYS-21_6-04/assets/132853752/6276d6f3-73c2-4f6f-803c-948f3af59e01)
## Задание 3
Выполните действия и приложите текст конфига текущего сервиса:
1. Установите PostgreSQL с именем контейнера <ваши фамилия и инициалы>-netology-db.
2. Предсоздайте БД <ваши фамилия и инициалы>-db.
3. Задайте пароль пользователя postgres, как <ваши фамилия и инициалы>12!3!!
4. Пример названия контейнера: ivanovii-netology-db.
5. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

## Задание 4
Выполните действия:
1. Установите pgAdmin с именем контейнера <ваши фамилия и инициалы>-pgadmin.
2. Задайте логин администратора pgAdmin <ваши фамилия и инициалы>@ilove-netology.com и пароль на выбор.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.
4. Прокиньте на 80 порт контейнера порт 61231.

В качестве решения приложите:
- текст конфига текущего сервиса;
- скриншот админки pgAdmin.
### Ответ 3 и 4:
![Screenshot at 2023-05-25 01-00-04](https://github.com/Roman-Teterevlev/SYS-21_6-04/assets/132853752/930afe20-1a51-4e4f-b8b8-60d733e74464)
![Screenshot at 2023-05-25 01-01-44](https://github.com/Roman-Teterevlev/SYS-21_6-04/assets/132853752/60ff21c2-90fe-45e9-8114-91fff222f607)
## Задание 5
Выполните действия и приложите текст конфига текущего сервиса:
1. Установите Zabbix Server с именем контейнера <ваши фамилия и инициалы>-zabbix-netology.
2. Настройте его подключение к вашему СУБД.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

## Задание 6
Выполните действия и приложите текст конфига текущего сервиса:
1. Установите Zabbix Frontend с именем контейнера <ваши фамилия и инициалы>-netology-zabbix-frontend.
2. Настройте его подключение к вашему СУБД.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.
### Ответ 5, 6:
![Screenshot at 2023-05-25 01-04-20](https://github.com/Roman-Teterevlev/SYS-21_6-04/assets/132853752/a4ec7a0b-382e-401a-abd9-dad22896ece7)
## Задание 7
Выполните действия.
1. Настройте линки, чтобы контейнеры запускались только в момент, когда запущены контейнеры, от которых они зависят.
2. В качестве решения приложите:
- текст конфига целиком;
- скриншот команды docker ps;
- скриншот авторизации в админке Zabbix.
### Ответ:
```
version: "3"
services:
  PostgreSQL:
    image: postgres:latest
    container_name: teterevlevrv-netology-db
    ports:
      - 5432:5432
    volumes:
      - ./pg_data:/var/lib/postgresql/data/pgdata
    environment:
      POSTGRES_PASSWORD: teterevlevrv12!3!!
      POSTGRES_DB: teterevlevrv-db
      PGDATA: /var/lib/postgresql/data/pgdata
    networks:
      teterevlevrv-my-netology-hw:
        ipv4_address: 172.22.0.2
    restart: always

  pgadmin:
    image: dpage/pgadmin4
    container_name: teterevlevrv-pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: teterevlevrv@ilove-netology.com
      PGADMIN_DEFAULT_PASSWORD: teterevlevrv12!3!!
    ports:
      - 61231:80
    networks:
      teterevlevrv-my-netology-hw:
        ipv4_address: 172.22.0.3
    restart: always

  zabbix-server:
    image: zabbix/zabbix-server-pgsql
    links:
      - PostgreSQL
      - pgadmin
    container_name: teterevlevrv-zabbix-netology
    environment:
      DB_SERVER_HOST: 172.22.0.2
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: teterevlevrv12!3!!
    ports:
      - 10051:10051
    networks:
      teterevlevrv-my-netology-hw:
        ipv4_address: 172.22.0.4
    restart: always

  zabbix_wgui:
    image: zabbix/zabbix-web-apache-pgsql
    links:
      - PostgreSQL
      - pgadmin
      - zabbix-server
    container_name: teterevlevrv-netology-zabbix-frontend
    environment:
      DB_SERVER_HOST: 172.22.0.2
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: teterevlevrv12!3!!
      ZBX_SERVER_HOST: zabbix_wgui
      PHP_TZ: "Europe/Moscow"
    ports:
      - 80:8080
      - 443:8443
    networks:
      teterevlevrv-my-netology-hw:
        ipv4_address: 172.22.0.5
    restart: always

networks:
  teterevlevrv-my-netology-hw:
    driver: bridge
    ipam:
      config:
      - subnet: 172.22.0.0/24
```
![Screenshot at 2023-05-25 01-17-22](https://github.com/Roman-Teterevlev/SYS-21_6-04/assets/132853752/11f053ac-7783-4ac2-a2cb-75683968021d)
![Screenshot at 2023-05-25 01-19-26](https://github.com/Roman-Teterevlev/SYS-21_6-04/assets/132853752/e6d06c6c-66d6-4784-81fa-d43a4c0c170d)
## Задание 8
Выполните действия:
1. Убейте все контейнеры и потом удалите их.
2. Приложите скриншот консоли с проделанными действиями.
### Ответ:
![Screenshot at 2023-05-25 01-31-28](https://github.com/Roman-Teterevlev/SYS-21_6-04/assets/132853752/d450ce8b-7d40-4fad-be51-8e57de47e1e2)
