```bash
#START METASPLOITABLE2
msfconsole

noapic

msfadmin
msfadmin

sudo systemctl enable postgresql
sudo systemctl start postgresql
sudo systemctl status postgresql

#SSH(HOST)
sudo /etc/init.d/ssh status
sudo dpkg-reconfigure openssh-server
sudo /etc/init.d/ssh start
netstat -tuln | grep :22

#POSTGRES(HOST)
psql -h 10.0.2.4 -p 5432 -U postgres -W
pass: postgres

#SMBD
ps aux | grep smbd
netstat -tuln | grep -E "139|445"
```

### Metasploitable2:

> **Metasploitable2** — это уязвимая виртуальная машина, предназначенная для тестирования на проникновение. Здесь ваши команды:

```bash
msfconsole #запускает Metasploit Framework консоль
noapic #параметр загрузки ядра, отключающий APIC
msfadmin / msfadmin #логин и пароль для Metasploitable2.
```
```bash
search exploit_name #поиск эксплойта
use exploit/path/to/exploit #использование эксплойта
exploit #запуск эксплойта
```

Возможные эксплойты:
- vsftpd
- backdoor
- bootforce
- postgres
- smb
### PostgreSQL:

```bash
sudo systemctl enable postgresql #включение автоматического запуска
sudo systemctl start postgresql #запуск службы PostgreSQL
sudo systemctl status postgresql #проверка текущего статуса PostgreSQL.
```



### SSH (HOST):

Команды для работы с OpenSSH:

1. **sudo /etc/init.d/ssh status** — Проверка статуса SSH-сервера.
    
2. **sudo dpkg-reconfigure openssh-server** — Переконфигурация OpenSSH-сервера.
    
3. **sudo /etc/init.d/ssh start** — Запуск SSH-сервера.
    
4. **netstat -tuln | grep :22** — Проверка, слушает ли SSH-сервер порт 22.
    

### PostgreSQL (HOST):

Подключение к PostgreSQL-серверу:

1. **psql -h 10.0.2.4 -p 5432 -U postgres -W** — Подключение к PostgreSQL-серверу на хосте 10.0.2.4 с портом 5432 и пользователем postgres.
    
2. **pass: postgres** — Пароль для пользователя postgres.
    

### SMBD:

Проверка статуса и сетевых соединений службы SMB:

1. **ps aux | grep smbd** — Поиск процесса smbd.
    
2. **netstat -tuln | grep -E "139|445"** — Проверка, слушает ли SMB-сервер порты 139 или 445.