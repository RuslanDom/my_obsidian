# Unit tests
Тесты для каждой функции или метода
Один тест проверяет одну функцию за раз

Стандартный unit test

```python
import unittest  
  
from Tests_work.flask_tests.web_max_number import app  
 
  
  
class TestMaxNumber(unittest.TestCase):  
    """Механизм для инициализации определенных объектов для каждого теста"""  
    def setUp(self):  
        app.config['TESTING'] = True  
        app.config['DEBUG'] = False  
        self.app = app.test_client()  
        self.base_url = '/max/'  
  
  
    def test_can_get_correct_max_number_in_series_of_two(self):  
        number = 1, 3  
        # Создание url для теста в виде строки  
        url = self.base_url + ''.join(str(i) for i in number)  
        # Делаем запрос на endpoint  
        response = self.app.get(url)  
        # Так как мы возвращаем обычную строку flask конвертирует её в байты,  
        # поэтому не забываем сделать decode()        response_text = response.data.decode()  
        # Ожидаемый ответ  
        expected_correct_answer = f"{max(number)}"  
        # Проверка на истину  
        self.assertTrue(expected_correct_answer in response_text)
```

# Запуск теста с терминала
Необходимо изменить импорт:
```python
from web_max_number import app 
```


Директория:

	(.venv) ruslan@PC:~/Programming/Tests_work/flask_tests$
	
Код:

	 python -m unittest tests/test_max_number.py 
	 

# Integration test

# End-to-End tests
Тестирует полный цикл работы web-приложения от начала до конца