<div align="center">

<img src="assets/cheburnet-scripts-banner.jpg" alt="ЧебурNET Scripts" width="100%">

# ЧебурNET Auto Tuning

### Адаптивная оптимизация и защита Linux-серверов для Xray и Remnawave

![Version](https://img.shields.io/badge/version-1.0.0-8b5cf6?style=for-the-badge)
[![Validation](https://img.shields.io/github/actions/workflow/status/leonidkopysov/CheburNET-Auto-Tuning/validate.yml?branch=main&style=for-the-badge&label=проверка)](https://github.com/leonidkopysov/CheburNET-Auto-Tuning/actions/workflows/validate.yml)
[![License](https://img.shields.io/badge/license-MIT-22c55e?style=for-the-badge)](LICENSE)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](#-совместимость)
[![Debian](https://img.shields.io/badge/Debian-12-A81D33?style=for-the-badge&logo=debian&logoColor=white)](#-совместимость)

**BBR · fq · ZRAM · sysctl · Conntrack · Remnawave · Fail2ban · Diagnostics**

</div>

## Быстрый запуск

Запустите от `root`:

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/leonidkopysov/CheburNET-Auto-Tuning/main/cheburnet-auto-tuning.sh)
```

Перед изменением системы скрипт показывает план и запрашивает подтверждение. Обозначения в интерактивных вопросах: `Д` — да, `Н` — нет, `П` — пропустить.

> [!IMPORTANT]
> Скрипт меняет параметры ядра, сети, swap и защиты сервера. Перед первым запуском на критической системе подготовьте snapshot или проверенную резервную копию.

## Что делает скрипт

| Раздел | Возможности |
|---|---|
| Сеть | BBR, `fq`, TCP/UDP buffers, backlog, MTU probing и диапазон локальных портов |
| Conntrack | адаптивный лимит, hash buckets, контроль загрузки таблицы |
| ZRAM | поиск, восстановление и установка ZRAM, выбор размера, алгоритма и приоритета |
| Remnawave/Xray | поиск контейнера ноды, NOFILE, network namespace, API-порт и системные лимиты |
| Защита | kernel hardening, SSH-аудит, Fail2ban, UFW/nftables, Docker socket и публичные порты |
| Обслуживание | security updates, NTP, TRIM, диск, inode, сертификаты и проверка после reboot |

Настройки рассчитываются по ресурсам и фактическому состоянию сервера. Повторный запуск поддерживается: скрипт снова выполняет диагностику и применяет только необходимые исправления.

## Совместимость

| Система | Статус |
|---|:---:|
| Ubuntu 22.04 LTS | ✅ Поддерживается |
| Ubuntu 24.04 LTS | ✅ Основная платформа |
| Debian 12 | ✅ Поддерживается |
| x86_64 / amd64 | ✅ Поддерживается |
| arm64 / aarch64 | ✅ Поддерживается |
| KVM/VPS и выделенный сервер | ✅ |
| Xray / Remnawave Node | ✅ |

Другие Debian-based системы требуют отдельной проверки.

## Переменные автоматизации

| Переменная | Назначение |
|---|---|
| `CHEBURNET_ASSUME_YES=1` | пропустить стартовое подтверждение |
| `CHEBURNET_SECURITY=0` | отключить блок безопасности |
| `CHEBURNET_ENABLE_UFW=1/0` | включить или пропустить настройку UFW |
| `CHEBURNET_PANEL_IPS="IP/CIDR"` | адреса панели Remnawave для доступа к API ноды |
| `CHEBURNET_PANEL_PORT=2222` | API-порт Remnawave Node |
| `CHEBURNET_FIREWALL_PORTS="tcp:443"` | дополнительные публичные порты |
| `CHEBURNET_HARDEN_SSH=1` | включить key-only SSH при наличии `authorized_keys` |
| `CHEBURNET_INSTALL_ZRAM_PACKAGES=0` | не устанавливать пакеты ZRAM |
| `CHEBURNET_SYSTEM_MAINTENANCE=0` | отключить системное обслуживание |
| `CHEBURNET_CERTIFICATES=0` | отключить аудит сертификатов |
| `CHEBURNET_POST_REBOOT_CHECK=0` | не создавать проверку после перезагрузки |

Пример автоматического запуска:

```bash
CHEBURNET_ASSUME_YES=1 \
bash <(curl -fsSL https://raw.githubusercontent.com/leonidkopysov/CheburNET-Auto-Tuning/main/cheburnet-auto-tuning.sh)
```

## Диагностика

В конце скрипт показывает контрольные результаты по sysctl, BBR/qdisc, conntrack, ZRAM, Remnawave Node, firewall и службам. Если требуется перезагрузка, одноразовая проверка сохраняет результат в:

```text
/var/lib/cheburnet-tuning/post-reboot-last.txt
```

## Безопасность

- Секретные значения не нужно передавать в Issues или публичные логи.
- SSH hardening включается только отдельной переменной и после проверки ключей.
- Скрипт не добавляет SSH-ключи.
- При конфликте с существующей системой firewall выводится предупреждение или остановка.
- Внешние проекты и пакеты сохраняют собственные лицензии.

Подробнее: [SECURITY.md](SECURITY.md) и [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

## Автор и лицензия

**Леонид Копысов**  
GitHub: **leonidkopysov**  
Telegram: **[@kopysovleonid](https://t.me/kopysovleonid)**

Оригинальный код ЧебурNET распространяется по лицензии [MIT](LICENSE).
