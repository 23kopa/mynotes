Что у тебя настроено в `docker-compose.yml`:

1. **cloudflared** — локальный DNS-over-HTTPS прокси на порту 5053, чтобы Pi-hole мог использовать защищённый DNS от Cloudflare.
    
2. **pihole** — локальный DNS-сервер и блокировщик рекламы.
    - Слушает 53 порт TCP и UDP, проброшенный на хост.
    - DNS запросы Pi-hole направлены на `127.0.0.1#5053`, то есть через cloudflared.
    - Веб-интерфейс Pi-hole доступен на 80 порту.
        
3. **wireguard** — VPN сервер на 51820 UDP порту.
    - Проброшен порт 51820 UDP.
    - Использует `your.domain.com` или внешний IP для подключения клиентов.
    - Настроен на IP-пул `10.13.13.0/24`.
    - Клиенты будут использовать Pi-hole в качестве DNS (через 127.0.0.1).


`docker-compose.yml`
```yaml
version: "3.8"

services:
  cloudflared:
    image: cloudflare/cloudflared:latest
    command: proxy-dns --port 5053 --upstream https://1.1.1.1/dns-query --address 0.0.0.0
    restart: unless-stopped

  pihole:
    image: pihole/pihole:latest
    environment:
      TZ: 'Europe/Moscow'
      WEBPASSWORD: 'user23'
      DNS1: cloudflared:5053
      DNS2: cloudflared:5053
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80"
    volumes:
      - ./etc-pihole/:/etc/pihole/
      - ./etc-dnsmasq.d/:/etc/dnsmasq.d/
    cap_add:
      - NET_ADMIN
    restart: unless-stopped

  wireguard:
    image: linuxserver/wireguard
    container_name: wireguard
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Moscow
      - SERVERURL=188.242.33.79 # внешний IP
      - SERVERPORT=51820
      - PEERS=1
      - PEERDNS=127.0.0.1 # чтобы DNS шел через Pi-hole
      - INTERNAL_SUBNET=10.13.13.0
    volumes:
      - ./wireguard:/config
      - /lib/modules:/lib/modules
    ports:
      - "51820:51820/udp"
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
      - net.ipv4.ip_forward=1
    restart: unless-stopped

```

### Инструкция по пробросу порта 51820/UDP:

1. **Открой веб-интерфейс роутера.**  
2. **В меню слева найди пункт:** `Переадресация NAT` → `Виртуальные серверы`
- **Зайди в раздел "Виртуальные серверы" и "Добавить".**
- **Заполни поля:**
    - **Имя сервиса / Название порта**: `WireGuard`
    - **Протокол**: `UDP`
    - **Внешний порт**: `51820`
    - **Внутренний IP-адрес**: `192.168.1.101` _(IP твоего Raspberry Pi)_
    - **Внутренний порт**: `51820`
- **Сохрани изменения.**
- **Перезагрузи роутер**, если требуется (или просто проверь, чтобы правило появилось в списке активных).

 ### Закрепляем IP за Raspberry Pi через DHCP (рекомендуемый способ)

1. В разделе **Список клиентов** найди устройство с MAC `2C-CF-67-97-C1-F7` (это твой Raspberry Pi).
2. У него сейчас IP `192.168.0.101`
3. Нужно добавить правило статического назначения IP (иногда это называется "Резервирование IP" или "Static DHCP"):
    - Найди на странице или в меню пункт, который позволяет **Добавить MAC и IP для резервирования**.
    - Введи MAC-адрес: `2C-CF-67-97-C1-F7`
    - Введи IP, который хочешь закрепить,`192.168.0.101`.
    - Сохрани настройки.
4. Перезагрузи Raspberry Pi, чтобы он получил новый IP.
***
### Что произойдёт, когда запустишь `docker-compose up -d`:

- Все три контейнера поднимутся и начнут работу в фоне.
    
- **cloudflared** будет проксировать DNS запросы с Pi-hole к 1.1.1.1.
    
- **pihole** будет слушать на 53 портах (TCP и UDP) хоста, значит запросы из локальной сети пойдут через Pi-hole.
    
- **wireguard** будет слушать на 51820 UDP порту, настроен для VPN-подключений, используя DNS Pi-hole.
    
- Если ты настроил на роутере проброс порта 51820 UDP на IP твоего Raspberry Pi (192.168.0.101), VPN клиенты смогут подключаться извне.