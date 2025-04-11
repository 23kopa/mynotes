
`rustup` — инструмент для управления версиями Rust и связанными инструментами.

```bash
rustup show #Текущая версия
```

```bash
#Результат
Default host: x86_64-unknown-linux-gnu
rustup home:  /root/.rustup

installed toolchains
--------------------

stable-x86_64-unknown-linux-gnu (default)
nightly-x86_64-unknown-linux-gnu
solana

active toolchain
----------------

stable-x86_64-unknown-linux-gnu (directory override for '/home/user23/sol-transfer')
rustc 1.84.1 (e71f9a9a9 2025-01-27)
```
***
Для глобальных переключений между stable и nightly:
```bash
rustup default stable   # или
rustup default nightly
```
Для локальных переключений для конкретных проектов :
```bash
rustup override set stable   # или
rustup override set nightly
```

```bash
rustc --print=target-list  # Показывает все доступные таргеты компиляции

rustup target add wasm32-unknown-unknown  # Добавляет поддержку WebAssembly
```