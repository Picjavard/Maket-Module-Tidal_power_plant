# Maket-Module-Tidal_power_plant
 Модуль приливной электростанции для макета демонстрации альтернативных источников электроэнергии

## Список компонентов
 
* Микроконтроллер Arduino
* OLED-дисплей (1306 128x64, I2C) - https://sl.aliexpress.ru/p?key=LR1dsBY
* Датчик расхода воды - https://sl.aliexpress.ru/p?key=LR1dsBY

## 3D-модели для печати

Thingiverse: https://www.thingiverse.com/thing:6749869

## Подключение

### OLED-дисплей

| OLED-дисплей	| Arduino Nano |	Arduino Micro / Pro Micro |
| :---:| :---:| :---:|
| GND	| GND |	GND ![gnd](https://github.com/user-attachments/assets/c6ad6934-e844-4701-8427-447d0940b3cc) |
| VCC	| 5V |	5V |
| SCL	| A5 | 3 |
| SDA	| A4 |	2 |

### Датчик расхода воды

| Датчик расхода воды	| Arduino Nano | Arduino Micro / Pro Micro |
| :---:| :---:|:---:|
| GND(Черный)	| GND |	GND |
| VCC(Красный)	| 5V/VIN | 5V/VIN |
| SIG(Желтый)	| D2 | TX |

### Питание модуля

| Питание модуля	| Arduino Nano / Micro / Pro Micro |
| :---:| :---:| 
| GND	| GND |	
| +5V	| VIN |	

## Тестовый код oled-дисплея
```cpp
#include <GyverOLED.h>  // подключение библиотеки oled-дисплея
GyverOLED<SSD1306_128x64, OLED_NO_BUFFER> oled;


void setup() {
  oled.init();              // инициализация
  oled.clear();             // очистка дисплея
  oled.autoPrintln(false);  // отключить автопереход на новую строку
  oled.setScale(4);         // масштаб текста
  oled.setCursor(0, 0);     // установка позиции курсора
  oled.print("Init");
  oled.update();  // обновить экран
  delay(1000);
}

void loop() {
  // выводим 1 раз в секунду
  static uint32_t tmr;
  if (millis() - tmr > 1000) {
    tmr = millis();
    oled.clear();
    oled.setCursor(0, 0);
    oled.print();
    oled.update();
  }
}
```

## Код
```cpp
// измеряем обороты
// пин тахометра (желательна внешняя подтяжка 4.7к к VCC)

#define TACH_PIN 2  // Nano
//#define TACH_PIN 1  // Micro

#include <Tachometer.h>  // подключение библиотеки тахометра
Tachometer tacho;

#include <GyverOLED.h>  // подключение библиотеки oled-дисплея
GyverOLED<SSD1306_128x64, OLED_NO_BUFFER> oled;


void setup() {
  //Serial.begin(9600);

  // пин тахометра вентилятора подтягиваем к VCC
  pinMode(TACH_PIN, INPUT_PULLUP);

  // настраиваем прерывание
  attachInterrupt(0, isr, FALLING);  // Nano
  //attachInterrupt(3, isr, FALLING); // Micro

  tacho.setTimeout(2000);  // время таймаута

  oled.init();              // инициализация
  oled.clear();             // очистка дисплея
  oled.autoPrintln(false);  // отключить автопереход на новую строку
  oled.setScale(4);         // масштаб текста
  oled.setCursor(0, 0);     // установка позиции курсора
  oled.print("Init");
  oled.update();  // обновить экран
  delay(1000);
}

// обработчик прерывания
void isr() {
  tacho.tick();  // сообщаем библиотеке о новом сигнале
}

void loop() {
  // выводим 1 раз в секунду
  static uint32_t tmr;
  if (millis() - tmr > 1000) {
    tmr = millis();

    //Serial.println(tacho.getRPM());     // вывод на ПК

    oled.clear();
    oled.setCursor(0, 0);
    oled.setScale(4);         // масштаб текста
    oled.print(tacho.getRPM());
    oled.setCursor(0, 5);
    oled.setScale(3);         // масштаб текста
    oled.print("  кВт/ч");
    oled.update();
  }
}
```
