Модуль ***subprocess*** отвечает за выполнение следующих действий: порождение новых процессов, соединение c потоками стандартного ввода, стандартного вывода, стандартного вывода сообщений об ошибках и получение кодов возврата от этих процессов

# Subprocess

## Run()

Пример
```python
import  subprocess  
  
  
def run_program():  
    res = subprocess.run(['python', 'test_program.py'])  
    print(res)  
  
if __name__ == "__main__":  
    run_program()
```

### capture_output
Если capture_output равно true, **будут захвачены stdout и stderr**. При использовании автоматически создается внутренний объект Popen с stdout=PIPE и stderr=PIPE . Аргументы stdout и stderr не могут быть заданы одновременно с capture_output
```python
subprocess.run(['python', 'test_program.py'], capture_output=True)
```
## Check_output()
Захватывает вывод команды и вызывает исключение, если команда не может быть выполнена

```python
subprocess.check_output(["ps", 'aux'])
```
# Shlex

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