Схема БД состоит из четырех таблиц:  
- Product(maker, model, type)  
- PC(code, model, speed, ram, hd, cd, price)  
- Laptop(code, model, speed, ram, hd, price, screen)  
- Printer(code, model, color, type, price)  
- 
Таблица Product представляет производителя (maker), номер модели (model) и тип ('PC' - ПК, 'Laptop' - ПК-блокнот или 'Printer' - принтер). Предполагается, что номера моделей в таблице Product уникальны для всех производителей и типов продуктов. 

В таблице PC для каждого ПК, однозначно определяемого уникальным кодом – code, указаны модель – model (внешний ключ к таблице Product), скорость - speed (процессора в мегагерцах), объем памяти - ram (в мегабайтах), размер диска - hd (в гигабайтах), скорость считывающего устройства - cd (например, '4x') и цена - price (в долларах). 

Таблица Laptop аналогична таблице РС за исключением того, что вместо скорости CD содержит размер экрана -screen (в дюймах). 
В таблице Printer для каждой модели принтера указывается, является ли он цветным - color ('y', если цветной), тип принтера - type (лазерный – 'Laser', струйный – 'Jet' или матричный – 'Matrix') и цена - price.

1. Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd
```sql
SELECT model, speed, hd FROM PC WHERE price < 500
```

|model|speed|hd|
|---|---|---|
|1232|500|10.0|
|1232|450|8.0|
|1232|450|10.0|
|1260|500|10.0|

2. Найдите производителей принтеров. Вывести: maker
```sql
SELECT DISTINCT maker FROM Product WHERE type = 'Printer'
```

|maker|
|---|
|A|
|D|
|E|
3. Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.
```sql
SELECT model, ram, screen FROM Laptop WHERE price > 1000

```

| model | ram | screen |
| ----- | --- | ------ |
| 1750  | 128 | 14     |
| 1298  | 64  | 15     |
| 1752  | 128 | 14     |
4. Найдите все записи таблицы Printer для цветных принтеров.
```sql
SELECT * FROM Printer WHERE color = 'y'
```

| code | model | color | type | price    |
| ---- | ----- | ----- | ---- | -------- |
| 3    | 1434  | y     | Jet  | 290.0000 |
| 2    | 1433  | y     | Jet  | 270.0000 |
5. Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.
```sql
SELECT model, speed, hd 
FROM PC WHERE price < 600 AND (cd = '12x' OR cd = '24x')
```

| model | speed | hd   |
| ----- | ----- | ---- |
| 1232  | 500   | 10.0 |
| 1232  | 450   | 8.0  |
| 1232  | 450   | 10.0 |
| 1260  | 500   | 10.0 |
6. Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.
```sql
SELECT DISTINCT maker, speed 
FROM Product JOIN Laptop ON Product.model = Laptop.model WHERE hd >= 10
```

| maker | speed |
| ----- | ----- |
| A     | 450   |
| A     | 600   |
| A     | 750   |
| B     | 750   |
7. Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).
```sql
SELECT Product.model, price FROM Product 
	JOIN PC ON Product.model = PC.model WHERE maker = 'B'
UNION
SELECT Product.model, price FROM Product
	JOIN Laptop ON Product.model = Laptop.model WHERE maker = 'B'
UNION
SELECT Product.model, price FROM Product
	JOIN Printer ON Product.model = Printer.model WHERE maker = 'B'
```

| model | price     |
| ----- | --------- |
| 1121  | 850.0000  |
| 1750  | 1200.0000 |
8. Найдите производителя, выпускающего ПК, но не ПК-блокноты.
```sql
SELECT maker FROM Product WHERE type = 'PC'
EXCEPT
SELECT maker FROM Product WHERE type = 'Laptop'
```

| maker |
| ----- |
| E     |
9. Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker

```sql
SELECT DISTINCT maker 
FROM Product JOIN PC ON Product.model = PC.model WHERE PC.speed >= 450
```

| maker |
| ----- |
| A     |
| B     |
| E     |
10. Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price
```sql
SELECT model, price FROM Printer WHERE price = (SELECT MAX(price) FROM Printer)
```

| model | price    |
| ----- | -------- |
| 1288  | 400.0000 |
| 1276  | 400.0000 |
11. Найдите среднюю скорость ПК.
```sql
SELECT SUM(speed)/COUNT(speed) FROM PC
```

| |
|---|
|608|
12. Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.
```sql
SELECT SUM(speed)/COUNT(speed) FROM Laptop WHERE price > 1000
```

|     |
| --- |
| 700 |
13. Найдите среднюю скорость ПК, выпущенных производителем A
```sql
SELECT AVG(speed) 
FROM PC JOIN Product ON PC.model = Product.model WHERE Product.maker = 'A'
```

| |
|---|
|606|

---

Рассматривается БД кораблей, участвовавших во второй мировой войне. Имеются следующие отношения:  
- Classes (class, type, country, numGuns, bore, displacement)  
- Ships (name, class, launched)  
- Battles (name, date)  
- Outcomes (ship, battle, result)  
Корабли в «классах» построены по одному и тому же проекту, и классу присваивается либо имя первого корабля, построенного по данному проекту, либо названию класса дается имя проекта, которое не совпадает ни с одним из кораблей в БД. Корабль, давший название классу, называется головным.  

Отношение Classes содержит имя класса, тип (bb для боевого (линейного) корабля или bc для боевого крейсера), страну, в которой построен корабль, число главных орудий, калибр орудий (диаметр ствола орудия в дюймах) и водоизмещение ( вес в тоннах). 
В отношении Ships записаны название корабля, имя его класса и год спуска на воду. 
В отношение Battles включены название и дата битвы, в которой участвовали корабли.
В отношении Outcomes – результат участия данного корабля в битве (потоплен-sunk, поврежден - damaged или невредим - OK).  
Замечания. 
-  В отношение Outcomes могут входить корабли, отсутствующие в отношении Ships. 
- Потопленный корабль в последующих битвах участия не принимает.


14. Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий.
```sql
SELECT Ships.class, name, Classes.country 
FROM Ships JOIN Classes ON Ships.class = Classes.class 
WHERE Classes.numGuns >= 10
```

| class          | name           | country |
| -------------- | -------------- | ------- |
| Tennessee      | California     | USA     |
| North Carolina | North Carolina | USA     |
| North Carolina | South Dakota   | USA     |
| Tennessee      | Tennessee      | USA     |
| North Carolina | Washington     | USA     |
15. Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD
```sql
SELECT hd FROM PC GROUP BY hd HAVING COUNT(hd) >= 2
```

| hd   |
| ---- |
| 5.0  |
| 8.0  |
| 10.0 |
| 14.0 |
| 20.0 |
16. Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.
```sql
SELECT DISTINCT a.model, b.model, a.speed, a.ram 
FROM PC as a, PC as b 
WHERE a.speed = b.speed AND a.ram = b.ram AND a.model > b.model
```

| model | model | speed | ram |
| ----- | ----- | ----- | --- |
| 1233  | 1121  | 750   | 128 |
| 1233  | 1232  | 500   | 64  |
| 1260  | 1232  | 500   | 32  |
17. Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК.  Вывести: type, model, speed
```sql
SELECT DISTINCT type, L.model, L.speed FROM Product JOIN Laptop AS L 
ON Product.model = L.model 
WHERE L.speed < ALL (SELECT speed FROM PC)
```

| type   | model | speed |
| ------ | ----- | ----- |
| Laptop | 1298  | 350   |
18. Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price
```sql
SELECT DISTINCT maker, Pr.price
FROM Product JOIN Printer AS Pr ON Product.model = Pr.model
WHERE Pr.price = (SELECT min(price) FROM Printer WHERE color = 'y') 
	AND Pr.color = 'y'
```

|maker|price|
|---|---|
|D|270.0000|
19. Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов.  
Вывести: maker, средний размер экрана.
```sql
SELECT maker, AVG(L.screen)
FROM Product JOIN Laptop AS L ON Product.model = L.model
WHERE type = 'Laptop' GROUP BY maker
```

| maker | AVG(screen) |
| ----- | ----------- |
| A     | 13          |
| B     | 14          |
| C     | 12          |
20. Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК.
```sql
SELECT maker, COUNT(model) 
FROM Product
WHERE type = 'PC'
GROUP BY maker HAVING COUNT(model) >=3
```

| maker | COUNT(model) |
| ----- | ------------ |
| E     | 3            |


























































