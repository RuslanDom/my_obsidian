# Multiprocessing and Threading

Пример работы multiprocessing и threading

```python
from multiprocessing import Process  
from threading import Thread  
import time  
  
  
def task_1(num: int):  
    res = 0  
    for i in range(1000000):  
        res += i ** num  
    # print(f'Задача выполнена: {res}')  
  
def proc(i):  
    p1 = Process(target=task_1, args=(i,))  
    p2 = Process(target=task_1, args=(i,))  
    p3 = Process(target=task_1, args=(i,))  
    start_time = time.time()  
    p1.start()  
    p2.start()  
    p3.start()  
    p1.join()  
    p2.join()  
    p3.join()  
    res_time = time.time() - start_time  
    print(f"Время затраченное с использованием процессов: {res_time}")  
  
def thread(i):  
    t1 = Thread(target=task_1, args=(i,))  
    t2 = Thread(target=task_1, args=(i,))  
    t3 = Thread(target=task_1, args=(i,))  
    start_time = time.time()  
    t1.start()  
    t2.start()  
    t3.start()  
    t1.join()  
    t2.join()  
    t3.join()  
    res_time = time.time() - start_time  
    print(f"Время затраченное с использованием потоков: {res_time}")  
  
if __name__ == "__main__":  
    proc(5)  
    thread(5)
```

# Цикл событий

Основной default - й цикл событий asyncio.run(func())

## Создание своего цикла событий

```python
import asyncio

async def main():
	await asyncio.sleep(3)
	
loop = asyncio.new_event_loop()
try:	
	loop.run_until_complete(main())
finally;
	loop.close()

```

## Получение доступа к циклу событий

```python
import asyncio

def my_call_later():
	print("Меня вызовут позже")

async def main():
	loop = asyncio.get_running_loop()
	loop.call_soon(my_call_later)  # Вызовёт на 2-й итерации
	await delay(3)  # Вызовёт на 1-й итерации

asyncio.run(main())
```

# Создание задач

Для создания задач используется команда asyncio.create_task()

```python
async def _main():  
    sleep_for_three = asyncio.create_task(delay(3)) 
    sleep_for_two =  asyncio.create_task(delay(2))
    print("Этот код выполнился раньше")  
    task1 = await sleep_for_three  
    task2 = await sleep_for_two
    print(task1)
    print(task2)  
  
asyncio.run(_main())
```





# Вспомогательные функции

## delay()

```python
import asyncio  
async def delay(delay_seconds:int):  
    print(f"Засыпаю на {delay_seconds} с")  
    await asyncio.sleep(delay_seconds)  
    print(f"Сон в течении {delay_seconds} с")  
    return delay_seconds

```