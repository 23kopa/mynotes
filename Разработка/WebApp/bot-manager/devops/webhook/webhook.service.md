Путь: `/etc/systemd/system/webhook.service`
```ini
[Unit]
Description=GitHub Webhook Listener
After=network.target

[Service]
User=root
WorkingDirectory=/opt/webhook/github
ExecStart=/opt/webhook/github/deploy_venv/bin/python /opt/webhook/github/webhook_listener.py
Restart=always

[Install]
WantedBy=multi-user.target
```

Запуск:
```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable webhook.service
sudo systemctl start webhook.service
sudo systemctl status webhook.service
```