
```bash
#!/bin/bash

# Проверяем, что был передан хотя бы один аргумент (директория или файл для удаления)
if [ "$#" -eq 0 ]; then
    echo "Ошибка: не указаны файлы или директории для удаления."
    exit 1
fi

# Функция для очистки файлов и директорий
cleanup() {
    echo "Очистка файлов и директорий..."
    for item in "$@"; do
        if [ -e "$item" ]; then
            echo "Удаление: $item"
            rm -rf "$item"
        else
            echo "Не найдено: $item"
        fi
    done
    echo "Очистка завершена."
}

# Вызов функции очистки с переданными аргументами
cleanup "$@"
```

### Пример использования:

Предположим, что ваш скрипт, с которым вы работаете, создает файлы и директории:

- `/home/user23/Argus_Linux_build_22.7z`
- `/data/Project/Argus`

Если вы хотите запустить очистку этих директорий и файлов, вызовите `cleanup.sh` с соответствующими путями:

```bash
bash cleanup.sh /home/user23/Argus_Linux_build_22.7z /data/Project/Argus /home/user23/smbcredentials
```