# Документация по Flask

https://docs-python.ru/packages/veb-frejmvork-flask-python/

# LINUX
# Добавляем переменную в виртуальное окружение

	export FLASK_APP="app.py"

Выставим режим DEBUG для того чтобы изменения сразу отображались на сервере

	export FLASK_DEBUG=1

или 

```python
if __name__ == "__main__":
	app.run(debug=True)
```

# Запуск сервера из cmd
Запускать следует из рабочей директории!

![[Pasted image 20241028212022.png]]