
```bash
 sudo netstat -tulpn  #просмотр открытых портов
 watch lsof -i -P -a  #просмотр портов при открытии программы
```

```bash
sudo ufw status #проверка статуса
sudo ufw enable #включение фаервола
sudo ufw allow 22 #разрешить входящий SSH
sudo ufw deny 80 #запретить входящий HTTP
```

sudo iptables -L

apt-get install iptables-persistent