`Pi-hole с DoH upstream` + `прокси-сервер с фильтрацией` + `WireGuard` + DHCP

Общая архитектура:
```
[Клиент] ⇄ Wi-Fi / Ethernet ⇄ [Raspberry Pi Zero W2]
	                                    │
	                                    ├─ DHCP-сервер (раздаёт IP)
	                                    ├─ Pi-hole (DNS с DoH)
	                                    └─ Прокси-сервер (HTTP(S)-фильтрация)
```

Вывод настрйоки Pi-Hole:
```
Configure your devices to use the Pi-hole as their DNS server
using:

IPv4: 192.168.0.105
IPv6: Not Configured
If you have not done so already, the above IP should be set to
static.
View the web interface at http://pi.hole/admin:80 or
http://192.168.0.105:80/admin

Your Admin Webpage login password is SWJjZyYV
```

> Необходимо настроить статический IP-адрес для RPi в веб-приложении роутера