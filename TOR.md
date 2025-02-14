# STEP 1

## Установка TOR

Начнем с начала . По очереди вводим в терминале команды для обновления пакетов и установки пакета tor:

```shell
sudo apt-get update
sudo apt-get install tor
```

Чтобы использовать мосты, необходимо установить следующий пакет:

```shell
sudo apt install obfs4proxy
```

Находим и запускаем в телеграмм [бота](https://t.me/GetBridgesBot) и запрашиваем у него мосты. После чего открываем файл конфигурации tor:

```shell
sudo xed /etc/tor/torrc
```

После того, как откроется конфигурация, туда нужно дописать следующие строки в конец файла:

```
ClientTransportPlugin obfs4 exec /usr/bin/obfs4proxy
Bridge obfs4 51.89.225.173:15573 0D1F29997A237A368D37667D04DD6E62BF35764E
Bridge obfs4 51.89.175.235:61312 DB9B67A2404FB1B01F125EBB72823DFCEEFD0205 
UseBridges 1 
```

Здесь следует обратить внимание на то, что для указания моста сначала пишем ключевое слово: Bridge, и только потом уже мост. Сохраняем конфигурацию и перезапускаем tor.

```shell
sudo /etc/init.d/tor restart
```

Проверить, работает ли сервис можно с помощью команды:

```shell
/etc/init.d/tor status
```

В ответе вы получите что-то вроде этого. Ну или должны получить.

```shell
● tor.service - Anonymizing overlay network for TCP (multi-instance-master)
     Loaded: loaded (/usr/lib/systemd/system/tor.service; enabled; preset: enabled)
     Active: active (exited) since Sun 2024-11-03 14:03:05 MSK; 16s ago
    Process: 20287 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 20287 (code=exited, status=0/SUCCESS)
        CPU: 2ms

ноя 03 14:03:05 PC systemd[1]: Starting tor.service - Anonymizing overla…)...
ноя 03 14:03:05 PC systemd[1]: Finished tor.service - Anonymizing overla…er).
Hint: Some lines were ellipsized, use -l to show in full.
```

## Запуск TOR

Для остановки и запуска tor необходимо использовать следующие команды:

```shell
sudo /etc/init.d/tor start
sudo /etc/init.d/tor stop
```

Еще один нюанс заключается в том, что если вы не хотите, чтобы tor стартовал вместе с системой, следует выполнить команду:

```shell
sudo systemctl disable tor.service
```

## Proxychain

## Редактор config

	sudo nano /etc/proxychains.conf

## Запуск proxychains

	proxychains <name>

	proxychains postman or firefox