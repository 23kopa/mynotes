Путь: `/etc/systemd/system/botmanager.service`
```ini
[Unit]
Description=Gunicorn instance to serve botmanager Flask app
After=network.target

[Service]
User=www-data
Group=www-data
WorkingDirectory=/var/www/botmanager
Environment="FLASK_ENV=production"
EnvironmentFile=/var/www/botmanager/.env
ExecStart=/var/www/botmanager/venv/bin/gunicorn --workers 3 --bind 127.0.0.1:5000 run:app

[Install]
WantedBy=multi-user.target
```