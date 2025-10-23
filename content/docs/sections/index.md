---
title: 'Несколько маршрутов (секции)'
weight: 6
toc: true
---

В podkop можно направлять разный трафик в разные outbound (VLESS, WG, AWG, etc). Например список Geoblock вы можете послать в ваш VLESS-сервер, расположенный в Европе, Youtube к WG-серверу в РФ, а инструменты Google AI к Shadowsocks в США.

В OpenWrt LuCi есть понятие секций. Секция - это расширяемая часть конфигов openwrt. Пример упрощенного основного конфига main и экстра-секции **wg**:
```
config main 'main'
    option proxy_string 'string'
    option community_lists_enabled '1'
    list community_lists 'geoblock'

config extra 'wg'
    option mode 'vpn'
    option interface 'wg0'
    option community_lists_enabled '1'
    list community_lists 'youtube'
```

Экстра-секций может быть любое количество. Вы можете перенаправлять нужный вам трафик, даже если этого нет в предустановленных списках.

# Список доменов в Podkop

Список Russia inside включает в себя следующие подсписки:

- GEO Block
- Block
- Porn
- News
- Anime
- Youtube
- Meta*
- Twitter(X)
- HDRezka
- Tik-Tok

Подсписки Meta*, Twitter, Cloudflare и Discord содержат так же подсети указанных сервисов, которые по-умолчанию отсутствуют в Russia inside.
Подсети необходимы если вам нужны рабочие голосовые чаты в Discord или какой-то из вышеназванных трех сервисов работает плохо, что-то не загружается или тормозит.

Таким образом, при использовании Russia inside вы можете выбрать дополнительно следующие списки:
- Meta*
- Twitter
- Discord
- Telegram
- Cloudflare
- Google AI
- Google Play
- H.O.D.C.A. (Список доменов за CF, AWS итд)
- Hetzner ASN
- OVH ASN
- Digital Ocean ASN
- CloudFront ASN

Вместе нельзя выбрать и использовать:
- Russia inside
- Russia outside
- Ukraine

Что делать если вам нужно добавить отдельный туннель для сервиса  который присутствует в Russia inside?

Вам необходимо отказаться от использования списка Russia inside и вместо него использовать вышеуказанные подсписки, собрав то, что вам необходимо и исключив сервис, который вам нужно пустить в отдельный туннель.

Если вы хотите более подробно ознакомиться с тем, какие сайты включены в тот или иной список, вы можете сделать это здесь:
https://github.com/itdoginfo/allow-domains/

# Как это настраивается в LuCi

![section](images/luci-section-example.png)

---

\* Meta Platforms Inc. — организация, признанная экстремистской и запрещённая на территории РФ.