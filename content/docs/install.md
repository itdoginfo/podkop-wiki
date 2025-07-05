---
title: "Установка"
weight: 1
toc: true
---

Пакет собирается автоматически из исходного кода. Готовые пакеты находятся в [релизах](https://github.com/itdoginfo/podkop/releases).

Пакет написан полностью на `ash`, поэтому файлы `podkop_*.ipk` и `luci-app-podkop_*.ipk` подходят для всех архитектур.

Тестировался на **ванильной** OpenWrt 23.05 и OpenWrt 24.10.

На FriendlyWrt 23.05 присутствуют зависимости от `iptables`, которые ломают `tproxy`. Если у вас появляется warning про это в логах, следуйте инструкции по приведённой там ссылке.

Поддержки `apk` на данный момент нет. Он будет сделан после того как разгребу основное.

## Требования

- OpenWrt 23.05 и 24.10. Рекомендуется 24.10 из-за обновляющегося sing-box в этой версии.
- Минимум 20MB свободного места на NAND. `sing-box` идёт как зависимость

## Автоматическая установка и обновление

Вставьте эту строку в консоль роутера

```
sh <(wget -O - https://raw.githubusercontent.com/itdoginfo/podkop/refs/heads/main/install.sh)
```

Если есть уже установленный `podkop`, произойдёт обновление.

## Ручная установка из пакетов

1. Сделать `opkg update`, чтобы установились зависимости.
2. Скачать пакеты `podkop_*.ipk` и `luci-app-podkop_*.ipk` из релиза.
3. Установить пакеты: `opkg install <путь-до-пакета>`. Первым установить `podkop`, потом `luci-app-podkop`.

## Несовместимость

1. Скрипт **Getdomains** несовместим с `podkop`. Его можно удалить скриптом из репозитория [domain-routing-openwrt](https://github.com/itdoginfo/domain-routing-openwrt)

```
sh <(wget -O - https://raw.githubusercontent.com/itdoginfo/domain-routing-openwrt/refs/heads/master/getdomains-uninstall.sh)
```

Оставляет туннели, зоны, forwarding. А также `stubby` и `dnscrypt`. Они не помешают, но вы их можете удалить самостоятельно. Конфиг `sing-box` будет перезаписан в `podkop`.

2. Пакет `https-dns-proxy` тоже перезаписывает конфигурацию `/etc/config/dhcp` и из-за этого возникнет конфликт, приводящий к некорректной работе. Скрипт установки предложит его удалить, если он обнаружит его на роутере. Удаление вручную:

```
opkg remove --force-depends luci-app-https-dns-proxy https-dns-proxy luci-i18n-https-dns-proxy*
```

3. Legacy `iptables` пакеты, а именно `iptables-mod-extra` мешает работе `tproxy` (необходимо ещё раз провести эксперимент для даблчека).

## Удаление

```
opkg remove luci-i18n-podkop-ru luci-app-podkop podkop
```

## Обновление OpenWrt
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