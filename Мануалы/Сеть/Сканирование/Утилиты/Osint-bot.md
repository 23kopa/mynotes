ПО для безопасности:

| Функция                  | ПО             |
| ------------------------ | -------------- |
| Проверка email на утечки | LeakCheck API  |
| Анализ IP                | VirusTotal     |
| Анализ доменов           | AlienVault OTX |
| Поиск по соцсетям        | Sherlock       |
API-Keys:

| Telegram   | 7746854574:AAFTnRDQxaR0Ylam_mbe8txOJoFMKvo2Ah0                                   |
| ---------- | -------------------------------------------------------------------------------- |
| Abuseipdb  | 85135212dec5a570576240212e7f6d2af86441b7d5021aed4362d1b32b520af39f60fb0e9c554cf7 |
| LeakCheck  | d5223ae81bf20f38396931060b9bbd79c2e7df1f                                         |
| VirusTotal | a6e9516f86ec8a6d03b4799a5b457144a93cf015f2662bfd51ae9b49388d9ba6                 |
| Sherlock   | local                                                                            |

***
Содержимое бота:

| 1   | main.py        | точка входа, запуск бота                        |
| --- | -------------- | ----------------------------------------------- |
| 2   | config.py      | хранение конфигурации (токен, БД)               |
| 3   | db.py          | функции работы с базой данных                   |
| 4   | handlers.py    | обработчики команд и сообщений                  |
| 5   | kb.py          | клавиатуры (статические и динамические)         |
| 6   | admin.py       | админские команды и функции                     |
| 7   | middlewares.py | мидлвари (например, логирование, антифлуд)      |
| 8   | states.py      | состояния FSM, CallbackData                     |
| 9   | text.py        | все текстовые сообщения бота                    |
| 10  | utils.py       | вспомогательные функции (например, API-запросы) |
```sh
touch main.py config.py db.py handlers.py kb.py admin.py middlewares.py states.py text.py utils.py #Команда создания
```
***
Библиотеки pip:
```python
#Установка библиотек
pip install aiogram  # Библиотека для создания Telegram-ботов 
pip install aiohttp  # Асинхронная работа с HTTP-запросами 
pip install requests  # Простая работа с HTTP-запросами
pip install beautifulsoup4  # Парсинг HTML и извлечение данных из веб-страниц
pip install requests-html  # Расширенная работа с HTML-страницами 
pip install rich  # Красивый вывод в терминале
pip install pandas  # Работа с таблицами и данными
pip install requests_futures  # Асинхронные HTTP-запросы
pip install colorama  # Цветной вывод текста в консоли
pip install sqlalchemy  # Работа с базами данных (ORM для SQL)
pip install sqlite3  # Встроенная база данных SQLite

```
***
###### Подключение к боту

Создание виртуального окружения:
```sh
python3 -m venv venv #Создать в папке с проектом
```

Подключение к виртуальному окружениюу:
```sh
cd ~/name-bot #Директория бота

source venv/bin/activate #Активирование вирутального окружения
```

Включение работы бота 24/7:

```sh
nohup python main.py > bot.log 2>&1 &
```

```sh
ps aux | grep python #Просмотр фоновой активности 

cat nohup.out #Просмотр процессов в фоновом режиме
```

```sh
kill <ID> #Мягкое выключение

kill -9 <ID> #Принудительное выключениеу
```
***
Актуальное использование scherlock:
```
python3 -m sherlock_project.sherlock kopa23
```
***
Очистка файлов:
```sh
echo "" > main.py
echo "" > config.py
echo "" > db.py
echo "" > handlers.py
echo "" > kb.py
echo "" > admin.py
echo "" > middlewares.py
echo "" > states.py
echo "" > text.py
echo "" > utils.py
```
***

