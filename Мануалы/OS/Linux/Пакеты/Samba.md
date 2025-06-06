##### Установка
Необходимые пакеты: `samba`, `samba-client`
```bash
sudo apt install samba samba-client# Установка
```

```bash
sudo systemctl status smbd # Проверка статуса
```
***
##### Настройка конфигурации smb.conf
```bash
cp /etc/samba/smb.conf /etc/samba/smb.conf_sample # Резеврная копия дефолтного конфига
```

```bash
sudo nano /etc/samba/smb.conf # Настройка конфигурации
```

###### smb.conf (описание)
```bash
# Описание расшаренной папки
[shared]
    #Путь к папке, которую будем расшаривать
    path = /home/user/shared

    # Указываем, доступна ли эта папка для записи 
    read only = no  
    # Можно использовать writable = yes

    # Указываем, будет ли папка отображаться в сетевых ресурсах
    browsable = yes  
    # Можно также использовать visible = yes 

    # Устанавливаем права доступа к папке. 
    create mask = 0777
    directory mask = 0777
    # 0777 — полный доступ для всех

    # Указываем пользователя, который имеет доступ к этому ресурсу
    valid users = username  
    # Замените 'username' на имя пользователя

    # Указываем, какие пользователи могут редактировать или видеть файлы
    write list = username  
    # Только указанные пользователи могут изменять содержимое

    # Включаем/выключаем использование гостевого доступа
    guest ok = no  
    # Установка 'yes' разрешает доступ без пароля

    # Включение или отключение журналирования
    logging = file
    # /var/log/samba/log.smbd - путь к логам по дефолту

    # Устанавливаем максимальное количество подключений
    max connections = 10

    # Отключаем или включаем поддержу оповещений
    announce version = 3.0  
    # Вы можете использовать свою версию Samba

    # Применение фильтра для скрытия файлов с определённым расширением
    hide files = /*.tmp/*.bak  
    # Пример для скрытия временных и резервных файлов
``` 
###### smb.conf (мой)
```bash
#Пример
[global]
	workgroup = WORKGROUP
	security = user
	map to guest = bad user
	wins support = no
	dns proxy = no

[public]
	path = /home/user23/samba/public
	guest ok = yes
	force user = nobody
	browsable = yes
	writable = yes

[private]
	path = /home/user23/samba/private
	valid users = @smbgrp
	guest ok = no
	browsable = yes
	writable = yes
```

```bash
testparm -s # Проверка конфига
```

```bash
sudo systemctl restart smbd # Перезапуск Samba
sudo systemctl restart nmbd
```
***
##### Настройка директорий и пользователей:
```bash
mkdir /home/user23/samba # Директория для файлов
mkdir /home/user23/samba/public # Публичный каталог
mkdir /home/user23/samba/private # Приватный каталог
```

```bash
groupadd smbgrp # Создание группы пользователей
useradd user23 # Добавление пользователей Samba
usermod -aG smbgrp user23 # Добавление пользователей в группу
```

```bash
chmod -R 0755 /home/user23/samba/public # Назначения прав публичного каталога
```

```bash
# Привязка группы к директории
sudo chgrp -R smbgrp /home/user23/samba/private/
sudo chgrp -R smbgrp /home/user23/samba/public/
```

```bash
# Настройка прав директорий
sudo chmod 2770 /home/user23/samba/private/
sudo chmod 2775 /home/user23/samba/public/
```



```bash
chgrp smbgrp /home/user23/samba/private # Изменение группы к которой принадлежит приватная директория
```

```bash
smbpasswd -a user23 # Добавление пароля пользователю
smbpasswd -e user23 # Включение пользователя
```

```bash
groups user23  # Показ групп пользователя user23
```

***

##### Настройка межсетевого экрана
TCP-ports: 139, 445
UDP-ports: 137, 138
Для указания собственного диапазона адресов, замените значение после ключа “-s”:
```bash
iptables -A INPUT -p tcp -m tcp --dport 445 -s 10.0.0.0/24 -j ACCEPT # Порт 445
```

```bash
iptables -A INPUT -p tcp -m tcp --dport 139 -s 10.0.0.0/24 -j ACCEPT # Порт 139
```

```bash
iptables -A INPUT -p udp -m udp --dport 137 -s 10.0.0.0/24 -j ACCEPT # Порт 137
```

```bash
iptables -A INPUT -p udp -m udp --dport 138 -s 10.0.0.0/24 -j ACCEPT # Порт 138
```
***
Для сохранения правил и применения их после перезагрузки сервера следует воспользоваться пакетом iptables-persistent.
```bash
apt-get install iptables-persistent # Установка
```

```bash
sudo netfilter-persistent save # Сохранение правил
```

```bash
iptables -L # Проверка существующих правил
```
***
##### Настройка общих папок в Windows

Отключение парольной защиты: `Панель управления` → `Сеть` → `Центр управления сетями и общим доступом` → `Расширенные настройки общего доступа` → `Все сети` → `Общий доступ с парольной защитой` → `Отключить общий доступ с парольной защитой` → `Сохранить изменения`.

Публичная директория:
1. `"Public"` → `Свойства` → `Расширенная настройка` → `Открыть общий доступ к этой папке` → `Разрешения` → `Все` → `Полный доступ`
2. `"Public"` → `Свойства` → `Общий доступ` → `(Добавляем пользователя "Все")` → Ч`тение и запись` → `Поделиться`

Приватная директория:
3. `"Private"` → `Свойства` → `Расширенная настройка` → `Открыть общий доступ к этой папке` → `Разрешения` → `(Добавляем пользователя)` → `Полный доступ`
4. `"Private"` → `Свойства` → `Общий доступ` → `(Добавляем пользователя)` → `Чтение и запись` → `Поделиться`
***

##### Подключение к общим папкам из Linux
```bash
sudo apt-get install smbclient
```

```bash
smbclient -U <Имя_пользователя> <IP-адрес><Имя_каталога_на_сервере>
```

```bash
smbclient //192.168.102.58/public -N # Пример гостевого подключения
```
> Параметр `-N` отключает запрос пароля, если доступ к шару разрешен без него
###### Параметры smbclient
```bash
# Параметры подключения и команды для smbclient

# -U : Указание пользователя для подключения
# -N : Не запрашивать пароль, если используется анонимный доступ (например, если доступ открыт для всех)
# -L : Получить список доступных шары на удаленном хосте
# -d : Уровень отладки. Число от 0 (по умолчанию) до 10 (очень подробный вывод)
# -m : Указать максимальную версию SMB-протокола (например, SMB2 или SMB3)
# -W : Указать рабочую группу
# -p : Указать порт для подключения (по умолчанию 445)
# -V : Показать версию smbclient
# -I : Указать IP-адрес для подключения (если имя хоста не работает)
# -S : Указать NetBIOS-имя сервера
# -b : Установить размер буфера отправки в байтах
# -t : Установить таймаут в секундах
# -g : Вывести результат в формате, удобном для парсинга
# -q : Тихий режим (minimal output)
# -P : Использовать аутентификацию через Kerberos
# -k : Использовать Kerberos аутентификацию
# -l : Запустить программу только для проверки существования указанных каталогов (но не подключаться)
# -E : Принудительно использовать сетевое подключение как отдельное задание (делать это быстрее)
# -c : Выполнение команды после подключения (например, "ls" для вывода содержимого)
# --option=name=value : Указать конкретную настройку smbclient (например, --option=username=...)
# --no-pass : Не использовать пароль при подключении (аналог -N)
# --realm : Указать домен для Kerberos аутентификации
# --help : Получить помощь по использованию команды smbclient
# --version : Вывести информацию о версии программы smbclient
```
###### Примеры команд smbclient
```bash
# Подключение к SMB-шаре без пароля
smbclient //192.168.102.58/public -N  # Подключаемся без пароля (для общего доступа)

# Подключение с пользователем и паролем
smbclient //192.168.102.58/public -U user23  # Указываем пользователя (пароль будет запрашиваться)

# Получение списка доступных шары
smbclient -L 192.168.102.58 -U user23  # Список всех шары на хосте

# Указание версии SMB-протокола (например, SMB3)
smbclient //192.168.102.58/public -U user23 -m SMB3  # Подключаемся через SMB3

# Использование отладки для более подробного вывода
smbclient //192.168.102.58/public -U user23 -d 3  # Уровень отладки 3 (подробный вывод)

# Указание порта для подключения
smbclient //192.168.102.58/public -U user23 -p 139  # Подключаемся через порт 139 (например)

# Просмотр версии smbclient
smbclient -V  # Печатает версию программы smbclient

# Использование Kerberos аутентификации
smbclient //192.168.102.58/public -U user23 -k  # Используем Kerberos для аутентификации

# Указание рабочего домена (для Windows)
smbclient //192.168.102.58/public -U user23 -W WORKGROUP  # Указываем рабочую группу

# Подключение с указанием IP-адреса вместо имени хоста
smbclient //192.168.102.58/public -I 192.168.102.58  # Указываем IP для подключения

# Список параметров (выполнится без подключения)
smbclient //192.168.102.58/public -l  # Запуск проверки существования каталога на сервере

# Выполнение команд при подключении (например, просмотреть список файлов)
smbclient //192.168.102.58/public -U user23 -c "ls"  # После подключения выполняется команда "ls" (список файлов)
```
***
##### Подключение к общим папкам из Windows
`Win+R` → `<IP-адрес>`

```bash
\\192.168.102.138
```



##### Монтирование общей бабки как сетевого диска
фывыфв
##### Samba на хостинг-сервере
ыфвф
##### Прочие команды
```bash
sudo pdbedit -L # Список пользователей настроеных в Samba
```

```bash
sudo systemctl list-dependencies samba-ad-dc.service # Проверка зависимостей Samba
```

```bash
ls -ld /home/user23/samba/private # Проверка прав на private
```

```bash
groups user23 # Проверка групп user23 
```


***
Прочие ресурсы: https://serverspace.ru/support/help/configuring-samba/?utm_source=google.com&utm_medium=organic&utm_campaign=google.com&utm_referrer=google.com

`edfkseod`

groups user23 

stat /home/user23/samba/private/

getfacl /home/user23/samba/public/


ls -l /home/user23/samba/private/
