# Sharkord Pterodactyl Egg

[![Pterodactyl Egg](https://img.shields.io/badge/Pterodactyl-Egg-blue.svg)](https://pterodactyl.io)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

An official-style, robust Pterodactyl egg for hosting **Sharkord** — a self-hosted voice, video, and text chat server. This egg comes pre-configured with **Caddy** (featuring automatic Cloudflare DNS-01 SSL certificate generation) to make securing your instance effortless.

---

## Features

- **Automated Binary Management:** Automatically downloads or updates the latest Sharkord binary on startup.
- **Built-in Reverse Proxy (Caddy):** Includes an integrated Caddy web server compiled with the Cloudflare DNS plugin.
- **Automatic SSL via DNS-01:** Obtains valid Let's Encrypt certificates through Cloudflare without requiring open ports 80/443.
- **Smart Configuration Persistence:** Automatically writes a default template (`demo.sharkord.com`) and updates ports safely without overwriting your custom domain or tokens.
- **Console Cleaner:** Optional toggle to suppress verbose Caddy log output and keep your server console clean.

---

## Requirements

- A running **Pterodactyl Panel** (v1.x or v2.x).
- A Docker daemon supporting standard Debian-based images (`ghcr.io/parkervcp/yolks:debian`).
- A **Cloudflare account** managing your domain, with an **API Token** that has `Zone.DNS:Edit` permissions.

---

## Installation Guide

1. Download the `egg-sharkord.json` file from this repository.
2. Go to your Pterodactyl Admin Panel -> **Nests** -> select your nest (or create one) -> **Import Egg**.
3. Upload the `egg-sharkord.json` file and save.
4. Create a new server in Pterodactyl using this newly created **Sharkord** egg.

---

## Configuration & Startup Variables

| Variable Name | Env Key | Default | Description |
| :--- | :--- | :--- | :--- |
| **WebRTC Port** | `RTC_PORT` | `40000` | Additional port allocated for WebRTC voice and video streams. |
| **Auto Update** | `AUTO_UPDATE` | `1` | Automatically check and download the latest Sharkord release on startup (`1` = enabled, `0` = disabled). |
| **Start Caddy Server** | `START_CADDY` | `0` | Starts the built-in Caddy reverse proxy server (`1` = enabled, `0` = disabled). |
| **Disable Caddy Logs** | `DISABLE_CADDY_LOGS` | `0` | Hides Caddy console output logs to keep the server console clean (`1` = hidden, `0` = visible). |

---

## Setting Up SSL with Cloudflare (DNS-01 Challenge)

Because Pterodactyl containers run behind random ports and lack privileged access to ports 80 and 443, standard HTTP SSL challenges won't work out of the box. This egg uses Cloudflare DNS-01 validation instead:

1. Turn on **Start Caddy Server** in your server settings (`START_CADDY=1`).
2. Start your server for the first time. The script will generate a template `Caddyfile`.
3. Open your server's **File Manager** in Pterodactyl and open `Caddyfile`.
4. Replace `demo.sharkord.com` with your actual domain name and update the port to match your server's assigned primary port.
5. Replace `YOUR_CLOUDFLARE_API_TOKEN_HERE` with your real Cloudflare API token.

Example of a correctly configured `Caddyfile`:
```caddy
yourdomain.com:25565 {
    tls {
        dns cloudflare your_actual_cloudflare_api_token_here
    }
    reverse_proxy 127.0.0.1:4991 {
        header_up X-Real-IP {remote_host}
    }
    header -X-Frame-Options
    header -Content-Security-Policy
    header Access-Control-Allow-Origin *
}
```
6. Save the file and restart your server. Caddy will automatically request and apply your SSL certificate.

---

## Documentation Links

- [Sharkord Official Repository](https://github.com/Sharkord/sharkord)
- [Caddy Server Documentation](https://caddyserver.com/docs/)
- [Caddy Cloudflare DNS Plugin](https://github.com/caddy-dns/cloudflare)
- [Cloudflare API Tokens Guide](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)
- [Pterodactyl Documentation](https://pterodactyl.io/)

---
---

# Sharkord Pterodactyl Egg (Русский)

Надежный и продуманный конфигурационный файл (Egg) для панели Pterodactyl для запуска **Sharkord** — хостинг-сервера голосовых, видео и текстовых чатов. Этот конфиг идет с уже встроенным веб-сервером **Caddy** (со сборкой под плагин Cloudflare DNS-01), что позволяет автоматически выпускать SSL-сертификаты без лишней головной боли.

---

## Возможности

- **Автоматическое обновление бинарника:** Сама скачивает или обновляет последнюю версию Sharkord при старте.
- **Встроенный обратный прокси (Caddy):** Содержит компиляцию Caddy с поддержкой DNS-провайдера Cloudflare.
- **Авто-SSL через DNS-01:** Получает валидные сертификаты Let's Encrypt через ваш Cloudflare, не требуя открытых системных портов 80 и 443.
- **Умная работа с конфигом:** Создает шаблон с заглушкой `demo.sharkord.com` при первом запуске и меняет порт автоматически, но как только вы пропишете свой домен — скрипт больше никогда его не затрет.
- **Очистка консоли:** Переключатель для скрытия лишних логов Caddy, чтобы в консоли сервера отображался только чистый вывод Sharkord.

---

## Требования

- Работающая панель **Pterodactyl** (v1.x или v2.x).
- Docker-образ на базе Debian (`ghcr.io/parkervcp/yolks:debian`).
- Аккаунт в **Cloudflare**, где привязан ваш домен, и выпущенный **API Token** с правами `Zone.DNS:Edit`.

---

## Установка

1. Скачайте файл `egg-sharkord.json` из этого репозитория.
2. Перейдите в админ-панель Pterodactyl -> **Nests** (Гнезда) -> выберите нужное гнездо -> **Import Egg** (Импортировать яйцо).
3. Загрузите скачанный JSON-файл и сохраните.
4. Создайте новый сервер в панели, выбрав появившееся яйцо **Sharkord**.

---

## Переменные окружения

| Название переменной | Ключ (Env) | По умолчанию | Описание |
| :--- | :--- | :--- | :--- |
| **WebRTC Port** | `RTC_PORT` | `40000` | Дополнительный порт для медиа-потоков WebRTC (голос и видео). |
| **Auto Update** | `AUTO_UPDATE` | `1` | Автоматически проверять и качать свежую версию Sharkord при старте (`1` = вкл, `0` = выкл). |
| **Start Caddy Server** | `START_CADDY` | `0` | Запуск встроенного прокси-сервера Caddy (`1` = вкл, `0` = выкл). |
| **Disable Caddy Logs** | `DISABLE_CADDY_LOGS` | `0` | Скрывает логи Caddy из общей консоли сервера (`1` = скрыть, `0` = показывать). |

---

## Настройка SSL через Cloudflare (DNS-01 проверка)

Поскольку контейнеры в Pterodactyl работают на изолированных портах и не имеют доступа к привилегированным портам 80/443, классический способ получения SSL через HTTP невозможен. Данный конфиг решает это через проверку DNS-01 от Cloudflare:

1. В настройках сервера в панели включите `START_CADDY=1`.
2. Запустите сервер в первый раз — скрипт автоматически сгенерирует файл `Caddyfile`.
3. Откройте **Файловый менеджер** панели Pterodactyl и найдите файл `Caddyfile`.
4. Замените `demo.sharkord.com` на ваш реальный домен, а также укажите назначенный вашему серверу основной порт панели.
5. Замените `YOUR_CLOUDFLARE_API_TOKEN_HERE` на ваш настоящий токен Cloudflare.

Пример правильно заполненного файла `Caddyfile`:
```caddy
e.yourdomain.com:25565 {
    tls {
        dns cloudflare ваш_настоящий_токен_cloudflare_сюда
    }
    reverse_proxy 127.0.0.1:4991 {
        header_up X-Real-IP {remote_host}
    }
    header -X-Frame-Options
    header -Content-Security-Policy
    header Access-Control-Allow-Origin *
}
```
6. Сохраните файл и перезапустите сервер. Caddy автоматически получит SSL-сертификат и запустит защищенное соединение.

---

## Ссылки на документацию

- [Официальный репозиторий Sharkord](https://github.com/Sharkord/sharkord)
- [Документация Caddy Server](https://caddyserver.com/docs/)
- [Плагин Caddy Cloudflare DNS](https://github.com/caddy-dns/cloudflare)
- [Инструкция по созданию API-токенов Cloudflare](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)
- [Официальный сайт Pterodactyl](https://pterodactyl.io/)
