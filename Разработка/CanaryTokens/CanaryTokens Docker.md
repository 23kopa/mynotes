Копирование репозитория:
```sh
https://github.com/thinkst/canarytokens-docker
```

Версии докера:
```sh
Docker Compose version v2.33.1
Docker version 28.0.0, build f9ced58 
```
***
Освободить 53 порт!
***
Настройка IP-адресов
```sh
cp switchboard.env.dist switchboard.env
cp switchboard.env.dist switchboard.env
```

```sh
nano frontend.env
nano switchboard.env
```

```sh
# frontend.env
# Required Settings
CANARY_PUBLIC_IP=83.147.247.89
CANARY_DOMAINS=infotokens.ru
CANARY_NXDOMAINS=nx.infotokens.ru
LOG_FILE=frontend.log
```

```sh
# switchboard.env
# Required Settings
CANARY_PUBLIC_DOMAIN=infotokens.ru
CANARY_WG_PRIVATE_KEY_SEED=B6KiMhyhvPmwbXxJlT5xxbiJKGA62Qox0VE6krSwpFU=
LOG_FILE=switchboard.log
```
***
Сборка проекта:
```sh
docker-compose up -d 
```
***


