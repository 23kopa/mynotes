```bash
#!/bin/bash

# --- НАСТРОЙКИ Telegram ---
BOT_TOKEN="7505610899:AAF2RxxuifLLUhqprRvDDrOBs_-1TrdPADU"
CHAT_ID="541867126"

send_telegram() {
    local message="$1"
    curl -s -X POST "https://api.telegram.org/bot${BOT_TOKEN}/sendMessage" \
        -d chat_id="${CHAT_ID}" \
        -d text="$(hostname): $message"
}

# --- Создание папки для бэкапа ---
BACKUP_DIR="/root/Backups/$(date +'%Y-%m-%d')"
mkdir -p "$BACKUP_DIR/TG-Bots"
if [[ $? -ne 0 ]]; then
    send_telegram "❌ Не удалось создать директорию бэкапа: $BACKUP_DIR"
    exit 1
fi

# --- Копирование файлов ботов ---
rsync -avz --exclude='__pycache__' --exclude='vika_venv' /root/TG-Bots/vika_bot/ "$BACKUP_DIR/TG-Bots/vika_bot/"
if [[ $? -ne 0 ]]; then
    send_telegram "❌ Ошибка при бэкапе vika_bot"
    exit 1
fi

rsync -avz --exclude='__pycache__' --exclude='my_venv' /root/TG-Bots/my_bot/ "$BACKUP_DIR/TG-Bots/my_bot/"
if [[ $? -ne 0 ]]; then
    send_telegram "❌ Ошибка при бэкапе my_bot"
    exit 1
fi

# --- Удаление старых бэкапов (оставляем только 2) ---
cd /root/Backups || {
    send_telegram "❌ Не удалось перейти в /root/Backups"
    exit 1
}

BACKUP_ITEMS=$(ls -1tr | grep -v '^backups\.sh$')
TO_DELETE=$(echo "$BACKUP_ITEMS" | head -n -2)

if [[ -n "$TO_DELETE" ]]; then
    echo "$TO_DELETE" | xargs -I {} rm -rf {}
    if [[ $? -ne 0 ]]; then
        send_telegram "⚠️ Ошибка при удалении старых бэкапов"
    fi
fi

# --- Успешное завершение ---
send_telegram "✅ Бэкап успешно завершён: $(date +'%Y-%m-%d')"

```

`test_notify.sh`
```bash
#!/bin/bash

BOT_TOKEN="7505610899:AAF2RxxuifLLUhqprRvDDrOBs_-1TrdPADU"
CHAT_ID="541867126"

send_telegram() {
    local message="$1"
    curl -s -X POST "https://api.telegram.org/bot${BOT_TOKEN}/sendMessage" \
        -d chat_id="${CHAT_ID}" \
        -d text="$(hostname): $message"
}

# Тестовое уведомление
send_telegram "🔔 Это тестовое уведомление из скрипта"

```