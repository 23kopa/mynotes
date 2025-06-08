```shell
netsh advfirewall firewall add rule name="Allow ICMP" protocol=icmpv4 dir=in action=allow
```

- **Основные компоненты системы:**
    
     `libc-l10n`, `locales`, `debconf-i18n` — для поддержки локализации и языковых настроек.
    - `libc6`, `libc-l10n` — для базовых библиотек системы.
    - `apt-transport-https`, `curl`, `wget` — для скачивания и установки обновлений через HTTPS.
    - `lsb-base`, `gnupg-l10n`, `gnome-keyring` — для работы с ключами и хранилищами ключей.
- **Работа с сетью и безопасностью:**
    
    - `openvpn`, `ufw`, `iptables` — для настройки VPN и брандмауэра.
    - `ssh`, `rsh-client`, `curl`, `wireless-tools` — для работы с удаленными подключениями и управления беспроводными сетями.
    - `samba-dsdb-modules`, `samba-common` — для сетевых файловых систем и взаимодействия с Windows-сетями.
- **Интерфейсы и графическая оболочка:**
    
    - `kwin-style-breeze`, `kde-spectacle`, `ksysguard`, `kde-style-breeze` — для настройки интерфейса и управления системой.
    - `xorg-docs-core`, `xterm`, `xserver-xorg` — для работы с X-сервером и терминалами.
    - `gnome-keyring`, `gnome-icon-theme`, `gtk2-engines` — для работы с графическим окружением и темами.
- **Системные утилиты и утилиты для диагностики:**
    
    - `htop`, `ksysguardd`, `powertop` — для мониторинга и оптимизации производительности.
    - `mc`, `vim`, `nano` — для работы в командной строке.
    - `lsb-release`, `sysstat` — для управления системой и получения информации о версиях.
- **Поддержка приложений и мультимедиа:**
    
    - `vlc`, `gimp`, `blender`, `inkscape`, `chromium`, `firefox` — для работы с мультимедийными файлами и браузерами.
    - `pavucontrol-qt`, `pulseaudio` — для управления звуковыми настройками.
    - `cups-bsd`, `cups-common`, `cups-client` — для работы с принтерами.
- **Дополнительные утилиты и утилиты для разработки:**
    
    - `python3-psutil`, `python3-setuptools`, `python3-distutils` — для поддержки Python.
    - `git`, `make`, `gcc` — для компиляции и разработки программ.
    - `libssl-dev`, `libcurl4-openssl-dev` — для работы с безопасными соединениями и сетевыми приложениями. [^1]


