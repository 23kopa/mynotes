#### Pi-Hole

*Вариант 1 — Назначить **статический IP через DHCP резервацию** (рекомендуется)*
*Вариант 2 — Назначить статический IP **вручную** на Raspberry Pi*

```
curl -sSL https://install.pi-hole.net | bash
```


Настройка Pi-Hole:
1. `Custom`
2. `127.0.0.1#5053`
3. `Yes`
4. `Hide domains`

Для домашней сети выбирай: `Hide Domains`
> Ошибка Check for existing repository — это просто **проверка**, что раньше там не было клонов. Скрипт видит, что репозиториев еще нет, и выполняет клонирование.

Итоговый вывод:
```
Configure your devices to use the Pi-hole as their DNS server
using:

IPv4: 192.168.0.101
IPv6: Not Configured
If you have not done so already, the above IP should be set to
static.
View the web interface at http://pi.hole/admin:80 or
http://192.168.0.101:80/admin

Your Admin Webpage login password is W35eu1d1
```

- Твой Pi-hole находится по IPv4 адресу **192.168.0.101** (не 192.168.0.105, как ты писал ранее).
- IPv6 пока не настроен.
- Тебе нужно в настройках сети на устройствах указать именно **192.168.0.101** в качестве DNS-сервера.
- Веб-интерфейс доступен по адресу:  
    [http://pi.hole/admin](http://pi.hole/admin) или http://192.168.0.101/admin
- Пароль для входа в админку: `W35eu1d1`.

Остановка
```bash
sudo systemctl stop pihole-FTL
sudo systemctl disable pihole-FTL
```
***
#### cloudflared
1. Скачиваем и устанавливаем `cloudflared`
```
sudo apt update
sudo apt install -y curl
curl -LO https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm
sudo mv cloudflared-linux-arm /usr/local/bin/cloudflared
sudo chmod +x /usr/local/bin/cloudflared
```

2. Проверяем, что `cloudflared` установлен:
```
cloudflared --version
```

3. Создаём системный сервис для `cloudflared`
```
sudo nano /etc/systemd/system/cloudflared.service
```

```ini
[Unit]
Description=cloudflared DNS over HTTPS proxy
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/cloudflared proxy-dns --port 5053 --upstream https://1.1.1.1/dns-query
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```sh
sudo systemctl daemon-reload
sudo systemctl enable cloudflared
sudo systemctl start cloudflared
sudo systemctl status cloudflared
```

5. В веб-интерфейсе Pi-hole → **Settings → DNS**
```
127.0.0.1#5053
127.0.0.1#5053
```

6. Проверяем, что DNS-запросы проходят через DoH

```
dig pi-hole.net @127.0.0.1 -p 5053
```

Остановка
```bash
sudo systemctl stop cloudflared
sudo systemctl disable cloudflared
```
