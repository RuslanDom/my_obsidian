# Подключение

# Создание ключей

```shell
ruslan@PC:~$ ssh-keygen -t rsa -b 2048 -C "TimeWeb Machine" -f timeweb
```
pass: Ruslan

### **Установить права доступа к загруженному файлу приватного ключа**
Для этого в терминале выполните следующую команду, заменив <путь_к_ключу>  
на путь к файлу приватного ключа:

```shell
chmod 600 /home/Documents/TimeWeb_keys/timeweb_key
```
Команда ограничивает доступ к ключу, чтобы только вы могли его использовать, — это требуется для безопасности.

### **Подключитесь к серверу через SSH. (root)**
Введите следующую команду, заменив <путь_к_ключу>, <имя_пользователя> и <публичный_IP> на соответствующие данные:

ssh -i <путь_к_ключу> -p 2022 <имя_пользователя>@<публичный_IP>
```shell
ssh -i ./Documents/TimeWeb_Keys/timeweb_key -p 2022 root@185.119.59.245
```

#### **Подключитесь к серверу через SSH.**
1. Необходимо сгенерировать пару ключей для входа с использованием ключей
```shell
ssh-keygen -t rsa -b 4096 -C Ruslan_key
```
2. Проверяем публичный ключ и копируем его
```shell
cat .ssh/id_rsa.pub
```
1. **СОЗДАНИЕ ПОЛЬЗОВАТЕЛЯ** и выдача прав новому пользователю

```shell
ssh -i ./Documents/TimeWeb_Keys/timeweb_key -p 2022 root@185.119.59.245
...выполняется вход
root@4211349-qf86747:~# adduser admin
...вводится пароль и повтор
root@4211349-qf86747:~# usermod -aG sudo admin
```
2. Переключение на пользователя admin на сервере su (swap user)
```shell
root@4211349-qf86747:~# su - user
...переключение с root на admin
admin@4211349-qf86747:~$ 
```
3. Создаём пару ключей на сервере
```shell
admin@4211349-qf86747:~$ ssh-keygen
```
4. Создаём authorized_keys и помещаем туда необходимы ключ
```shell
admin@4211349-qf86747:~$ cat >> .ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC8l2m8m+5wC7We/pGZf8tly+AqhtpUNttT9e3wDjT1bWJRcWMbS91hFdKsxaiw/3v+9PG/fHVLZCBfP6yHjaUng2C495Ww85pjQiqsfXq9jqOgnsGfFJEkEYgab5yQd3JYMHO6WGwGAQDnpP+2rkeW13kYtoH3iDiDAmLCvBRWnv2cbP37nPr9j6/3dHBKD3F73LO9MJ8AeChl0FygYeI5gzHVZthLgG6k+DJF7mY571ZnjPIyrC+ob22nuH2xPVj8bsgm9yyghb/FnbjdojjfeF8F0qo/B3nkXbGakifT4ulIPz/Lxdau0Y+JwisxMgfD/ntglwtlEpbKs7oYqBaYCBPGCq6tHGn/4W9s4VdOvImVGlk9rZXuUMHUK5H61yVmpJ0n2rcQlPmCmyM3TzmMPmTnB6ewbclGw8rbh0o1kA6/+tYVTlWEaJtcV/L9SAUUHkQp+5bva9r6CznQckzAwEWbC6OH07ETN9NMwoXRXAOceUDZV6cX1i/W816TtLk8jlnF0QP/tI4JdIfcd2Pp+icOFHfj0Izt8/S9sLWD9nMl6t2KHCAbSdCNY28ePm7CnboLmpPwiEbFbN2D2fn6d03wp5MLZduFtmbUole2eLgd5Z0EYNeiT2udeCWYXaDMJIUnN2RMYkWaXj6lEBFekvCTJlOxwuCrtqKvHprYuw== Ruslan_key
```

Теперь вход под пользователем admin происходит под ключом
```shell
ssh -p 2022 admin@185.119.59.245
```

# Настройка python проекта

## Установка python и виртуальной среды
5. Заходим под админа на сервер pass: cop
```shell
ssh -p 2022 admin@185.119.59.245
```
6. Проверка версии python на сервере 
```shell
admin@4211349-qf86747:~$ python3 -V
```
7. Установка python3-virtualenv
```shell
admin@4211349-qf86747:~$ sudo apt install python3-virtualenv
```
8. Установка рабочей папки
```shell
admin@4211349-qf86747:~$ mkdir workspace
admin@4211349-qf86747:~$ cd workspace/
```
9. Создаём виртуальное окружение
```shell
admin@4211349-qf86747:~/workspace$ python3 -m virtualenv -p python3 venv
```
10. Активируем виртуальное окружение
```shell
admin@4211349-qf86747:~/workspace$ source venv/bin/activate
```

### Консольный текстовый редактор vim

11. Установим консольный текстовый редактор vim
```shell
sudo apt install vim
```
12. Создание конфигурационного файла для vim (в основной директории admin)
```shell
admin@4211349-qf86747:~$ cat << EOF > .ssh/.vimrc
> set number
> set cursorline
> set encoding=UTF-8
> EOF
```
Проверим:
```shell
admin@4211349-qf86747:~$ cat .ssh/.vimrc 
set number
set cursorline
set encoding=UTF-8
```
Команды для vim:
	сохранить - :w
	закрыть файл - :q
	выйти из insert - CTRL + C
## Установка Flask на сервер

```shell
admin@4211349-qf86747:~/workspace$ pip install flask
```
### Создадим для примера простой flask service
13. Создаём пустой файл
```shell
admin@4211349-qf86747:~/workspace$ touch app.py
```
14. Откроем в vim редакторе
```shell
admin@4211349-qf86747:~/workspace$ vim app.py
```
**В редакторе vim**:
	сохранить - :w
	закрыть файл - :q
	выйти из insert - CTRL + C
15. Старт приложения flask
```shell
admin@4211349-qf86747:~/workspace$ flask run -h 185.119.59.245
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://185.119.59.245:5000
Press CTRL+C to quit
``` 