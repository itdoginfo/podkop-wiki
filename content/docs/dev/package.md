---
title: 'Разработка пакета'
weight: 1
toc: true
---

## Разработка пакета

Есть два варианта:
- Просто поставить пакет на роутер или виртуалку и прямо редактировать через SFTP (opkg install openssh-sftp-server)
- SDK, чтобы собирать пакеты

Для сборки пакетов нужен SDK, один из вариантов скачать прям файл и разархивировать
https://downloads.openwrt.org/releases/24.10.2/targets/x86/64/
Нужен файл в имени которого есть SDK

```
wget https://downloads.openwrt.org/releases/24.10.2/targets/x86/64/openwrt-sdk-24.10.2-x86-64_gcc-13.3.0_musl.Linux-x86_64.tar.zst
tar xf openwrt-sdk-23.05.5-x86-64_gcc-12.3.0_musl.Linux-x86_64.tar.xz
mv openwrt-sdk-23.05.5-x86-64_gcc-12.3.0_musl.Linux-x86_64 SDK
```
Последнее делается для удобства.

Создаём директорию для пакета
```
mkdir package/utilites
```

Симлинк из репозитория
```
ln -s ~/podkop/podkop package/utilites/podkop
ln -s ~/podkop/luci-app-podkop package/luci-app-podkop
```

В первый раз для сборки luci-app необходимо обновить и установить пакеты
```
./scripts/feeds update -a
./scripts/feeds install -a
```

Для make можно добавить флаг -j N, где N - количество ядер для сборки.

При первом make выводится меню, можно просто save, exit и всё. Первый раз долго грузит зависимости.

Сборка пакета. Сами пакеты собираются быстро.
```
make package/podkop/{clean,compile} V=s
```

Также для LuCi
```
make package/luci-app-podkop/{clean,compile} V=s
```

Оба сразу
```
make package/luci-app-podkop/{clean,compile} package/podkop/{clean,compile} V=s
```

.ipk находятся в `bin/packages/x86_64/base/`

### Ошибки
```
Makefile:17: /SDK/feeds/luci/luci.mk: No such file or directory
make[2]: *** No rule to make target '/SDK/feeds/luci/luci.mk'.  Stop.
time: package/luci/luci-app-podkop/clean#0.00#0.00#0.00
    ERROR: package/luci/luci-app-podkop failed to build.
make[1]: *** [package/Makefile:129: package/luci/luci-app-podkop/clean] Error 1
make[1]: Leaving directory '/SDK'
make: *** [/SDK/include/toplevel.mk:226: package/luci-app-podkop/clean] Error 2
```

Не загружены пакеты для LuCi

### Зависимости make 
https://openwrt.org/docs/guide-developer/toolchain/install-buildsystem

Ubuntu
```
sudo apt update
sudo apt install build-essential clang flex bison g++ gawk \
gcc-multilib g++-multilib gettext git libncurses-dev libssl-dev \
python3-distutils rsync unzip zlib1g-dev file wget
```

### Примеры ссылок-конфигураций
https://github.com/itdoginfo/podkop/blob/main/String-example.md

## Интернационализация (i18n)

Интернационализация — это подготовка пакета к переводу на разные языки. 

На этом этапе помечаются строки кода, подлежащие локализации с помощью [`gettext`](https://www.gnu.org/software/gettext/),
и создаются шаблоны переводов `.pot (Portable Object Template)`, 
которые затем используются для создания конкретных файлов перевода `.po`.

В LuCI функция `gettext` имеет алиас `_`, который чаще используется в коде для краткости, например:
```javascript
let o = s.tab('basic', _('Basic Settings'));
```

### Этапы создания или обновления шаблона перевода
1. Перейдите в поддиректорию `luci-app-podkop` проекта `podkop`.

```bash
cd luci-app-podkop
```

2. Запустите скрипт генерации шаблона переводов:
```bash
./xgettext.sh 
```
Скрипт соберёт все строки, помеченные с помощью `_()`, и создаст обновлённый шаблон перевода `podkop.pot`.
Шаблон перевода `podkop.pot` будет находиться в соответствующей папке шаблонов, обычно `po/templates/podkop.pot`

После этого `.pot` можно использовать для создания или обновления файлов перевода `.po` для нужных локалей.

## Локализация (l10n)

Для локализации интерфейса podkop на выбранный язык используется дополнительный языковой пакет, например `luci-i18n-podkop-ru`.

Перед сборкой пакета следует обновить или создать файл перевода `.po (Portable Object)` для соответствующей локали.

### Этапы создания или обновления перевода

1. Перейдите в поддиректорию `luci-app-podkop` проекта `podkop`.

```bash
cd luci-app-podkop
```

2. Запуск скрипта обновления перевода

Для создания или обновления `.po` файла используйте скрипт `msgmerge.sh` с указанием кода локали, например:
```bash
./msgmerge.sh ru
```
* Скрипт автоматически создаст новый `.po` файл, если его ещё нет, или объединит существующий файл перевода с 
обновлённым шаблоном `.pot`
* `.po` файлы будут находиться в соответствующей папке локали, обычно `po/<код_локали>/podkop.po`

3. Редактирование перевода

После выполнения скрипта откройте `.po` файл и переведите строки, помеченные как `msgid`, например:
```text
msgid "Settings"
msgstr "Настройки"
```

4. Проверка

Проверьте корректность `.po` файла:
```bash
msgfmt po/ru/podkop.po -o /dev/null --check
```
После этого необходимо зафиксировать изменения шаблона `.pot` и файлов перевода `.po`, после чего отправить их в репозиторий.