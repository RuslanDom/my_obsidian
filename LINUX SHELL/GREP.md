# Команда: grep

Команда `grep` (global regular expression print) — это инструмент в Linux и Unix, который помогает вам искать определенный текст в файлах. Вы можете использовать его для поиска слов, фраз или шаблонов в одном или нескольких файлах, что упрощает поиск и управление информацией.

Образец файла poem.txt

![[Pasted image 20250224215344.png]]

---
### Поиск без учета регистра

С помощью метки **-i** можно находить все вхождения фрагмента текста в независимости от регистра:

```shell
grep -i little poem.txt
```
![[Pasted image 20250224215435.png]]

---
### Поиск по всему слову

Параметр **-w** позволяет возвращать только те строки, в которых выражение будет отдельным словом, а не подстрокой другого слова:

```shell
grep -w ‘a’ poem.txt
```
![[Pasted image 20250224215514.png]]

---
### Обратный поиск

Если необходимо найти все строки без вхождений какой-то последовательности символов, то нужно использовать флаг **-v**:

```shell
grep -v ‘to’ poem.txt
```
Такой поиск еще называют инвертированным. Вывод будет содержать все строки без вхождения **to**:
![[Pasted image 20250224215618.png]]

---

### Поиск текста в файле

Найдем все вхождения предлога **to** в стихе. Для этого применим команду:

```shell
grep ‘to’ poem.txt
```

После выполнения команды получим следующее:
![[Pasted image 20250224220017.png]]

---
С помощью параметра **-e** можно указать сразу несколько условий:

```shell
grep -e ‘to’ -e ‘Little’ poem.txt
```
Тогда в результате мы получим все вхождения, содержащие хотя бы одно из перечисленных условий:
![[Pasted image 20250224220057.png]]

---
Флаг **-n** добавляет к выводу номер строки с совпадением:
```shell
grep -n ‘sky’ poem.txt
```
![[Pasted image 20250224220152.png]]

---
Если в поисковом запросе нужно использовать специальные символы, то нужно явно указывать это с помощью **-F**. Иначе будет ошибка о неверно заданном регулярном выражении.
```shell
grep -F ‘[’ poem.txt
```

---

### Поиск совпадений в нескольких файлах

Используя маски, в grep можно осуществлять поиск шаблона одновременно в нескольких файлах:
```shell
grep Error /var/log/messages* 
```
![[Pasted image 20250224220339.png]]

Для удобства пользователей при поиске во множестве файлов, в начале каждой строки указывается название путь до файла.

При необходимости отфильтровать вывод выполненной команды, нужно перенаправить его с помощью оператора **|**. В таком случае не нужно указывать файл, так как grep осуществит поиск в выводе:
```shell
ps aux | grep ‘17:47’ 
```

При выполнении этой команды мы получим список конкретных процессов:
![[Pasted image 20250224220428.png]]

---
### Вывод контекста

Иногда очень полезно не только извлекать саму строку с совпадением, но и включать в вывод строки, предшествующие и следующие за ней. Например, если мы стремимся извлечь все ошибки из файла с логами, но предполагаем, что после строки с основной ошибкой может содержаться важная информация, то в таком случае с grep мы можем отобразить дополнительные строки.

Для этого воспользуемся опций **-A** с указанием количества строк после нее, которые хотим увидеть в выводе:
```shell
grep -A3 ‘twirling’ poem.txt 
```
![[Pasted image 20250224220535.png]]

Если нужно получить строки до найденного вхождения, то тогда поможет аналогичный параметр **-B**:
```shell
grep -B2 ‘wind’ poem.txt 
```
![[Pasted image 20250224220608.png]]

При ситуации, когда нужно получить строки и до, и после, применяем параметр **-С**:
```shell
grep -С1 ‘on’ poem.txt
```
![[Pasted image 20250224220719.png]]

---

### Рекурсивный поиск в grep

Во всех примерах до этого мы искали в пределах одного файла. Но у этой утилиты есть возможности выполнить рекурсивных поиск в нескольких файлах, находящихся в одном каталоге или подкаталогах. 

Попробуем найти все вхождения ‘the’ в каталоге root. Осуществить это поможет опция **-r**:
```shell
grep -r ‘the’ /root
```
![[Pasted image 20250224220948.png]]

Обычно в папке с файлами присутствуют двоичные файлы, для которых маловероятно проведение поиска. Чтобы их пропустить, нужно добавить опцию **-l**:
```shell
grep -rl ‘the’ /root
```

Ожидаемо результат изменился:
![[Pasted image 20250224221019.png]]
К часть файлов есть доступ только у суперпользователей и, чтобы искать и по ним тоже, нужно запускать grep через sudo. Либо же можно воспользоваться параметром -s – тогда сообщения об ошибках будут скрыты, а файлы пропущены:
```shell
grep -rls ‘the’ /root
```

---
### Количество строк

Для некоторых задач может понадобиться не найти все строки с вхождением, а узнать количество совпадений. Для этого нужно применить параметр **-c**, счетчик:
```shell
grep -c ‘ll’ poem.txt
```
![[Pasted image 20250224221303.png]]

---
### Вывод имен файлов

Опцией **-l** в grep можно задать вывод только имен файлов, где найдено хотя бы одно совпадение. Например, при выполнении следующей команды будут показаны названия файлов, в которых обнаружены совпадения с ключевым словом ‘root’:
```shell
grep -lr ‘root’ /var/log
```
![[Pasted image 20250224221415.png]]

---
### Цветный вывод

Изначально, grep не выделяет совпадения цветом, но во многих дистрибутивах установлен алиас, который активирует эту функцию. Однако при использовании команды с sudo эта функция может не работать. Для подключения подсветки вручную, нужна опция **—color** с параметром **always**:
```shell
grep --color=always ‘ll’ poem.txt
```
![[Pasted image 20250224221452.png]]

---
## **Доступ к системной информации с помощью grep**

Еще одной полезной функцией grep является возможность просматривать системную информацию. 

Например, с помощью следующей команды можно узнать модель вашего компьютера:
```shell
grep -i 'Model' /proc/cpuinfo
```
![[Pasted image 20250224221601.png]]










## Синтаксис регулярных выражений

### POSIX BRE

Используется по умолчанию.

- `.` — любой символ
- `[abc]` — один из a, b, c
- `[^a-z]` — любой символ, кроме маленьких латинских букв
- `^` и `$` — начало и конец строки
- `\( \)` — подвыражение
- `*` — ноль или более раз
- `\{m,n\}` — от _m_ до _n_ раз
- `\n` — backreference

### POSIX ERE

С ключом -E.

- не надо использовать обратный слеш для `( )`, `{ }`
- `?` — ноль или один раз
- `+` — один или более раз
- `|` — или

### Perl

С ключом -P.

## Отдельные команды rgep, fgrep, ogrep

Существуют модификации grep: _egrep_ (с обработкой расширенных регулярных выражений), _fgrep_ (трактующая символы $*[]^|()\ буквально), _rgrep_ (с включённым рекурсивным поиском). Как сказано в руководстве man (с точностью до перевода):

- egrep — то же самое, что grep -E
- fgrep — то же самое, что grep -F
- rgrep — то же самое, что grep -r

## Полезные опции

- -c — только подсчёт строк с совпадениями (не самих совпадений).
- -v — наоборот, вывести несматчившиеся строки.
- -o — вывести только сматчившиеся фрагменты строк.
- -i — регистронезависимый поиск.
- -A, -B, -C — вывод контекста совпадения (несколько строк after, before строки с совпадением).
- -w, --word-regexp — избавляет от проблемы ложных срабатываний, например, при поиске слова word на строках вроде overworded.
---

***Пример: 
Эта команда ищет строку `“error”`в `/var/log/syslog`файле.
```shell
grep "error" /var/log/syslog
```

Для поиска строки во всех файлах каталога и его подкаталогов используйте `-r`опцию:

```shell
grep -r "search_string" /path/to/directory
```

Для выполнения поиска без учета регистра используйте `-i`опцию: 

```shell
grep -i "search_string" filename
```

Чтобы подсчитать количество появлений шаблона, используйте `-c`опцию:

```shell
grep -c "search_string" filename
```

Для поиска шаблона в нескольких файлах можно использовать подстановочные знаки:
Эта команда выполняет поиск `“error”`во всех файлах .log в `/var/log/`каталоге.

```shell
grep "error" /var/log/*.log
```

## Параметры grep

`grep`также поставляется с различными опциями для настройки его поведения:

1. `-i`: Игнорировать различия в регистре.
2. `-v`: Инвертируйте совпадение, чтобы выбрать несовпадающие строки.
3. `-c`: Подсчитайте количество совпадающих строк.
4. `-l`: Список имен файлов, содержащих совпадение.
5. `-L`: Список имен файлов, не содержащих совпадений.
6. `-n`: Добавляйте к каждой строке вывода номер строки.
7. `-H`: Распечатать имя файла для каждого совпадения.
8. `-r`или `-R`: Прочитать все файлы в каждом каталоге рекурсивно.
9. `-w`: Сопоставлять только целые слова.
10. `-x`: Сопоставлять только целые строки.
11. `-E`: Используйте расширенные регулярные выражения (ERE).
12. `-F`: Интерпретировать шаблон как список фиксированных строк (fgrep).
13. `-q`: Тихо, ничего не писать в стандартный вывод.

## pidof
---
Аналогично, поиск по имени.

```shell
sobols@sobols-VirtualBox:~$ pgrep init
sobols@sobols-VirtualBox:~$ pgrep systemd -l
1 systemd
201 systemd-journal
243 systemd-udevd
673 systemd-logind
1084 systemd
sobols@sobols-VirtualBox:~$ pgrep ^systemd$ -l
1 systemd
1084 systemd
sobols@sobols-VirtualBox:~$ pidof systemd
1084
sobols@sobols-VirtualBox:~$ pidof init
1
```