---
title: 'Режимы работы'
weight: 2
---

Есть два режима работы: Proxy и VPN.

# VPN
В этом режиме вы выбираете интерфейс настроенный ранее. Это может быть Wireguard, AmneziaWG, OpenVPN, OpencConnect и любой другой туннель или VPN.

Обязательно должна быть создана Zone и настроен Forwarding. Иначе трафик ходить не будет. Это подробно описано в "Туннели"

# Proxy
В этом режиме сейчас поддерживаются две технологии: VLESS и Shadowsocks.

Этот режим использует tproxy и sing-box. Вам необходимо просто скопировать строку в **Proxy String** и всё настроится автоматически.

Не все строки сейчас могут быть распознаны. Чтоб реализовать их поддержку, кидайте в чат примеры строк без чувствительных данных.