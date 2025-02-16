# OS

## Документация по модулю OS

https://docs-python.ru/standart-library/modul-os-python/

#### _`os.environ`_:

Переменная [`os.environ`](https://docs-python.ru/standart-library/modul-os-python/upravlenie-sredoj-okruzhenija-koda/ "Управление переменной средой окружения системы в Python.") это объект сопоставления, подобный [словарю](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-dict-slovar/ "Словарь dict в Python."), который представляет переменные среды окружения системы. Например, `os.environ['HOME']` это путь к домашнему каталогу на некоторых платформах и эквивалентен `getenv("HOME")` в языке `C`.

Переменная `os.environ` фиксируется при первом импорте [модуля `os`](https://docs-python.ru/standart-library/modul-os-python/ "Модуль os в Python, доступ к функциям ОС."), во время запуска Python. Изменения в среде OS, сделанные после этого времени, не отражаются `os.environ`, за исключением изменений, внесенных путем непосредственного изменения `os.environ`.

Если платформа поддерживает функцию [`os.putenv()`](https://docs-python.ru/standart-library/modul-os-python/upravlenie-sredoj-okruzhenija-koda/#os.putenv), то сопоставление `os.environ` может использовать ее для изменения среды. Функция `os.putenv()` будет вызываться автоматически при изменении `os.environ`.

Если платформа не поддерживает функцию `os.putenv()`, то измененная копия сопоставления `os.environ` может быть передана соответствующим функциям создания процессов, чтобы заставить дочерние процессы использовать измененную среду.

В Unix ключи и значения используют [`sys.getfilesystemencoding()`](https://docs-python.ru/standart-library/modul-sys-python/kodirovka-ispolzuemaja/ "Кодировка, используемая Python.") и [обработчик ошибок `'surrogateescape'`](https://docs-python.ru/standart-library/modul-codecs-python/obrabotchiki-oshibok-kodirovki/ "Обработчики ошибок кодировки."). Если вы хотите использовать другую кодировку, то используйте [`os.environb`](https://docs-python.ru/standart-library/modul-os-python/upravlenie-sredoj-okruzhenija-koda/#os.environb).

**Заметка**:

- Вызов `os.putenv()`, напрямую не изменят `os.environ`, поэтому лучше менять значения ключей [`os.environ`](https://docs-python.ru/standart-library/modul-os-python/upravlenie-sredoj-okruzhenija-koda/#os.environ).
- На некоторых платформах, включая FreeBSD и Mac OS X, настройка `os.environ` может вызвать утечки памяти.

Если платформа поддерживает функцию [`os.unsetenv()`](https://docs-python.ru/standart-library/modul-os-python/upravlenie-sredoj-okruzhenija-koda/#os.unsetenv), то с ее помощью можно удалить элементы в этом отображении `os.environ` для сброса указанной переменной среды. Функция `os.unsetenv()` будет вызываться автоматически при удалении элемента из `os.environ` и при вызове одного из методов [.pop()](https://docs-python.ru/tutorial/operatsii-slovarjami-dict-python/metod-dict-pop/ "Метод dict.pop() в Python, примеры кода") или [.clear()](https://docs-python.ru/tutorial/operatsii-slovarjami-dict-python/metod-dict-clear/ "Метод dict.clear() в Python. Очистить словарь.").

```python
>>> import os
>>> os.environ['LANG']
# 'ru_RU.UTF-8'
>>> os.environ['HOME']
# '/home/docs-python'
>>> os.environ['HOME'] = '/tmp'
>>> os.environ.pop('HOME')
# '/tmp'
>>> os.environ.get('HOME', '/var/www')
# '/var/www'

# очистка среды окружения
os.environ.clear()
os.environ
# environ({})
```




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