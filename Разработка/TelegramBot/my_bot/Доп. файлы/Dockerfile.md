```
# Используем официальный образ Python
FROM python:3.11

# Устанавливаем рабочую директорию в контейнере
WORKDIR /vika_app

# Копируем файлы проекта в контейнер
COPY . /vika_app

# Устанавливаем зависимости из requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Устанавливаем переменные окружения (если нужно, можно добавить)
# ENV VAR_NAME value

# Запуск приложения
CMD ["python3", "vikamain.py"]

```