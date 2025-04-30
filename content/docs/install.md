---
title: 'Установка'
weight: 1
---

Пакет собирается автоматически из исходного кода. Готовые пакеты находятся в [Releases](https://github.com/itdoginfo/podkop/releases).

Пакет написан полностью на **ash**, поэтому файлы podkop.opkg и luci-app-podkop.opk подходят для всех архитектур.

# Требования
- OpenWrt 23.05 и 24.10
- Минимум 20MB свободного места на NAND. Sing-box идёт как зависимость

# Автоматическая установка
Вставьте эту строку в консоль роутера
```
sh <(wget -O - https://raw.githubusercontent.com/itdoginfo/podkop/refs/heads/main/install.sh)
```

# Удаление
```
opkg remove luci-app-podkop podkop
```