Зайди в консоль и лучше зайди от root, чтобы постоянно команду sudo не писать, но вообще пох, как те удобнее
```sh
sudo su
# Потом введи пароль
```
***
## 1. Установи пакеты на Linux.
```sh
sudo apt update
sudo apt install python3-venv wget nano
```

```sh
mkdir Parser # Создание папки
cd Parser # Переход в папку
```
***
## 2. Установи Google Chrome (обязательно).

```sh
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

```sh
sudo apt-get install fonts-liberation
```

```sh
sudo dpkg -i google-chrome-stable_current_amd64.deb
```
***
## 3. Установи ChromeDriver (обязательно).
```sh
wget  https://storage.googleapis.com/chrome-for-testing-public/133.0.6943.98/linux64/chromedriver-linux64.zip # Пиши эту команду в той папке где хочешь чтобы лежал архив
```

```sh
unzip chromedriver-linux64.zip # Очевидно разархивация)))0)
```

```sh
cd chromedriver-linux64 # Переход в папку где лежит драйвер
```

```sh
sudo mv chromedriver /usr/local/bin/ # Перенос драйвера в bin (шоб запускать)
```

```sh
sudo chmod +x /usr/local/bin/chromedriver # Установка прав на исполнение 
```
***
```sh
google-chrome --version
chromedriver --version
```

```sh
# Они должны быть одних версий
Google Chrome 133.0.6943.98 
ChromeDriver 133.0.6943.98
```
***
## 4. Создай директорию где будешь хранить парсер.
```sh
mkdir Parser # Создай директорию
cd Parser # Перейди туда
```
***
## 5. В директории с парсерами создай виртуальное окружение.
```python
python3 -m venv venv # Создание виртуального окружения
```

```sh
source venv/bin/activate # Активация 
```

> Чтобы выключить потом пропиши deactivate просто
***
## 6. Установи библиотеки python.
```
pip3 install bs4
```

```
pip3 install selenium
```
***
## 7. Скрипт.
1. Создай скрипт 
```sh 
touch parser.py
```

2. Вставь туда этот код 
```python
import time
import re
from urllib.parse import quote
from datetime import datetime
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from concurrent.futures import ThreadPoolExecutor
from tqdm import tqdm
import signal
import sys

# Флаг отмены
cancel_parsing = False

# Обработчик сигнала для отмены парсинга
def signal_handler(sig, frame):
    global cancel_parsing
    cancel_parsing = True
    print("\nПарсинг отменен пользователем.")
    sys.exit(0)

signal.signal(signal.SIGINT, signal_handler)

# Настройка Selenium
chrome_options = Options()
chrome_options.add_argument('--disable-gpu')
chrome_options.add_argument('--no-sandbox')

# Функция для выбора города и страны
def choose_city_and_country():
    cities = {
        '1': {'country': 'Словакия'},
        '2': {'country': 'Польша'},
        '3': {'country': 'Германия'},
        '4': {'country': 'Армения'},
        '5': {'country': 'Грузия'},
        '6': {'country': 'Таиланд'}
    }

    print("Выберите страну:")
    for key, value in cities.items():
        print(f"{key}. {value['country']}")

    choice = input("Введите номер: ")
    if choice in cities:
        selected_country = cities[choice]['country']
        print(f"Вы выбрали: {selected_country}")
        return selected_country
    else:
        print("Неверный выбор, попробуйте снова.")
        return choose_city_and_country()

# Функция для выбора фильтра
def choose_filter():
    print("\nВыберите тип жилья:")
    print("1. Отели")
    print("2. Апартаменты")

    filter_choice = input("Введите номер фильтра: ")
    if filter_choice == '1':
        filter_type = '204'  # ID для отелей
        print("Вы выбрали: Отели")
    elif filter_choice == '2':
        filter_type = '201'  # ID для апартаментов
        print("Вы выбрали: Апартаменты")
    else:
        print("Неверный выбор, попробуйте снова.")
        return choose_filter()
    
    return filter_type

# Функция для валидации даты
def validate_date(date_str):
    try:
        return datetime.strptime(date_str, "%d-%m-%Y")
    except ValueError:
        print("Неверный формат даты. Пожалуйста, используйте формат dd-mm-yyyy.")
        return None

# Функция для парсинга информации об отеле по его ссылке
def parse_hotel(hotel_url):
    global cancel_parsing
    if cancel_parsing:
        print("Парсинг отменен.")
        return None

    options = Options()
    options.add_argument('--disable-gpu')
    options.add_argument('--no-sandbox')

    with webdriver.Chrome(options=options) as driver:
        try:
            driver.get(hotel_url)
            time.sleep(3)

            title = driver.title.strip()

            try:
                address_element = driver.find_element(By.CSS_SELECTOR, 'div.a53cbfa6de.f17adf7576')
                full_address = address_element.text.strip().split("\n")[0]
            except:
                full_address = "Адрес не указан"
            
            try:
                rating_element = driver.find_element(By.CSS_SELECTOR, '[data-testid="review-score-component"]')
                rating_text = rating_element.text.strip()
                match = re.search(r"(\d+(\.\d+)?)", rating_text)
                if match:
                    rating = match.group(1)
                else:
                    rating = "Оценка отсутствует"
            except:
                rating = "Оценка отсутствует"

            return title, full_address, hotel_url, rating

        except Exception as e:
            print(f"Ошибка при обработке {hotel_url}: {e}")
            return None

# Основная часть программы
if __name__ == "__main__":
    # Выбор города
    country = choose_city_and_country()

    # Выбор фильтра (отели или апартаменты)
    filter_type = choose_filter()

    # Ввод и валидация дат
    checkin_date = None
    checkout_date = None
    while not checkin_date:
        checkin_date_input = input("\nВведите дату заезда (dd-mm-yyyy): ")
        checkin_date = validate_date(checkin_date_input)

    while not checkout_date:
        checkout_date_input = input("Введите дату выезда (dd-mm-yyyy): ")
        checkout_date = validate_date(checkout_date_input)

    # Преобразуем даты в нужный формат для URL
    checkin_date_str = checkin_date.strftime("%Y-%m-%d")
    checkout_date_str = checkout_date.strftime("%Y-%m-%d")
    
    city_encoded = quote(country)
    
    # Формирование URL для поиска отелей с выбранными датами и фильтром
    url = f'https://www.booking.com/searchresults.ru.html?ss={city_encoded}&dest_id=&dest_type=city&checkin={checkin_date_str}&checkout={checkout_date_str}&group_adults=2&no_rooms=1&group_children=0&nflt=ht_id%3D{filter_type}'

    # После формирования URL добавьте вывод его в консоль для проверки
    print(f"\nСформированный URL: {url}\n")
    
    # Открытие страницы и получение ссылок на отели
    driver = webdriver.Chrome(options=chrome_options)
    driver.get(url)
    time.sleep(3)

    hotels_links = []
    hotel_elements = driver.find_elements(By.CSS_SELECTOR, '[data-testid="title"]')

    # Прокрутка страницы вниз для загрузки большего числа отелей
    last_height = driver.execute_script("return document.body.scrollHeight")
    while True:
        driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
        time.sleep(2)
        new_height = driver.execute_script("return document.body.scrollHeight")
        if new_height == last_height:
            break
        last_height = new_height

    # Добавим строку прогресса для загрузки отелей
    for hotel in tqdm(hotel_elements[:20], desc="Загрузка ссылок", unit="отель"): 
        if cancel_parsing:
            break  # Прерывание цикла, если отмена активирована
        try:
            hotel_link_element = hotel.find_element(By.XPATH, "./ancestor::a")
            hotel_url = hotel_link_element.get_attribute("href")
            hotels_links.append(hotel_url)
        except Exception as e:
            print(f"Ошибка при получении ссылки: {e}")

    driver.quit()

    if cancel_parsing:
        print("Парсинг отменен до начала парсинга данных.")
    else:
        # Парсинг информации об отелях с прогресс-баром
        with ThreadPoolExecutor(max_workers=5) as executor: 
            results = list(tqdm(executor.map(parse_hotel, hotels_links), desc="Парсинг", total=len(hotels_links)))

        hotels = [res for res in results if res]

        # Сохранение данных в файл
        with open("result.txt", "w", encoding="utf-8") as file:
            for hotel in hotels:
                file.write(f"Название: {hotel[0]}\n")
                file.write(f"Полный адрес: {hotel[1]}\n")
                file.write(f"Рейтинг: {hotel[3]}\n")
                file.write(f"Ссылка: {hotel[2]}\n")
                file.write("-" * 50 + "\n")

        print("Парсинг завершен. Данные сохранены в файл result.txt")

```
***


***
Инсталлятор делал так: 
```
pip3 install pyinstaller
```

```
pyinstaller --onefile par_barcmd.py
```

```
chmod +x par_barcmd
```