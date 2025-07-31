---
title: 'Свой Outbound'
weight: 25
toc: true
---

Если по какой-то причине у вас нет VLESS:// ссылки или podkop пока не поддерживает ваш тип соединения, вы можете самостоятельно сделать outbound для Sing-box и вставить его в podkop.

# Конвертирование ссылок
Один из простых вариантов конвертирования - это использовать приложения на базе Sing-box: Nekobox/Nekoray.

Импортируете туда ссылку и делаете экспорт конфигурации. Копируйте только outbound.

Ещё один вариант конвертирование ссылки онлайн-конвертором https://singboxconverter.amphub.fyi/

Конвертор сделан участником нашего сообщества. Все преобразования делаются на фронтенде, то есть никуда не передаются.

# Amnezia VLESS
Сервер от Amnezia не даёт VLESS ссылок, поэтому при его использовании вам нужно либо самостоятельно сделать ссылку, либо сконфигурить outbound. Пример:
```
{
      "type": "vless",
      "server": "$SERVER",
      "server_port": 443,
      "uuid": "$UUID",
      "flow": "xtls-rprx-vision",
      "tls": {
        "enabled": true,
        "server_name": "$FAKE_SERVER",
        "utls": {
          "enabled": true,
          "fingerprint": "$FINGERPRINT"
        },
        "reality": {
          "enabled": true,
          "public_key": "$PUBLIC_KEY",
          "short_id": "$SID"
        }
      }
}
```

Подставьте свои значения и вставьте этот outbound в podkop.