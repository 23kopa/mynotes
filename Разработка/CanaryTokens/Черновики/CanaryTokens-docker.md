```sh
git clone https://github.com/thinkst/canarytokens-docker
```

```sh
cp switchboard.env.dist switchboard.env
cp switchboard.env.dist switchboard.env
```

```sh
nano frontend.env
nano switchboard.env
```

***
```sh
sudo apt install nginx certbot python3-certbot-nginx -y
```

```sh
sudo nano /etc/nginx/sites-available/infotokens
```

```sh
server {
    listen 80;
    server_name infotokens.ru;
    
    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

```sh
sudo ln -s /etc/nginx/sites-available/infotokens /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```
***
```sh
cd nginx
nano nginx.conf
```

```sh
sudo apt update && sudo apt install -y redis-server mongodb python3 python3-venv python3-pip git
```

```sh
sudo systemctl start redis
sudo systemctl enable redis
sudo systemctl start mongodb
sudo systemctl enable mongodb
```

```sh
poetry --version
python3 --version
pip3 --version
```

```sh
cd /home/user23/CanaryTokens/canarytokens
poetry install
```
***
```sh
minify-html = "^0.9.2" # Проблемный пакет старой версии
```

#### Изменение конфигурации
```sh
nano pyproject.toml 
# minify-html = "^0.15.0"
```

```sh
rm poetry.lock
```

```sh
poetry lock
```

#### Установка определенной версии

```sh
pip3 install minify-html==0.9.2
```

***
```sh
nano __init__.py
```

```sh
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, CanaryTokens!"
```

```
nano .env
```

```sh
export FLASK_APP=canarytokens
export FLASK_ENV=development
export SECRET_KEY='your_secret_key_here'
export REDIS_URI=redis://localhost:6379/0
export MONGO_URI=mongodb://localhost:27017/canarytokens
```

```
flask run --host=0.0.0.0 --port=5000
```