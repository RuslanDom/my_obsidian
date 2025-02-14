# OS

## Документация по модулю OS

https://docs-python.ru/standart-library/modul-os-python/





# SYS

## Документация по модулю sys

https://docs-python.ru/standart-library/modul-sys-python/



### Ввод в терминал с клавиатуры

	sys.stdin()

### Сразу прочитать и записать в переменную 

	data = sys.stdin.read()

## Перенаправление вывода print в файл Python: альтернативы sys.stdout

### contextlib
Если вам нужно **быстро перенаправить вывод функции `print`** в файл, воспользуйтесь возможностями Python, включая **менеджер контекста** и функцию `redirect_stdout` из модуля `contextlib`. Этот способ считается "pythonic" и предпочтительнее, чем прямое взаимодействие с `sys.stdout`. Для перенаправления можете использовать следующий код:

```python
from contextlib import redirect_stdout

# Вывод 'print' отправляется в 'output.txt'
with open('output.txt', 'w') as f, redirect_stdout(f):
    print('Привет, файл, встречай вывод!')
```

### STDOUT в консоль и в файл

```python
print("Привет, консоль!")  # Почтальон доставляет ваше сообщение в консоль 

with open('output.txt', 'w') as file:   # Меняем адрес доставки 📁✉️
    print("Здравствуй, файл!", file=file) # Теперь почта приходит в файл, а не в консоль!
```

С проверкой существования пути до файла

```python
import os

filename = os.path.join('my_directory', 'output.txt')
dirname = os.path.dirname(filename)

if not os.path.exists(dirname):
    os.makedirs(dirname)

with open(filename, 'w') as f:
    print('Приветствую пространство!', file=f)
```

### С модулем logging

```python
import logging

logging.basicConfig(filename='output.log', level=logging.INFO)
logger = logging.getLogger()

logger.info('Это сообщение отобразится в output.log через 5...4...3...')
```

### Перенаправление через cmd stdout в файл

```shell
python my_script.py > output.txt
```

### Мой Контекстный менеджер для перенаправления stdout, stderr в указанные файлы

```python
from types import TracebackType  
from typing import Type, Literal, IO  
import sys  
import traceback  
  
  
class Redirect:  
    def __init__(self, stdout: IO = None, stderr: IO = None) -> None:  
        self.old_stdout = sys.stdout  
        self.old_stderr = sys.stderr  
        self.stdout = stdout  
        self.stderr = stderr  
  
    def __enter__(self):  
        if self.stdout:  
            sys.stdout = self.stdout  
        if self.stderr:  
            sys.stderr = self.stderr  
  
    def __exit__(  
            self,  
            exc_type: Type[BaseException] | None,  
            exc_val: BaseException | None,  
            exc_tb: TracebackType | None  
    ) -> Literal[True] | None:  
        if self.stdout:  
            self.stdout.close()  
            sys.stdout = self.old_stdout  
        if self.stderr:  
            # print(exc_type, exc_val, exc_tb, file=self.stderr)  # Или так  
            sys.stderr.write(traceback.format_exc())  
            self.stderr.close()  
            sys.stderr = self.old_stderr  
        return True
```