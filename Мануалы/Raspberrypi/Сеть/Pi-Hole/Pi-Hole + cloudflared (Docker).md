
Структура проекта:
```
pihole-dns-stack/
├── docker-compose.yml
└── etc-pihole/         # автоматически создается
└── etc-dnsmasq.d/      # автоматически создается
```

`docker-compose.yml`
```yaml
version: "3.8"

services:
  cloudflared:
    image: cloudflare/cloudflared:latest
    command: proxy-dns --port 5053 --upstream https://1.1.1.1/dns-query
    restart: unless-stopped

  pihole:
    image: pihole/pihole:latest
    environment:
      TZ: 'Europe/Moscow'
      WEBPASSWORD: 'W35eu1d1'   # можешь заменить на свой
      DNS1: 127.0.0.1#5053
      DNS2: 127.0.0.1#5053
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80"           # web interface
    volumes:
      - ./etc-pihole/:/etc/pihole/
      - ./etc-dnsmasq.d/:/etc/dnsmasq.d/
    cap_add:
      - NET_ADMIN
    restart: unless-stopped

```

```bash
docker compose up -d
```

> От root!

Остановка
```
docker compose down
```

```
alias dcup='docker-compose up -d'
alias dcdown='docker-compose down'
```