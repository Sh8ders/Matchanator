# Matchanator

An Arduino-powered automatic matcha preparation machine that heats water to the ideal temperature, precisely doses matcha powder through a sifting mechanism, and whisks everything together — all at the press of a button.

![Status](https://img.shields.io/badge/status-in%20progress-yellow)
![Platform](https://img.shields.io/badge/platform-Arduino%20Mega%202560-blue)

## What This Project Does

The Matchanator automates the full matcha-making process:

1. **Heats water** to ~75°C (the ideal temperature for matcha — not boiling)
2. **Dispenses matcha powder** via a stepper-driven auger with precise gram-level dosing
3. **Sifts the powder** through a fine mesh screen to eliminate clumps
4. **Pumps heated water** into the mixing cup using a food-safe peristaltic pump
5. **Whisks everything** with a motorized frother arm that lowers into the cup
6. **Signals completion** on an LCD display — ready to drink

A load cell under the mixing cup provides closed-loop weight feedback for both powder and water dosing, ensuring consistent results every time.

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      MATCHANATOR                        │
│                                                         │
│  ┌───────────┐   ┌───────────┐   ┌──────────────────┐   │
│  │   Water   │──▶│  Heater   │──▶│   Peristaltic    │   │
│  │ Reservoir │   │ (PTC 12V) │   │      Pump        │   │
│  └───────────┘   └─────┬─────┘   └────────┬─────────┘   │
│                        │                  │             │
│                  ┌─────┴─────┐            │             │
│                  │  DS18B20  │            ▼             │
│                  │ Temp Probe│     ┌─────────────┐      │
│                  └───────────┘     │  Mixing Cup │      │
│                                   │  (on scale) │      │
│  ┌───────────┐   ┌───────────┐    └──────┬──────┘      │
│  │  Matcha   │──▶│   Auger   │──▶ Sifter ─┘            │
│  │  Hopper   │   │ (Stepper) │                         │
│  └───────────┘   └───────────┘         ▲               │
│                                        │               │
│                              ┌─────────┴────────┐      │
│                              │  Whisk Motor +   │      │
│                              │  Servo Arm       │      │
│                              └──────────────────┘      │
│                                                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │              Arduino Mega 2560                   │   │
│  │         + ESP-01 WiFi Module                     │   │
│  │         + HX711 Load Cell Amplifier              │   │
│  │         + 20x4 I2C LCD + Rotary Encoder          │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

## Subsystems

| Subsystem | Key Components | Control Method |
|-----------|---------------|----------------|
| Water heating | PTC element (12V/50W), DS18B20 probe | Bang-bang with hysteresis via SSR |
| Water delivery | 12V peristaltic pump, silicone tubing | MOSFET PWM, closed-loop via load cell |
| Powder dispensing | 28BYJ-48 stepper, 3D-printed auger | Step count + load cell feedback |
| Sifting | 80-mesh SS screen, coin vibration motor | On during powder dispense cycle |
| Mixing | Cannibalized frother motor, SG90 servo | Servo lowers arm, DC motor spins whisk |
| User interface | 20x4 I2C LCD, rotary encoder | Menu-driven brew profile selection |
| Wireless | ESP-01 (ESP8266) WiFi module | Web interface served over local network |

## Repository Structure

```
Matchanator/
├── README.md
├── LICENSE
├── .gitignore
├── code/
│   ├── src/          # Arduino sketches (.ino files)
│   ├── lib/          # Custom libraries (scale, pump, heater, auger)
│   └── test/         # Individual subsystem test sketches
├── cad/
│   ├── fusion360/    # Source CAD files (.f3d)
│   └── stl/          # Export STL files for 3D printing
├── docs/
│   ├── build-log.md  # Dated build journal with photos
│   ├── parts-list.md # Complete bill of materials
│   ├── wiring.md     # Wiring documentation and pinout
│   ├── images/       # Build photos and diagrams
│   └── schematics/   # Fritzing or KiCad schematic files
└── hardware/
    └── pinout.md     # Arduino pin assignment reference
```

## Build Phases

- [ ] **Phase 1** — Bench-test subsystems (scale → pump → heater → auger)
- [ ] **Phase 2** — CAD enclosure and mounting around proven hardware
- [ ] **Phase 3** — Integration code (finite state machine)
- [ ] **Phase 4** — WiFi interface and UI polish
- [ ] **Phase 5** — Documentation, demo video, final cleanup

## Parts

See the full bill of materials in [`docs/parts-list.md`](docs/parts-list.md).

Estimated total cost: **~$225**

## Tools Required

- Soldering iron + solder
- Multimeter
- Wire strippers / flush cutters
- Small Phillips screwdriver set
- Hot glue gun
- 3D printer access (for auger, hopper, and brackets — PETG filament)

## Current Status

🟡 **Pre-build** — Parts being ordered, repository scaffolded, documentation started.

## Future Improvements (v2)

- Replace Arduino Mega + ESP-01 with a single ESP32 for native WiFi
- Design a custom PCB to replace breadboard wiring
- Add a matcha powder level sensor in the hopper
- Implement OTA (over-the-air) firmware updates
- Enclosed, food-safe housing with removable components for cleaning

## License

This project is licensed under the MIT License — see [`LICENSE`](LICENSE) for details.
