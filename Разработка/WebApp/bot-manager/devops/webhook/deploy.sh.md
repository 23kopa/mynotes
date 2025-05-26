Путь:  `/var/www/botmanager/deploy.sh`
```bash
#!/bin/bash

# Включаем логирование всего вывода в файл
exec > /var/log/deploy.log 2>&1
echo "===== DEPLOY START: $(date) ====="
echo "👤 Скрипт выполняется от пользователя: $(whoami)"

# Пути
PROJECT_DIR="/var/www/botmanager"
SERVICE_NAME="botmanager"

# Переход в директорию проекта
echo "📁 Переход в директорию $PROJECT_DIR"
cd "$PROJECT_DIR" || { echo "❌ Ошибка: не удалось перейти в $PROJECT_DIR"; exit 1; }

# Получение последних изменений из origin/main
echo "📥 Получение обновлений из git..."
git fetch origin
git reset --hard origin/main

# Очистка неотслеживаемых файлов, кроме исключений
echo "🧹 Очистка untracked файлов (venv, .env и т.д. сохраняются)"
git clean -fd -e venv -e .env -e .git -e .gitignore -e deploy.sh

# Перезапуск сервиса
echo "🔁 Перезапуск сервиса $SERVICE_NAME..."
if systemctl restart "$SERVICE_NAME"; then
    echo "✅ Сервис $SERVICE_NAME успешно перезапущен"
else
    echo "❌ Ошибка при перезапуске сервиса $SERVICE_NAME"
    exit 1
fi

# Завершение
echo "✅ DEPLOY FINISHED: $(date)"
```