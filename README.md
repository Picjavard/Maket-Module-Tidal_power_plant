# Maket-Module-Tidal_power_plant
 Модуль приливной электростанции для макета демонстрации альтернативных источников электроэнергии

## Список компонентов
 
* Микроконтроллер Arduino
* OLED-дисплей (1306 128x64, I2C)
* Датчик расхода воды


## Подключение

### OLED-дисплей

| OLED	| Arduino Nano |	Arduino Micro / Pro Micro |
| :---:| :---:| :---:|
| GND	| GND |	GND |
| VCC	| 5V |	5V |
| SCL	| A5 | D3 |
| SDA	| A4 |	D2 |

### Датчик расхода воды

| Датчик расхода воды	| Arduino Nano / Micro / Pro Micro |
| :---:| :---:| 
| GND	| GND |	
| VCC	| 5V |	
| SIG	| TX | 

### Питание модуля

| Питание модуля	| Arduino Nano / Micro / Pro Micro |
| :---:| :---:| 
| GND	| GND |	
| +5V	| VIN |	


