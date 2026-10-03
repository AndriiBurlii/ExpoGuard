# Фактична перевірка ЛР1

Дата виконання: 03.10.2026. Host macOS Darwin 27.0.0 arm64, Docker Desktop context `desktop-linux`, engine Linux arm64. Це не запуск в Ubuntu VM. Сирі версії та UTC час у [environment.txt](environment.txt); образ і ARM GCC у [toolchain.txt](toolchain.txt).

## Результати

| Перевірка | Факт |
|---|---|
| B-01 | `make clean`, `make build IMAGE=expoguard-lr1-build:local` завершилися з exit 0, ELF/HEX/BIN створено |
| B-02 | Друге `make clean` видалило build; повторна збірка exit 0; `cmp` маніфестів SHA256 exit 0 |
| Компілятор | ARM GCC 10.3.1, ціль Cortex-M0+, образ `expoguard-lr1-build:local` linux/arm64 |
| Розмір | text 13464, data 120, bss 1808 байт за `arm-none-eabi-size`; перевищення linker memory не було |
| Попередження | У двох логах не знайдено рядків `warning:` або `error:` компілятора |
| Базові файли | 99 файлів шаблона, крім адаптованого README, побайтно ідентичні; [template.sha256](template.sha256) |
| UART/LED код | USART2 PA2/PA3 AF1 115200 8N1, PA5 toggle та HAL_Delay 1000 підтверджено по джерелах |
| B-03 Ubuntu | Не виконано. VirtualBox встановлений, `VBoxManage list vms` та `list runningvms` не дали зареєстрованих VM. Це не виключає VM на іншому комп'ютері |
| B-04/B-05 Wokwi | Не підтверджено для ExpoGuard. VS Code 1.139.1 arm64 та `wokwi.wokwi-vscode` є на Mac. Команда Start Simulator викликана у вікні ExpoGuard, але наступне спостереження показало інший проєкт. Його UART/LED не зараховано; screenshots цього запуску не збережено як доказ ExpoGuard |
| Ліцензія | Наявність ліцензії раніше підтвердив користувач. Trial не активовано; запуск саме цієї копії потребує перевірки |
| Сенсори | Не реалізовані, не тестувалися, схема та .ioc не змінювалися |

Повні логи: [build-1.log](build-1.log), [build-2.log](build-2.log). Маніфести: [build-1.sha256](build-1.sha256), [build-2.sha256](build-2.sha256).

| Артефакт | SHA256 обох збірок |
|---|---|
| ELF | `0f7b87f63097adfc290c57ea018a65e5a05da061aaa48fd520c14fbfe05f85da` |
| HEX | `20c68952fd3c7ab6640f8ca53935729d6ca2c54d55dbba98597a92c2e0248415` |
| BIN | `c21e312881deefbec24a06e9808d3651367530ac8ea930d3b37cc011daa7f7df` |

Рівність підтверджена для цього шляху монтування `/workspace` і цього образа. Dockerfile використовує неприкріплені пакети; майбутня перебудова образа або інша архітектура може дати інші файли. ELF містить debug info, тому шлях також впливає на порівняння. Загальну відтворюваність між ноутбуками ще не доведено.

## Залишок

Повторити чисті збірки в Ubuntu 24.04. Потім у VS Code із наявною ліцензією запустити саме ExpoGuard і записати UART/LED. Повні джерела компонентів, PDF, screenshots, п'ять фактичних складнощів і відео залишаються незавершеними. Повний перелік: [lab-plan.md](../lab-plan.md).
