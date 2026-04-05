# Matchanator — Wiring Documentation

## Overview

The Matchanator uses a 12V main power rail stepped down to 5V for logic. All high-current loads (heater, pump) are switched via MOSFETs or SSR from the 12V rail. The Arduino Mega coordinates everything.

## Power Distribution

```
12V 5A PSU (barrel jack)
├── 12V rail
│   ├── PTC Heater (via SSR-25DD)
│   ├── Peristaltic Pump (via IRF520 MOSFET)
│   └── LM2596 Buck Converter
│       └── 5V rail
│           ├── Arduino Mega (Vin or 5V pin)
│           ├── SG90 Servo
│           ├── 28BYJ-48 Stepper (via ULN2003)
│           ├── Frother motor (via MOSFET)
│           ├── Coin vibration motor
│           └── ESP-01 (via 3.3V adapter)
```

## Wiring Diagram

*(To be added — Fritzing or KiCad schematic will go in `docs/schematics/`)*

## Connection Reference

See [`hardware/pinout.md`](../hardware/pinout.md) for the full Arduino pin assignment table.

## Important Notes

- The ESP-01 runs on **3.3V logic**. Use the breadboard adapter with built-in regulator, or a logic level converter between it and the Mega's 5V serial pins.
- The DS18B20 requires a **4.7kΩ pull-up resistor** between the data line and VCC.
- The IRF520 MOSFET needs **common ground** between the Arduino, MOSFET module, and the 12V supply.
- The HX711 load cell amplifier is sensitive to noise — keep its wiring away from the stepper motor and heater power lines.
- The SSR controlling the heater should be mounted on a **heat sink** if the heater draws sustained current.
