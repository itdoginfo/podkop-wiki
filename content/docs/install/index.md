---
title: "Установка"
weight: 1
toc: true
---

Пакет собирается автоматически из исходного кода. Готовые пакеты находятся в [релизах](https://github.com/itdoginfo/podkop/releases).

Пакет написан полностью на `ash`, поэтому файлы `podkop_*.ipk` и `luci-app-podkop_*.ipk` подходят для всех архитектур.

Тестировался на **ванильной** OpenWrt 24.10.

## Требования

- OpenWrt 24.10.
- Минимум 20MB свободного места на NAND. `sing-box` идёт как зависимость

## Автоматическая установка и обновление

Вставьте эту строку в консоль роутера

```
sh <(wget -O - https://raw.githubusercontent.com/itdoginfo/podkop/refs/heads/main/install.sh)
```

Если есть уже установленный `podkop`, произойдёт обновление.

## Установка, если GitHub заблокирован

Если с роутера недоступны `github.com`, `raw.githubusercontent.com` или `downloads.openwrt.org`, поставить podkop можно через зеркало:

```
wget -O - https://mirror.podkop.net/urjw/scripts/bootstrap.sh | sh
```

Скрипт сам определяет версию и архитектуру OpenWrt, скачивает собранный под неё набор пакетов вместе со всеми зависимостями и ставит его локально. Каждый файл проверяется по sha256. Уже установленные зависимости пропускаются, так что система целиком не обновляется.

Файлы репозиториев (`/etc/opkg/distfeeds.conf`, `/etc/apk/repositories.d/`) скрипт не меняет, работает и с `opkg`, и с `apk`, а после установки удаляет за собой временные файлы из `/tmp`. Как и обычный установщик, он снесёт конфликтующий `https-dns-proxy`, если найдёт его.

> [!NOTE]
> Нужно не меньше 15 МБ свободного места в `/overlay`. Если места меньше, скрипт остановится и скажет об этом.

Если готового набора под вашу версию и архитектуру на зеркале не окажется, скрипт выведет запасной вариант — тот же официальный `install.sh`, скачанный через зеркало.

Когда `wget` ругается на сертификат — обычное дело на свежей прошивке, где ещё не выставлено время, — добавьте `--no-check-certificate`:

```
wget --no-check-certificate -O - https://mirror.podkop.net/urjw/scripts/bootstrap.sh | sh
```

## Ручная установка из пакетов

1. Сделать `opkg update`, чтобы установились зависимости.
2. Скачать пакеты `podkop-*.ipk` и `luci-app-podkop-*.ipk` из релиза.
3. Установить пакеты: `opkg install <путь-до-пакета>`. Первым установить `podkop`, потом `luci-app-podkop`.
4. Если нужен русский язык в LuCi, то дополнительно скачайте и установите `luci-i18n-podkop-ru-*.ipk`.

## Установка на 23.05
Мы не рекомендуем использовать 23.05, если вам нужен podkop. Версия 0.5.0 и выше требует sing-box 1.12 и jq 1.7.1.

Но если всё-таки надо, то есть два варианта:
- Установить вручную релиз [0.4.11](https://github.com/itdoginfo/podkop/releases/tag/v0.4.11). Последний релиз, который работает с 1.11 и старым jq.
- Установить вручную sing-box, jq из пакетов для 24.10. И вручную установить podkop из [релизов](https://github.com/itdoginfo/podkop/releases).

## Установка на 25.12
Вы можете установить podkop используя автоматический скрипт установки или загрузить необходимые файлы в формате **.apk** из [релизов](https://github.com/itdoginfo/podkop/releases) и выполнить установку вручную. 

В версии 25.12 по умолчанию запрещена установка пакетов без проверки подписи(untrusted signature). Это повышает безопасность системы, но так же, по умолчанию, не позволяет устанавливать пакеты с помощью вэб-интерфейса LuCi.

Как выполнить ручную установку:

1. Загрузите пакеты `podkop-*.apk` и `luci-app-podkop-*.apk`. Если нужен русский язык в LuCi, то дополнительно скачайте  `luci-i18n-podkop-ru-*.apk`.
2. Передайте файлы на роутер.
3. Подключитесь к роутеру по SSH и выполните установку загруженных пакетов:
```
apk update
apk add --allow-untrusted <путь-до-пакета>
```

> [!TIP]
> Чтобы добавить в LuCi возможность устанавливать пакеты без доверенной подписи вы можете подключиться к роутеру по ssh, открыть в редакторе файл `/usr/libexec/package-manager-call`
> и в строке 38 заменить `action="add"` на `action="add --allow-untrusted"`. После этого вы сможете загружать пакеты перейдя в **LuCi -> Система -> Пакеты -> Загрузить пакет**.
> Эта настройка будет слетать каждый раз при обновлении OpenWrt.
>
> **Используйте с осторожностью, т.к это нарушает безопасность системы.**

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
