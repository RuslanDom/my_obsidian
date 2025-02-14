# Установка

Обновляем пакеты
```shell
sudo apt update
```

Устанавливаем Postgresql
```shell
sudo apt install postgresql posgresql-contrib
```

Проверяем статус
```shell
service postgresql status
```

Получить команды можно через эту команду
```shell
service postgresql
```

```
Usage: /etc/init.d/postgresql {start|stop|restart|reload|force-  reload|status} [version ..]
```

## Вход в учётную запись

```shell
sudo -i -u postgres 
```

Получить команды можно через `man psql`
```shell
postgres@PC:~$ man psql
```

Открываем консоль Postgresql введя psql
```shell
postgres@PC:~$ psql
psql (16.4 (Ubuntu 16.4-0ubuntu0.24.04.2))
Type "help" for help.
```

## Базовые команды 

- Список баз данных `\l`
```shell
postgres=# \l
```

- Выход из консоли `\q`

- Создать свою БД `createdb`
```shell
postgres@PC:~$ createdb <name>
or
sudo -u postgres psql -c 'CREATE DATABASE <name>;'
```

- Удалить БД `dropdb <name>`
```shell
postgres@PC:~$ dropdb <name>
```

- Изменить пароль
```shell
postgres=# ALTER USER postgres WITH PASSWORD 'password';
```

- Посмотреть список пользователей `\du`
```
                             List of roles
 Role name |                         Attributes                         
-----------+------------------------------------------------------------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS

```

- Создание нового пользователя
```shell
postgres=# CREATE USER <username> WITH PASSWORD 'password';
```

```shell
CREATE ROLE
postgres=# \du
                             List of roles
 Role name |                         Attributes                         
-----------+------------------------------------------------------------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS
 userrus   | 

```

- Дать права суперпользователя
```shell
postgres=# ALTER USER <username> WITH SUPERUSER;
```

- Удаление пользователя
```shell
postgres=# DROP USER <username>;
```

- Выход  из учётной записи `exit` либо комбинацию `ctrl+D`
```shell
postgres@PC:~$ exit
```

# УДАЛЕНИЕ

```shell
sudo apt-get --purge remove pgadmin3  
sudo apt-get --purge remove postgresql\*  
  
sudo rm -r /etc/postgresql/  
sudo rm -r /etc/postgresql-common/  
sudo rm -r /var/lib/postgresql/  
sudo userdel -r postgres  
sudo groupdel postgres
```