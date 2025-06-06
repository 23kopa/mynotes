Открой PowerShell или CMD **от имени администратора** и введи:
```powershell
netsh winsock reset
netsh int ip reset
ipconfig /release
ipconfig /renew
ipconfig /flushdns
```