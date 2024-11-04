---
title: 'Установка'
weight: 1
---

Пакет собирается автоматически из исходного кода. Готовые пакеты находятся в [Releases](https://github.com/itdoginfo/podkop/releases).

Пакет написан полностью на **ash**, поэтому файлы podkop.opkg и luci-app-podkop.opk подходят для всех архитектур.

# Требования
- OpenWrt 23.05
- Dnsmasq-full
- Sing-box, если вы хотите использовать VLESS или Shadowsocks

# Автоматическая установка
Вставьте эту строку в консоль роутера
```
sh <(wget -O - https://raw.githubusercontent.com/itdoginfo/podkop/refs/heads/main/install.sh)
```

Далее нужно выбрать какой туннель вы будете использовать. Если у вас уже настроен туннель, то выбираете **Skip**.

Шифрование DNS на текущий момент времени нужно настраивать самостоятельно. Используйте пакеты https-dns-proxy и stubby.

# Удаление
```
opkg remove luci-app-podkop podkop
```