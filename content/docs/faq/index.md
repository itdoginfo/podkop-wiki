---
title: 'Частые вопросы'
weight: 0
toc: true
---

### Как удалить подкоп

Вставьте эту строку в консоль роутера

```
opkg remove luci-i18n-podkop-ru luci-app-podkop podkop
```

### Как обновить openwrt если установлен подкоп
Перед обновление OpenWrt необходимо остановить podkop либо из LuCI, либо командой
```
service podkop stop
```

Если вы уже обновили ОС без стопа и столкнулись с отсуствующим DNS, то
```
service podkop stop

uci -q delete dhcp.@dnsmasq[0].server
uci add_list dhcp.@dnsmasq[0].server="8.8.8.8"
uci commit dhcp
service dnsmasq restart
ntpd -q -p ptbtime1.ptb.de

service podkop start
```

### Как вручную установить подкоп из пакетов

1. Сделать `opkg update`, чтобы установились зависимости.
2. Скачать пакеты `podkop_*.ipk` и `luci-app-podkop_*.ipk` из релиза.
3. Установить пакеты: `opkg install <путь-до-пакета>`. Первым установить `podkop`, потом `luci-app-podkop`.
