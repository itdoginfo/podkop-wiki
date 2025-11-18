---
title: 'Серверные рецепты'
weight: 61
toc: true
---

Данные рецепты подходят для **Xray**.

## Как использовать только IPv4?

Добавляем в настройки **outbounds** следующее:
```json
{
    "outbounds": [
    {
        "protocol": "freedom",
        "settings": {
            "domainStrategy": "UseIPv4"
        },
        "tag": "direct"
    }
    ]
}
```

## Как использовать IPv6 только для Youtube?

В случае, если в результате [проверки](/docs/faq/#kak-servisy-opredelyayut-ip-servera) вы решили использовать для **Youtube** IPv6, а для всего остального у вас используется IPv4, внесите следующие изменения в ваш конфиг:

в секции **outbounds**:
```json
{
    "outbounds": [
        {
            "protocol": "freedom",
            "tag": "direct_v6",
            "settings": {
                "domainStrategy": "UseIPv6"
            }
        }
    ]
}
```
в секции **routing** :
```json
{
    "routing": {
        "rules": [
            {
            "type": "field",
            "domain": [
                "geosite:google",
                "geosite:youtube"
            ],
            "outboundTag": "direct_v6"
            }
        ]
    }
}
```


## Как использовать ipv4 только для Gemini?

В случае, если в результате [проверки](/docs/faq/#kak-servisy-opredelyayut-ip-servera) вы решили использовать для **Gemini** IPv4, а для всего остального у вас используется IPv6, внесите следующие изменения в ваш конфиг:

в секции **outbounds**:

```json
{
    "outbounds": [
        {
            "protocol": "freedom",
            "tag": "direct_v4",
            "settings": {
                "domainStrategy": "UseIPv4"
            }
        }
    ]
}
```

в секции **routing** :

```json
{
    "routing": {
        "rules": [
            {
            "type": "field",
            "domain": [
                "geosite:google-deepmind",
            ],
            "outboundTag": "direct_v4"
            }
        ]
    }
}
```

## Как направить определенный сервис в Warp?

Установите на сервере `warp-cli`. 
Для Debian это выполняется следующим образом:
```shell
# add warp gpg key
curl -fsSL https://pkg.cloudflareclient.com/pubkey.gpg | gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg

# add warp repo
echo "deb [signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/$(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflare-client.list

# install
apt update
apt install cloudflare-warp
```

Затем:

```shell
# register warp
echo "y" | warp-cli registration new
# mode proxy
warp-cli mode proxy
warp-cli proxy port $WARP_PORT
# connect
warp-cli connect
```

где **$WARP_PORT** порт, который вы будете использовать для warp-прокси.

В конфиге укажите в разделе **outbounds**:
```json
{
    "outbounds": [
        {
            "tag": "warp",
            "protocol": "socks",
            "settings": {
                "servers": [
                    {"
                        address": "127.0.0.1",
                        "port": $WARP_PORT
                    }
                ]
            }
        }
    ]
}
```

в разделе **routing**:

```json
{
    "routing": {
        "rules": [
            {
                "outboundTag": "warp",
                "domain": [
                    "geosite: google-deepmind" 
                ]
            }
        ]
    }
}
```

В примере используется Gemini, но вы можете указать необходимые вам домены и сервисы. С полным списком доступных сервисов вы можете ознакомиться [здесь](https://github.com/v2fly/domain-list-community).
