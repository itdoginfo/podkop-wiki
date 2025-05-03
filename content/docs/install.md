---
title: 'Установка'
weight: 1
toc: true
---

Пакет собирается автоматически из исходного кода. Готовые пакеты находятся в [Releases](https://github.com/itdoginfo/podkop/releases).

Пакет написан полностью на **ash**, поэтому файлы podkop.opkg и luci-app-podkop.opk подходят для всех архитектур.

Тестировался на **ванильной** OpenWrt 23.05 и OpenWrt 24.10.
На FriendlyWrt 23.05 присутствуют зависимости от iptables, которые ломают tproxy. Если у вас появляется warning про это в логах, следуйте инструкции по приведённой там ссылке.

Поддержки APK на данный момент нет. APK будет сделан после того как разгребу основное.

# Требования
- OpenWrt 23.05 и 24.10
- Минимум 20MB свободного места на NAND. Sing-box идёт как зависимость

# Автоматическая установка
Вставьте эту строку в консоль роутера
```
sh <(wget -O - https://raw.githubusercontent.com/itdoginfo/podkop/refs/heads/main/install.sh)
```

Скрипт также предложит выбрать, какой туннель будет использоваться. Для выбранного туннеля будут установлены нужные пакеты, а для Wireguard и AmneziaWG также будет предложена автоматическая настройка - прямо в консоли скрипт запросит данные конфига. Для AmneziaWG можно также выбрать вариант с использованием конфига обычного Wireguard и автоматической обфускацией до AmneziaWG.

Для AmneziaWG скрипт проверяет наличие пакетов под вашу платформу в [стороннем репозитории](https://github.com/Slava-Shchipunov/awg-openwrt/releases), так как в официальном репозитории OpenWRT они отсутствуют, и автоматически их устанавливает.

# Ручная установка из пакетов
Сделать `opkg update`, чтобы установились зависимости.
Скачать пакеты `podkop_*.ipk` и `luci-app-podkop_*.ipk` из релиза. `opkg install` сначала первый, потом второй.

# Обновление
Та же самая команда, что для установки. Но с флагом **upgrade** сразу перейдёт к обновлению.
```
sh <(wget -qO- https://raw.githubusercontent.com/itdoginfo/podkop/refs/heads/main/install.sh) --upgrade
```

Если просто запустить скрипт установки, он найдёт уже установленный podkop и предложит обновиться.

# Несовместимость
1. Скрипт **Getdomains** несовместим с podkop. Его можно удалить скриптом из репозитория `itdoginfo/domain-routing-openwrt`
```
sh <(wget -O - https://raw.githubusercontent.com/itdoginfo/domain-routing-openwrt/refs/heads/master/getdomains-uninstall.sh)
```

Оставляет туннели, зоны, forwarding. А также stubby и dnscrypt. Они не помешают, но вы их можете удалить самостоятельно. Конфиг sing-box будет перезаписан в podkop.

2. **https-dns-proxy** пакет тоже перезаписывает конфигурацию `/etc/config/dhcp` и из-за этого возникнет конфликт, приводящий к некорректной работе. Скрипт установки предложит его удалить, если он обнаружит его на роутере. Удаление вручную:
```
opkg remove --force-depends luci-app-https-dns-proxy https-dns-proxy luci-i18n-https-dns-proxy*
```

3. Legacy iptables пакеты, а именно **iptables-mod-extra** мешает работе tproxy (необходимо ещё раз провести эксперимент для даблчека).

# Удаление
```
opkg remove luci-i18n-podkop-ru luci-app-podkop podkop
```