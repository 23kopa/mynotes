# 🤖 **mybot — универсальный Telegram-бот**  
Полезный помощник для разработчиков, пентестеров и не только.
***

## 🛠️ Установка и запуск
### 📦 Установка через Docker:
1. Разархивируй файлы бота в нужную директорию.
2. Используй команды ниже для запуска:

```
# 🔧 Собираем образ
docker build -t <IMAGE-NAME> .

# 🚀 Запускаем контейнер
docker run -d --name <CONTAINER-NAME> <IMAGE-NAME>
```

### 🔁 Пересборка после изменений:

```
# ⛔ Остановка контейнера
docker stop <CONTAINER-NAME>

# 🗑️ Удаление контейнера (опционально)
docker rm <CONTAINER-NAME>

# 🧼 Удаление образа
docker rmi <IMAGE-NAME>

# ✏️ Вносим изменения в код

# 🔧 Сборка нового образа
docker build -t <IMAGE-NAME> .

# 🚀 Запуск контейнера
docker run -d --name <CONTAINER-NAME> <IMAGE-NAME>
```
***
### 🖥️ Запуск на VPS через systemd:
Создай юнит-файл:
```
sudo nano /etc/systemd/system/mybot.service
```
Пример содержимого:
```
[Unit]
Description=My Telegram Bot
After=network.target

[Service]
User=root
WorkingDirectory=/root/TG-Bots/my_bot
ExecStart=/root/TG-Bots/my_bot/my_venv/bin/python main.py
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```
Далее:
```
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable mybot
sudo systemctl start mybot
```
---
## 🤖 Основной функционал

### 🧠 **Диалог с ChatGPT**
- Встроенная интеграция с OpenAI ChatGPT для общения и помощи прямо в Telegram.

### 📝 **Заметки и напоминания**
- Добавление заметок с указанием даты и времени 🕒.
- В назначенное время бот присылает напоминание 💬.

---

## 📎 Загрузка и конвертация файлов

- 🎵 **Извлечение аудио из видео** (YouTube-dl + ffmpeg).
- 📤 **Конвертация PDF → TXT / DOCX / Изображения**.

---

## 📂 Хранилище

- 📁 **Файловый менеджер**: доступ к Google Drive или локальному диску.
- 🔍 **Поиск файлов** по названию, типу и дате.
- 🏷️ **Умные теги** — бот автоматически присваивает теги файлам по содержанию.

---

## 🕵️ OSINT-инструменты

- 🔍 Проверка IP / доменов через AbuseIPDB, VirusTotal, Shodan.
- 📬 Проверка email / username на утечки (HaveIBeenPwned, Dehashed).
- 🌐 Whois, DNS-запросы, геолокация IP, открытые порты (обёртка над Nmap).
- 👁️ Мониторинг сайтов — уведомления об изменениях (например, вакансии или багтрекеры).

---

## 🧪 Пентест инструменты

- ⚠️ Поиск CVE по ключевым словам или названию продукта.
- 🐚 Генератор payload'ов / reverse shell'ов (как revshells.com).
- 🚨 Honeytokens — создание кастомных маркеров через canarytokens.org или собственный генератор.

---

## 🔔 Интеграция с GitHub / GitLab

### 🧵 Уведомления:
- 🔔 Push / Pull / Issues / Releases — через Webhooks + ngrok.
- 🗞️ Персональный дайджест по репозиториям.
- 🚨 Оповещения об уязвимостях в зависимостях проектов.
### ⚙️ Утилиты:
- 🐛 Создание issue прямо через Telegram.
- 📄 Генерация шаблонов README / issue / PR по нажатию кнопки.
- 🧾 Мониторинг логов CI/CD — бот присылает ошибки из GitLab CI.

---
## 📌 Заключение
Бот **mybot** — это многофункциональный ассистент для кибербезопасности, автоматизации и управления проектами. Он удобен для личного использования и для небольших команд, легко масштабируется и дополняется новыми модулями. 💡
> 🔧 Поддержка и развитие: открытый исходный код, легко расширяемый функционал. Pull request’ы приветствуются!

---

📫 По вопросам и предложениям — обращайтесь в Telegram или создайте issue на GitHub ✨