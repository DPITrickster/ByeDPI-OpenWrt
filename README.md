# ByeDPI для OpenWrt

[![Build Status](https://img.shields.io/github/actions/workflow/status/DPITrickster/ByeDPI-OpenWrt/build.yml?branch=main&style=flat-square)](https://github.com/DPITrickster/ByeDPI-OpenWrt/actions)
[![GitHub stars](https://img.shields.io/github/stars/DPITrickster/ByeDPI-OpenWrt?style=flat-square&color=gold)](https://github.com/DPITrickster/ByeDPI-OpenWrt/stargazers)

Пакеты **ByeDPI** для маршрутизаторов на базе OpenWrt. 

Оригинальный проект **ByeDPI** (от разработчика [**hufrea**](https://github.com/hufrea)) - это утилита для модификации трафика, которая поднимает на вашем устройстве локальный SOCKS-прокси сервер.

Данный репозиторий предоставляет автоматическую сборку и удобные `.ipk` / `.apk` установочные пакеты.

> [!TIP]
> Нет нужной архитектуры - напиши ишью 😉

---

> [!CAUTION]
> ### ByeDPI сам по себе НЕ маршрутизирует ваш трафик!
> 
> Это исключительно **прокси-сервер** и **утилита модификации трафика**. Без дополнительных манипуляций он **никак не будет взаимодействовать с проходящим трафиком вашего роутера**. 
> 
> Чтобы трафик заворачивался в ByeDPI, вам необходимо использовать его совместно с другими инструментами. Вот основные сценарии маршрутизации:
> - Подключение к прокси-ядрам (в том числе к утилите [**podkop**](https://podkop.net/) на базе sing-box).
> - Использование утилит перенаправления и туннелирования: **redsocks**, **redsocks2**, **HevSocks5Tunnel** или **tun2proxy**.
> - Маршрутизация трафика на ByeDPI при помощи **dnsmasq**.
> - Настройки в режиме прозрачности (transparent mode) для роутеров с малым количеством памяти.

---

## 1. Установка

Перейдите на страницу **[релизов](https://github.com/DPITrickster/ByeDPI-OpenWrt/releases)** и скачайте нужный пакет.
Перед скачиванием настоятельно рекомендуем узнать точную архитектуру вашего роутера из терминала:

**Для OpenWrt <= 24.10 (файлы `.ipk`):**
```shell
opkg print-architecture
awk -F\' '/DISTRIB_ARCH/ {print $2}' /etc/openwrt_release
```

**Для OpenWrt >= 25.12 (файлы `.apk`):**
```shell
apk --print-arch
awk -F\' '/DISTRIB_ARCH/ {print $2}' /etc/openwrt_release
```

### Подключение репозитория (фид)

Помимо разовой загрузки `.ipk`/`.apk`, релизы публикуются как готовый фид на GitHub Pages —
тогда установка и обновления идут штатным менеджером пакетов. Подставьте версию OpenWrt и
архитектуру вашего устройства.

**Для OpenWrt <= 24.10 (`opkg`):**
```shell
ARCH=$(awk -F\' '/DISTRIB_ARCH/ {print $2}' /etc/openwrt_release)
VER=24.10   # ветка вашего OpenWrt
echo "src/gz byedpi https://dpitrickster.github.io/ByeDPI-OpenWrt/$VER/$ARCH" >> /etc/opkg/customfeeds.conf
opkg update
opkg install byedpi
```

**Для OpenWrt >= 25.12 (`apk`):**
```shell
ARCH=$(awk -F\' '/DISTRIB_ARCH/ {print $2}' /etc/openwrt_release)
VER=25.12
apk add --allow-untrusted --repository "https://dpitrickster.github.io/ByeDPI-OpenWrt/$VER/$ARCH" byedpi
```

> [!NOTE]
> Фид **не подписан**. Для `apk` используйте `--allow-untrusted` (как в команде выше).
> Если `opkg` ругается на отсутствие подписи — добавьте `option check_signature 0`
> в `/etc/opkg.conf`.

> [!TIP]
> Помимо стабильных релизов публикуются **пред-релизы** (`prerelease`, тег вида
> `v0.17.3-pre-<хэш>-<версия>`). Они собираются, когда в upstream-проекте появились новые коммиты,
> но номер версии ещё не подняли. Это сборки для тестирования — в стабильный фид они **не** попадают,
> ставить их стоит только вручную со страницы релизов.

---

## 2. Настройка и запуск

Пакет предназначен исключительно для работы через консоль, **веб-интерфейса (LuCI) в нём нет**.

1. Откройте конфигурационный файл в текстовом редакторе для изменения настроек:
   ```shell
   vi /etc/config/byedpi
   ```
2. По завершении настройки активируйте сервис для автозагрузки и запустите его:
   ```shell
   /etc/init.d/byedpi enable
   /etc/init.d/byedpi start
   ```

---

## 3. Способы маршрутизации (Интеграция)
- Прокси-ядра и утилиты на их основе, например **Podkop**. Подробную инструкцию по настройке связки читайте в [руководстве Podkop с ByeDPI](https://github.com/DPITrickster/Podkop-ByeDPI-OpenWRT).
- [**Раздел Обсуждений**](https://github.com/DPITrickster/ByeDPI-OpenWrt/discussions). Если вы ищете примеры настройки проксирования через redsocks/tun2proxy или хотите поделиться своим опытом, загляните в этот раздел.

---

## Благодарности
- **Автор [изначального порта](https://github.com/spvkgn/ByeDPI-OpenWrt) ByeDPI для OpenWrt:** spvkgn — огромное спасибо за создание `Makefile`, `init`-скриптов и оригинальных GitHub Actions
- **Разработчик оригинального [ByeDPI](https://github.com/hufrea/byedpi):** hufrea — не забудьте поставить ⭐ его проекту
