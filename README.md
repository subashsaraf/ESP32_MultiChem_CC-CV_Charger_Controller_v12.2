# ESP32 Universal CC/CV Charger Controller

**Version:** v12.1  
**Platform:** ESP32 (DOIT ESP32 DEVKIT V1)  
**License:** MIT  
**Author:** Subash Saraf  

---

## 🚀 Overview

ESP32 Universal CC/CV Charger Controller is a **professional-grade, hysteresis-driven smart charger firmware** designed for **Lead Acid, Li-ion, and LiFePO₄ batteries**.  
It supports **fully automatic charging**, **load management**, **safety protections**, **LCD + Web UI**, and **long-term stability** for real-world deployments.

This project is suitable for:
- Solar & DC charger systems  
- Battery backup / UPS projects  
- Bench power & charger automation  
- Embedded power electronics learning  

---

## ✨ Key Features

- Full CC/CV charging algorithm  
- State machine:  
  **BULK → ABSORPTION → FLOAT → IDLE → BULK**  
- Battery-type-specific parameters (no magic numbers)  
- PWM safety for IR2104 bootstrap operation  
- Over-voltage, over-current & thermal protection  
- Load control (AUTO / MANUAL) with debounce & alarms  
- Web interface (WiFi + mDNS)  
- 20×4 I²C LCD UI with icons  
- Active buzzer alerts  
- DS18B20 temperature-based fan control  
- Preferences (Flash) storage for calibration & settings  
- Watchdog-protected long uptime  
- Simulation mode for safe testing  

---

## 🔋 Supported Batteries

| Battery Type | Voltage Range |
|-------------|---------------|
| Lead Acid 12V | 10.5 – 12.7 V |
| LiFePO₄ 12V | 10.0 – 13.8 V |
| Li-ion 3S | 9.0 – 12.6 V |

All parameters are defined in `battery_types.h`.

---

## 🧠 Charging State Diagram

```
        +-------+
        | BULK  |
        +-------+
            |
            v
     +--------------+
     | ABSORPTION   |
     +--------------+
            |
            v
        +-------+
        | FLOAT |
        +-------+
            |
            v
        +-------+
        | IDLE  |
        +-------+
            |
            v
        (Voltage Drop)
            |
            v
          BULK
```

BACKUP mode is entered automatically when input power is lost or safety limits are exceeded.

---

## 📌 Pin Mapping (DOIT ESP32 DEVKIT V1)

### Power & Control
| Function | GPIO |
|-------|------|
| PWM Output (IR2104) | GPIO 25 |
| Driver Shutdown (SD) | GPIO 33 |
| Load Relay Control | GPIO 5 |

### LEDs
| LED | GPIO |
|----|------|
| Critical Battery | GPIO 26 |
| Medium Battery | GPIO 27 |
| Full Battery | GPIO 32 |
| Battery Status PWM | GPIO 14 |

### Sensors
| Sensor | GPIO |
|------|------|
| DS18B20 Temperature | GPIO 4 |
| ADS1115 SDA | GPIO 21 |
| ADS1115 SCL | GPIO 22 |

### Buttons
| Button | GPIO |
|------|------|
| MODE | GPIO 19 |
| LOAD | GPIO 16 |
| WIFI RESET | GPIO 15 |
| CALIBRATE | GPIO 17 |

---

## 📟 LCD Display Layout (20×4)

```
POW ⚡  BAT [icon]  WiFi
Vin     Vbat       PWM%
Iout    Temp       Batt%
Power   Mode   Load
```

- Blinking mode name = active charging  
- PWM % = relative to **user-defined safe limit**  
- Battery icon scales with voltage  

---

## 📡 Serial Log Example

```
[224s] Input V: 24.01 V  Batt V: 14.75 V  Current: 0.02 A
PWM: 12% (Limit: 70%)  FanPWM: 35%  Temp: 32.1 °C
Mode: (ABSO → IDLE 6s / 20s)
Load: Off | LoadMode: A | Calib: 2.498 V
```

Includes:
- Next mode prediction  
- Elapsed / total timers  
- Calibration voltage  
- Safety-limited PWM percentage  

---

## 🛠️ Build & Upload

1. Install Arduino IDE (ESP32 core ≥ v2.x)  
2. Install libraries:
   - Adafruit ADS1X15  
   - LiquidCrystal I2C  
   - DallasTemperature  
   - Bounce2  
   - WiFiManager  
3. Select board:
   - **DOIT ESP32 DEVKIT V1**
4. Upload code
5. Monitor serial @ **115200 baud**

---

## 🧪 Simulation Mode

Enable safe testing without hardware:

```cpp
#define SIMULATION_MODE 1
```

Simulates:
- Battery voltage
- Current
- Temperature
- Mode transitions

---

## 🔐 Safety Design

- IR2104 bootstrap-safe PWM limits (70–85%)  
- Hard shutdown on over-voltage  
- Hiccup retry with latch-off  
- Watchdog protected main loop  
- I²C bus recovery logic  

---

## 📄 License

MIT License  
Free to use, modify, and distribute with attribution.

---

## 👨‍💻 Developer

**Dev By Subash Saraf**  
Embedded Systems | Power Electronics | ESP32  

---

⭐ If you find this project useful, please star the repository!
