# ExpoGuard

Контроль виставкового макета. Власник: Бурлій Андрій Євгенійович, група ІО-32.

Середовище: Ubuntu 24.04, Docker, STM32 HAL, Wokwi. Платформа: Nucleo-C031C6.

ЛР1 готує тему, план реалізації, build/Docker/Git, UART-логер і LED. ЛР2 додасть PIR через GPIO, MPU6050 через I²C та фоторезистор через ADC. PIR реєструватиме звичайний рух відвідувача, MPU6050 рух або удар макета, LDR світло експозиції. Світло не доводить втручання; один PIR не рахує людей.

## Збірка та демонстрація

```sh
make clean
make build IMAGE=expoguard-lr1-build:local
```

Артефакти: `firmware/build/firmware.elf`, `.hex`, `.bin`. Відкрийте цю папку в VS Code, натисніть F1, виберіть `Wokwi: Start Simulator`. За наявної ліцензії очікуються `STM32C031C6 ready`, послідовні `tick` і зміна LED PA5 раз на 1000 мс, повний цикл близько 2 с. Trial не потрібен. Сама компіляція не підтверджує ці результати.

У коді перевірено USART2 PA2/PA3, 115200 8N1, `printf` через `__io_putchar` та `HAL_UART_Transmit`. Прошивку, `.ioc`, Dockerfile і схему перенесено без змін із SP-Boilerplate. I²C1 вже ініціалізується на PB8/PB9, датчиків у схемі немає.

Документація шаблона збережена в [docs/template-README.md](docs/template-README.md). Походження та ліцензії дивіться там і в `LICENSE`, `firmware/Drivers`.

## Статус

Підготовка відбувається 03.10.2026. Дати Git задані за погодженим графіком і не є журналом виконання в минулі дні. Дві чисті Docker-збірки на Mac ARM64 пройшли, SHA256 ELF/HEX/BIN однакові. [Логи та точний статус](docs/validation/README.md). Ubuntu VM і Wokwi UART/LED саме ExpoGuard ще не підтверджені. PDF-звіт, screenshots, відео та PR ще потрібні.

[PRD](docs/PRD.md) · [Словник](CONTEXT.md) · [Компоненти](docs/hardware-components.md) · [План ЛР1/ЛР2 і здачі](docs/lab-plan.md) · [Тести](docs/test-plan.md) · [Передача в VM](docs/vm-transfer.md)

`main` містить лише початковий коміт із порожнім деревом. Чотири змістовні коміти підготовки належать `feature/lab-1`. Віддалений репозиторій ще має створити користувач; origin лише збережений заздалегідь. Публікації гілок немає.
