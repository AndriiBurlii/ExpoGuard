# Компоненти та інтерфейси

Стан дослідження 03.10.2026. ЛР1 використовує лише Nucleo-C031C6, UART і LED. Три сенсори заплановані для ЛР2; нижче наведено проєкт підключення, а не готову схему.

## Перевірена база

| Вузол | Поточне підключення та факт | Джерело |
|---|---|---|
| STM32C031C6 | Cortex-M0+, 32 КБ Flash, 12 КБ RAM; не Cortex-M4 із прикладу методички | [ST DS13867 Rev 4, с. 1](https://www.st.com/resource/en/datasheet/stm32c031c6.pdf#page=1) |
| USART2 | PA2 TX, PA3 RX, AF1; 115200, 8 біт, 1 stop, без parity | `Core/Src/stm32c0xx_hal_msp.c`, `Core/Src/main.c`; [DS13867, с. 35, табл. 13](https://www.st.com/resource/en/datasheet/stm32c031c6.pdf#page=35) |
| LED | PA5, GPIO output; HIGH світить; toggle раз на 1000 мс | `Core/Inc/main.h`, `Core/Src/main.c`; [Wokwi, Onboard LED](https://docs.wokwi.com/parts/board-st-nucleo-c031c6#onboard-led) |
| I²C1 | PB8 SCL, PB9 SDA, AF6, open-drain; ініціалізація є, пристроїв немає | `Core/Src/stm32c0xx_hal_msp.c`; [DS13867, с. 36, табл. 15](https://www.st.com/resource/en/datasheet/stm32c031c6.pdf#page=36) |

Шляхи `Core/...` у таблиці відносні до `firmware/`. USART2 у `diagram.json` з'єднано з вбудованим Serial Monitor. `syscalls.c` передає `_write` до `__io_putchar`; він викликає HAL UART. I²C timing `0x20303E5D` збережено як у шаблоні, фактичну частоту SCL ще не виміряно. SPI, PWM та ADC MCU доступні за datasheet; їхня наявність не означає налаштовані інтерфейси цього проєкту.

## План ЛР2

| Компонент | Піни, протокол та живлення | Що вимірює | Джерело та стан |
|---|---|---|---|
| `wokwi-pir-motion-sensor` | VCC/GND, OUT → PA4 GPIO input, пін лише кандидат | Рух поруч, FR-03 | [Wokwi, Pin names / Attributes / Using the sensor](https://docs.wokwi.com/parts/wokwi-pir-motion-sensor). OUT цифровий; типово HIGH 5 с, inhibit 1.2 с, retrigger продовжує HIGH. |
| `wokwi-mpu6050` | VCC → 3V3, GND, SCL → PB8, SDA → PB9, AD0 → GND; 7-бітна адреса 0x68, AD0 HIGH дає 0x69. Ціль 100 кГц | Прискорення в g, обертання в °/с, FR-04..07 | [Wokwi, Pin names / Units](https://docs.wokwi.com/parts/wokwi-mpu6050). Перевірені можливості моделі; електрика модуля окремо. |
| `wokwi-photoresistor-sensor` | VCC → 3V3, GND, AO → PA0 ADC, DO не використовуємо; пін лише кандидат | Відносне світло, FR-08..09 | [Wokwi, Pin names / Operation](https://docs.wokwi.com/parts/wokwi-photoresistor-sensor). LDR і 10 кОм утворюють подільник, AO залежить від світла. |

PA0 як аналоговий вхід треба звірити з [DS13867, с. 31..34, табл. 12](https://www.st.com/resource/en/datasheet/stm32c031c6.pdf#page=31), конкретним корпусом і CubeMX перед зміною `.ioc`. У STM32 HAL I²C адреса передається зсунутою вліво на один біт; у документації адреси залишаються 7-бітними. Pull-up SDA/SCL, допустимі рівні та всі GPIO/ADC з'єднання перевірити на етапі ЛР2. У поточній схемі датчики відсутні.

## Datasheet та невиконані частини дослідження

[TDK InvenSense PS-MPU-6000A-00 Rev 3.4, с. 10](https://product.tdk.com/system/files/dam/doc/product/sensor/mortion-inertial/imu/data_sheet/mpu-6000-datasheet1.pdf#page=10) описує діапазони ±2/4/8/16 g, ±250/500/1000/2000 °/с та I²C до 400 кГц. [С. 14](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6000-Datasheet1.pdf#page=14) дає VDD чипа 2.375..3.46 В і типовий струм 3.8 мА для accel+gyro без DMP. Це параметри чипа, не бюджет живлення довільної плати GY-521. Вміст цих сторінок перевірено через індекс первинного джерела; пряме завантаження PDF повернуло 403/404. Для звіту потрібно відкрити повний офіційний PDF і звірити редакцію.

[Офіційний Register Map RM-MPU-6000A-00 Rev 4.2](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6000-Register-Map1.pdf) знайдено, але прямий URL повернув HTML. План для драйвера: перевірити WHO_AM_I, вихід зі sleep через PWR_MGMT_1, налаштування CONFIG/ACCEL_CONFIG/GYRO_CONFIG та читання ACCEL/GYRO output. Адреси, біти, початкові значення й точні сторінки ще треба звірити за повним первинним PDF. Не видавати цей план за виконану перевірку регістрів.

Виробник та фізичні моделі PIR і LDR ще не визначені. Тому немає обґрунтованих значень струму, допустимого VCC та офіційних datasheet із сторінками. Документація Wokwi описує модель симулятора й не замінює ці дані для фізичного модуля. Струм усієї системи та можливість живлення всього набору від 3V3 Nucleo не підтверджені. До звіту потрібно явно узгодити з викладачем форму дослідження симульованих компонентів або вибрати фізичні аналоги й завершити таблицю.

Для ADC заплановано шкалу 0..4095 за 12-бітного налаштування і діапазон 0..VREF+, ціль VREF+=3.3 В. Таблиця Wokwi для 5 В і 10-бітного Arduino ADC не є налаштуванням STM32. Не подавати 5 В на аналоговий вхід і не називати відносне світло точним виміром lux без калібрування.
