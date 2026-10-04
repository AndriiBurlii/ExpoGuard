# Створення PR для ЛР1

1. Відкрийте [порівняння main і feature/lab-1](https://github.com/AndriiBurlii/ExpoGuard/compare/main...feature/lab-1?expand=1).
2. Перевірте `base: main` та `compare: feature/lab-1`.
3. Натисніть **Create pull request**.
4. Вкажіть назву `ЛР1: ExpoGuard, Бурлій Андрій, ІО-32` та опис нижче.
5. Натисніть **Create pull request** для створення. Якщо відомий GitHub-акаунт викладача та він доступний у списку, додайте його в **Reviewers**.
6. Передайте посилання на PR викладачу. Залиште PR відкритим до завершення перевірки.

Опис для вставлення:

```text
Бурлій Андрій Євгенійович, група ІО-32.

ЛР1 ExpoGuard: базова прошивка STM32C031C6 зі STM32 HAL, Docker/Makefile, UART-журнал і LED у Wokwi. Додано PRD, план компонентів GPIO/I2C/ADC і критерії тестів ЛР2.

PDF-звіт: docs/SP_Lab01_Burlii_IO-32.pdf.
П'ять скриншотів включено до PDF-звіту.
Перевірки та логи: docs/validation/README.md.

Дві чисті Docker-збірки на Mac дали однакові SHA256 ELF/HEX/BIN. Надані скриншоти Ubuntu підтверджують завершення make build; Wokwi показує STM32C031C6 ready, tick 0..7 та різні стани LED.

Драйвери датчиків і режими ExpoGuard заплановано на ЛР2.
```

[Інструкція GitHub](https://docs.github.com/en/pull-requests/how-tos/create-pull-requests/creating-a-pull-request)
