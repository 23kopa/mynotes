Конфигурация `/etc/nginx/sites-available/mykopa.ru`:
```nginx
server {
    server_name mykopa.ru www.mykopa.ru;

    root /root/TG-Bots/my_bot-web/web_app/mykopa;
    index index.html;

    location / {
        proxy_pass http://127.0.0.1:8443;  # Перенаправление на 8443
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/mykopa.ru/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/mykopa.ru/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot


}
server {
    if ($host = www.mykopa.ru) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = mykopa.ru) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    listen 80;
    server_name mykopa.ru www.mykopa.ru;
    return 404; # managed by Certbot




}
```

```nginx
server {
    server_name botmanager.site www.botmanager.site;

    root /root/WebApp/botmanager/static/;
    index index.html;

    location / {
        proxy_pass http://127.0.0.1:5000;  # Перенаправление на 8443
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/botmanager.site/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/botmanager.site/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot


}
server {
    if ($host = www.botmanager.site) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = botmanager.site) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    listen 80;
    server_name botmanager.site www.botmanager.site;
    return 404; # managed by Certbot




}
```

Создайте символическую ссылку:
```bash
sudo ln -s /etc/nginx/sites-available/mykopa.ru /etc/nginx/sites-enabled/
```

Для локального включения в директории приложения `/TG-Bots/my_bot-web/web_app/mykopa`:
```python
python3 -m http.server 8080
```
***
Получение сертификата SSL:
```sh
sudo apt install certbot python3-certbot-nginx
```

```sh
sudo certbot --nginx -d mykopa.ru -d www.mykopa.ru
```

```sh
sudo certbot --nginx -d botmanager.site -d www.botmanager.site
```

***
Изменения в main.py:
```python
import ssl
from aiohttp import web

# Обработчик для Web App
async def web_app_handler(request):
    try:
        return web.FileResponse('web_app/mykopa/index.html') # Путь к файлам
    except Exception as e:
        logging.error(f"Ошибка при обработке запроса: {e}")
        return web.Response(text="Произошла ошибка при загрузке приложения.", status=500)


# Настройка веб-сервера
async def setup_web_server():
    app = web.Application()
    app.router.add_get('/', web_app_handler)
    app.router.add_static('/static/', path='web_app', name='static')
    runner = web.AppRunner(app)
    await runner.setup()
    
    # Указываем SSL-сертификаты
    ssl_context = ssl.create_default_context(ssl.Purpose.CLIENT_AUTH)
    ssl_context.load_cert_chain(certfile='/etc/letsencrypt/live/mykopa.ru/fullchain.pem',
                                keyfile='/etc/letsencrypt/live/mykopa.ru/privkey.pem')

    # Запуск сервера на порту 8443
    site = web.TCPSite(runner, '0.0.0.0', 8443, ssl_context=ssl_context)
    await site.start()

async def main():
    # Запуск web-сервера
    await setup_web_server()
```
***
Для запуска в фоновом режиме `nohup`

Для горячей откладки:

```
pip install watchdog
```

```python
import os
import time
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler
import subprocess

class MyHandler(FileSystemEventHandler):
    def on_modified(self, event):
        if event.src_path.endswith("index.html") or event.src_path.endswith(".css") or event.src_path.endswith(".js"):
            print(f"File {event.src_path} changed. Restarting server...")
            subprocess.run(["pkill", "-f", "http.server"])  # Завершаем старый процесс
            time.sleep(1)  # Даем время на завершение процесса
            subprocess.Popen(["python3", "-m", "http.server", "8080"])  # Запускаем новый сервер

if __name__ == "__main__":
    path = os.path.abspath("mykopa")  # Путь до твоей директории
    event_handler = MyHandler()
    observer = Observer()
    observer.schedule(event_handler, path, recursive=True)
    observer.start()
    
    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()
    observer.join()

```

Создай файл сервиса:
```bash
sudo nano /etc/systemd/system/python-http-server.service
```

Добавь следующее содержимое:

```sh
[Unit]
Description=Simple Python HTTP Server
After=network.target

[Service]
ExecStart=/usr/bin/python3 -m http.server 8080
WorkingDirectory=/root/TG-Bots/my_bot-web/web_app/mykopa
Restart=always
User=root
Group=root
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target

```

Перезагрузи systemd, чтобы применить изменения:
```bash
sudo systemctl daemon-reload
sudo systemctl start python-http-server.service
sudo systemctl enable python-http-server.service
```
***
