# Установка 

1. Установите публичный ключ репозитория

```shell
sudo curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add
```

2. Создайте файл конфигурации репозитория

```shell
sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/focal pgadmin4 main" \ > /etc/apt/sources.list.d/pgadmin4.list && apt update'
```

1. Обновите пакеты

```shell
sudo apt update
```

2. Установите пакет pgadmin4

```shell
sudo apt install pgadmin4
```

## Если требует python 3.8

3. Для установки Python 3.8 могут потребоваться дополнительные пакеты. Выполните команду:
```shell
sudo apt install software-properties-common
```

4. Добавьте репозиторий с Python 3.8

```shell
sudo add-apt-repository ppa:deadsnakes/ppa
```

5. Обновите пакеты

```shell
sudo apt update
```

6. Установите python 3.8 и повторите пункт 4

```shell
sudo apt install python3.8
```