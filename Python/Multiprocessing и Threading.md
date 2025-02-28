# Multiprocessing
#### Ссылки
https://docs.python.org/3.4/library/multiprocessing.html

## Создание простого процесса Process-класс
```python
from multiprocessing import Process
import time


def func(sec):
	time.sleep(sec)


if __name__ == "__main__":
	proc_1 = Process(target=func, args=(3, ))
	proc_2 = Process(target=func, args=(3, ))
	proc_1.start()
	proc_2.start()
	proc_1.join()
	proc_2.join()
```

Чтобы показать отдельные задействованные идентификаторы процессов, приведем развернутый пример:
```python
from multiprocessing import Process
import os

def info(title):
	print(title)
	print('Module name: {}'.format(__name__))
	print("Parent PID: {}".format(os.getppid()))
	print('Process PID: {}'.format(os.getpid()))


def func(name):
	info(name)
	print('Info about {}'.format(name))

if __name__ == '__main__':
	info('main_line')
	proc = Process(target=func, args=('buddy',))
	proc.start()
	proc.join()


# main_line
# Module name: __main__
# Parent PID: 3423
# Process PID: 8142

# buddy
# Module name: __main__
# Parent PID: 8142
# Process PID: 8173
# Info about buddy
```

## POOL процессов
```python
from multiprocessing import Pool

def f(x):
    return x*x

if __name__ == '__main__':
    with Pool(5) as p:
        print(p.map(f, [1, 2, 3]))
```



















# Threading























