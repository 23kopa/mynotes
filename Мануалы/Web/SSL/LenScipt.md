```sh
sudo certbot --nginx -d kopa23.site -d www.kopa23.site
```
***
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