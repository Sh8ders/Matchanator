# Matchanator — Arduino Mega Pin Assignments

## Pin Map

| Pin | Type | Connected To | Notes |
|-----|------|-------------|-------|
| **Scale (HX711)** | | | |
| 2 | Digital | HX711 CLK | Clock for load cell amplifier |
| 3 | Digital | HX711 DT (Data) | Data from load cell amplifier |
| **Temperature** | | | |
| 4 | Digital | DS18B20 Data | 1-Wire bus, 4.7kΩ pull-up to 5V |
| **Heater** | | | |
| 5 | Digital | SSR-25DD Input (+) | HIGH = heater on |
| **Pump** | | | |
| 6 | Digital PWM | IRF520 MOSFET SIG | PWM for pump speed control |
| **Stepper (Auger)** | | | |
| 8 | Digital | ULN2003 IN1 | Stepper coil A |
| 9 | Digital | ULN2003 IN2 | Stepper coil B |
| 10 | Digital | ULN2003 IN3 | Stepper coil C |
| 11 | Digital | ULN2003 IN4 | Stepper coil D |
| **Vibration Motor** | | | |
| 12 | Digital | Coin vibe motor (via transistor) | Sifter vibration |
| **Whisk** | | | |
| 13 | Digital PWM | Frother motor (via MOSFET) | Whisk spin speed |
| 7 | Digital PWM | SG90 Servo signal | Whisk arm up/down |
| **LCD (I2C)** | | | |
| 20 (SDA) | I2C | LCD SDA | 20x4 I2C LCD data |
| 21 (SCL) | I2C | LCD SCL | 20x4 I2C LCD clock |
| **Rotary Encoder** | | | |
| 22 | Digital | Encoder CLK | Rotation signal A |
| 23 | Digital | Encoder DT | Rotation signal B |
| 24 | Digital | Encoder SW | Push button (active LOW) |
| **ESP-01 WiFi** | | | |
| TX1 (18) | Serial1 TX | ESP-01 RX | Via logic level converter |
| RX1 (19) | Serial1 RX | ESP-01 TX | 3.3V safe directly |

## Power Connections

| Rail | Source | Supplies |
|------|--------|----------|
| 12V | PSU barrel jack | Heater (via SSR), Pump (via MOSFET), Buck converter input |
| 5V | LM2596 output | Arduino Vin, Servo, Stepper driver, LCD, HX711, Encoder |
| 3.3V | ESP-01 adapter board | ESP-01 module only |
| GND | Common | All modules share common ground — this is critical |

## Notes

- Pin assignments are preliminary and may shift during integration.
- All grounds must be tied together (Arduino GND, PSU GND, MOSFET GND, SSR GND).
- Analog pins A0–A15 are available as reserves if needed.
- Serial0 (pins 0/1) is reserved for USB debugging — WiFi uses Serial1.
- Update this file whenever pin assignments change during the build.
