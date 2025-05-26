```nginx
server {
    listen 443 ssl;
    server_name botmanager.site www.botmanager.site;

    ssl_certificate /etc/letsencrypt/live/botmanager.site/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/botmanager.site/privkey.pem;  
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    root /var/www/botmanager;
    
    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

server {
    listen 80;
    server_name botmanager.site www.botmanager.site;

    return 301 https://$host$request_uri;
}
```

Cache:
```nginx
server {
    listen 443 ssl;
    server_name botmanager.site www.botmanager.site;

    ssl_certificate /etc/letsencrypt/live/botmanager.site/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/botmanager.site/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    root /var/www/botmanager/static;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /var/www/botmanager/app/static/;
        expires 30d;
        add_header Cache-Control "public, max-age=2592000";
    }
}

server {
    listen 80;
    server_name botmanager.site www.botmanager.site;

    return 301 https://$host$request_uri;
}
```

без кеша
```nginx
server {
    listen 443 ssl;
    server_name botmanager.site www.botmanager.site;

    ssl_certificate /etc/letsencrypt/live/botmanager.site/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/botmanager.site/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    root /var/www/botmanager/static;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /var/www/botmanager/app/static/;
        expires off;
        add_header Cache-Control "no-store, no-cache, must-revalidate, proxy-revalidate, max-age=0";
    }
}

server {
    listen 80;
    server_name botmanager.site www.botmanager.site;

    return 301 https://$host$request_uri;
}
```