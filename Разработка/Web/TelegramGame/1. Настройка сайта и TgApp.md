> https://chatgpt.com/share/67b04f4d-bfec-8006-a297-4ea5a80e55a1
```sh
sudo apt update && sudo apt upgrade -y
sudo apt install nginx certbot python3-certbot-nginx git curl -y
```
***
```sh
sudo nano /etc/nginx/sites-available/kopa23
```

```nginx
server {
    listen 80;
    server_name kopa23.site www.kopa23.site;

    root /var/www/kopa23;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
***
```sh
sudo ln -s /etc/nginx/sites-available/kopa23 /etc/nginx/sites-enabled/
sudo nginx -t  # Проверка конфигурации
sudo systemctl restart nginx
```
***
```
Добавление А записей kopa23.site www.kopa23.site
на сайте домена
```

```
sudo certbot --nginx -d kopa23.site -d www.kopa23.site
```

> Теперь сайт доступен по `https://kopa23.site/`.
***

```sh
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
node -v  # Проверить версию
npm -v   # Проверить версию
```

```sh
mkdir -p /var/www/kopa23 && cd /var/www/kopa23
npm init -y
```

```sh
npm install react react-dom vite npm install @telegram-web-app/react --save
```

```sh
npm install @vitejs/plugin-react
```
***
```sh
nano /var/www/kopa23/index.html
```

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Telegram Web App</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
</head>
<body>
    <div id="root"></div>
    <script type="module" src="/main.js"></script>
</body>
</html>
```
***
```sh
nano /var/www/kopa23/main.jsx
```

```js
import React from "react";
import ReactDOM from "react-dom";

const App = () => {
    const tg = window.Telegram.WebApp;
    tg.expand();

    return (
        <div style={{ padding: "20px", textAlign: "center" }}>
            <h1>Привет, {tg.initDataUnsafe?.user?.first_name || "пользователь"}!</h1>
            <button onClick={() => tg.close()}>Закрыть</button>
        </div>
    );
};

ReactDOM.render(<App />, document.getElementById("root"));
```
***
```sh
nano package.json
```

```json
{
  "name": "kopa23",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "vite"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "dependencies": {
    "@telegram-web-app/react": "^0.0.1-beta.1",
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "vite": "^6.1.0"
  }
}
```
***
```sh
nano vite.config.js
```

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
})
```
***
```sh
npm run dev
```
***
Включение для других пользователей в фоновом режиме:
```sh
 nohup npm run dev -- --host
```

```sh
ps aux | grep "npm run dev"
```