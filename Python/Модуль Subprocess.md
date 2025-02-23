Модуль ***subprocess*** отвечает за выполнение следующих действий: порождение новых процессов, соединение c потоками стандартного ввода, стандартного вывода, стандартного вывода сообщений об ошибках и получение кодов возврата от этих процессов


# Subprocess

**Разница между методами run и Popen в модуле subprocess** заключается в следующем:

1. **Метод run** запускает указанную программу и ждёт окончание её работы. Параметры запускаемой программы указываются в виде элементов списка. Метод возвращает результат как объект CompletedProcess.

2. **Метод Popen** сразу после запуска указанного процесса возвращает управление, то есть запущенный процесс будет выполняться в фоновом режиме по отношению к родительскому процессу. Метод возвращает объект Popen, который имеет ряд компонентов, например метод wait() ждёт завершение процесса, а с помощью poll() можно осуществлять опрос.

Таким образом, **метод run используется для запуска программы и ожидания её завершения, а метод Popen позволяет продолжать работу до завершения процесса, а затем повторно вызывать метод Popen.communicate() для передачи и получения данных в процесс**. 

## Run()
## Создание процесса

Для создания нового процесса используйте функцию `subprocess.run()`. Вот пример запуска системной команды `ls`:

```python
import subprocess

result = subprocess.run(["ls"])
```

Запустить

Передайте команду и ее аргументы в виде списка строк. В этом примере мы передаем только одну строку, так как `ls` не требует дополнительных аргументов.

Пример:
```python
import  subprocess  
  
  
def run_program():  
    res = subprocess.run(['python', 'test_program.py'])  
    print(res)  
  
if __name__ == "__main__":  
    run_program()
```
## Получение вывода

Для того чтобы получить вывод команды, укажите параметр `capture_output=True`:

```python
import subprocess

result = subprocess.run(["ls"], capture_output=True)
print(result.stdout)
```
**result.stdout** будет содержать вывод команды в виде байтовой строки. Если вы хотите получить текстовую строку, используйте метод `decode()`:
```python
print(result.stdout.decode())
```
Если capture_output равно true, **будут захвачены stdout и stderr**. При использовании автоматически создается внутренний объект Popen с stdout=PIPE и stderr=PIPE . Аргументы stdout и stderr не могут быть заданы одновременно с capture_output
```python
subprocess.run(['python', 'test_program.py'], capture_output=True)
```

## Обработка ошибок

По умолчанию, если команда завершится с ошибкой, функция `subprocess.run()` возвратит объект с информацией об ошибке, но не вызовет исключение. Чтобы изменить это поведение, укажите параметр `check=True`:
```python
import subprocess

try:
	resylt = subprocess.run(["ls", "non_existent_directory"], capture_output=True, check=True)
except subprocess.CalledProcessError as e:
	print(f"An error ocurred: {e}")
```
Теперь, если команда завершится с ошибкой, будет вызвано исключение `subprocess.CalledProcessError`.

## Работа с входными данными

Чтобы передать входные данные в команду, используйте параметр `input`:
```python
import subprocess  
  
input_data = "Hello, world!"  
result = subprocess.run(  
    ["echo"], input=input_data.encode(), capture_output=True  
)  
print(result.stdout.decode())
```
**Не забудьте преобразовать входные данные в байтовую строку с помощью метода `encode()`.**

## Использование контекстных менеджеров

С помощью контекстных менеджеров можно более удобно работать с процессами, которые требуют длительного времени выполнения:
```python
import subprocess  
  
with subprocess.Popen(["sleep", "5"]) as proc:  
    try:  
        proc.wait(timeout=3)  
    except subprocess.TimeoutExpired:  
        proc.terminate()  
        proc.wait()
```
В этом примере мы запускаем команду `sleep`, которая выполняется в течение 5 секунд. Однако, мы ждем ее выполнения только 3 секунды и затем прерываем процесс с помощью метода `terminate()`.

Теперь вы знакомы с основами работы с модулем `subprocess` в Python. Используйте эти знания для создания мощных и гибких инструментов, которые могут взаимодействовать с другими процессами и системными командами. Удачи вам в изучении Python! 😉

## text
---

В **stdout** и **stderr** по умолчанию возвращается байтовые строки, которые надо декодировать. Иначе можно установить флаг **text**:

```python
res = subprocess.run('ls -la', shell=True, capture_output=True)
print(res.stdout)

res = subprocess.run('ls -la', shell=True, text=True, capture_output=True)
print(res.stdout)
```

## Check_output()
---
Захватывает вывод команды и вызывает исключение, если команда не может быть выполнена

```python
subprocess.check_output(["ps", 'aux'])
```






# Shlex модуль

## split()
Если команда длинная тогда можно использовать shlex.split для разбивки

```shell
com_line = """ /bin/vikings -input eggs.txt -output "spam spam.txt" -cmd "echo '$MONEY'" """
args = shlex.split(com_line)
print(args)
# ['/bin/vikings', '-input', 'eggs.txt' ...]
```

## quote()

```python
import os  
import shlex, subprocess  
from typing import List  
from flask import Flask, request  
  
app = Flask(__name__)  
  
  
@app.route("/ps", methods=["GET"])  
def ps() -> str:  
	# Получает список слов от пользователя и записывает их в список
    args: List[str] = request.args.getlist("arg")  
	# Защищая от потенциальных уязвимостей инъекции оболочки возвращает экранированную версию строки s
	
    safe_command = shlex.quote(''.join(args))  
    # check_output() захватывает вывод команды и вызывает исключение, если команда не может быть выполнена  
    result = str(subprocess.check_output(["ps", safe_command]))    
    return "<br>".join(result.split(r'\n'))  
  
if __name__ == "__main__":  
    app.run(debug=True)
```

# Popen