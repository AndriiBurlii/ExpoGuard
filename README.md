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

03.10.2026 виконано дві чисті Docker-збірки на Mac ARM64; SHA256 ELF/HEX/BIN збіглися. 04.10.2026 отримано скриншоти збирання ExpoGuard в Ubuntu та запуску у Wokwi: `STM32C031C6 ready`, `tick 0`..`tick 7` і різні стани LED. [Логи та докази](docs/validation/README.md).

[PDF-звіт](docs/SP_Lab01_Burlii_IO-32.pdf) додано в репозиторій; п'ять скриншотів містяться всередині PDF. Оригінал відео, копія без звуку та розшифровка збережені в каталозі Reports навчальної роботи. Драйвери датчиків і режими ExpoGuard заплановано на ЛР2.

Підготовчі коміти фактично створено 03.10.2026; їхні дати задані за погодженим графіком і не є журналом виконання в попередні дні. Коміт зі звітом і доказами створено 04.10.2026.

[PRD](docs/PRD.md) · [Словник](CONTEXT.md) · [Компоненти](docs/hardware-components.md) · [План ЛР1/ЛР2 і здачі](docs/lab-plan.md) · [Тести](docs/test-plan.md) · [Передача в VM](docs/vm-transfer.md)

Репозиторій: [AndriiBurlii/ExpoGuard](https://github.com/AndriiBurlii/ExpoGuard). `main` містить початковий коміт із порожнім деревом; підготовка, звіт і докази належать `feature/lab-1`. Для здачі відкрийте PR з `feature/lab-1` у `main`: [інструкція PR](docs/pull-request.md). PR створює користувач.
