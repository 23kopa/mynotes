DNS-over-HTTPS нужен для того, чтобы сделать разрешение доменных имён в интернете более безопасным и приватным за счёт шифрования DNS-трафика через HTTPS.

Итого, используя DoH с сервером 1.1.1.1, вы получаете:
- Защищённые от прослушивания и подмены DNS-запросы
- Улучшенную приватность и защиту данных
- Быстрый и надёжный доступ к DNS
- Возможность обходить DNS-цензуру и фильтры
### Настройка DoH для Windows 11 (1.1.1.1 Cloudflare)
1. Настройка сетевого адаптера (IPv4)
```
Предпочитетльный DNS-сервер: 1.1.1.1
Альтернативный DNS-сервер: 1.0.0.1
```

***
2. PowerShell-команды для настройки DoH:

```powershell
Set-DnsClientDohServerAddress -ServerAddress 1.1.1.1 -AutoUpgrade $true
Set-DnsClientDohServerAddress -ServerAddress 1.0.0.1 -AutoUpgrade $true
Set-DnsClientDohServerAddress -ServerAddress 2606:4700:4700::1111 -AutoUpgrade $true
Set-DnsClientDohServerAddress -ServerAddress 2606:4700:4700::1001 -AutoUpgrade $true
```

> ! После выполнения настройки необходимо перезапустить сетевой адаптер

***
3. Команда для вывода адаптеров сети:
```powershell
Get-NetAdapter | Format-Table -AutoSize
```
4. Команды для перезапуска адаптера:
```powershell
Disable-NetAdapter -Name "Беспроводная сеть" -Confirm:$false
Start-Sleep -Seconds 3
Enable-NetAdapter -Name "Беспроводная сеть"
```
***
5. Команда проверки получает и выводит текущие настройки DNS-over-HTTPS:
```powershell
Get-DnsClientDohServerAddress
```
Корректный вывод: 

| ServerAddress        | AllowFallbackToUdp | AutoUpgrade | DohTemplate                          |
| -------------------- | ------------------ | ----------- | ------------------------------------ |
| 1.1.1.1              | False              | True        | https://cloudflare-dns.com/dns-query |
| 1.0.0.1              | False              | True        | https://cloudflare-dns.com/dns-query |
| 2606:4700:4700::1001 | False              | True        | https://cloudflare-dns.com/dns-query |
| 2606:4700:4700::1111 | False              | True        | https://cloudflare-dns.com/dns-query |
***
Заблокировать другие DNS-запросы на 53 порт (опционально)
```powershell
New-NetFirewallRule -DisplayName "Block non-DoH DNS" `
  -Direction Outbound `
  -Protocol TCP `
  -RemotePort 53 `
  -Action Block

New-NetFirewallRule -DisplayName "Block non-DoH DNS (UDP)" `
  -Direction Outbound `
  -Protocol UDP `
  -RemotePort 53 `
  -Action Block
```
> Можно через Брандмауэр

Попробуй выполнить команду для теста DNS-запроса на порт 53 напрямую, например с помощью `Test-NetConnection`:
```powershell
Test-NetConnection -ComputerName 8.8.8.8 -Port 53 -InformationLevel Detailed
```

Если правило работает, то соединение на порт 53 должно быть **заблокировано** и ты увидишь что-то вроде:
```powershell
ПРЕДУПРЕЖДЕНИЕ: TCP connect to (8.8.8.8 : 53) failed
```