# uAir-Sat

The **uAir-Sat** is a LoRaWAN IoT device based on the **POLSAT reference design**, which utilizes the **STM32WL5MOC** architecture and the **EM2050 OEM module** from EchoStar Mobile.  
The device has gone through three hardware revisions: **Rev1**, **Rev2**, and **Rev3**.

---

## Board Architecture

The uAir-Sat design consists of two interconnected modules:
- **Main Board**
- **Auxiliary Sensor Board**  
These boards are connected via a **Flexible Printed Circuit (FPC)**.

---

### 🧩 Main Board

#### Core Components
- **STM32WL5MOC** module  
  - Dual-core STM32L4 MCU (Arm Cortex-M4 + M0+)  
  - Integrated **Semtech SX1262** radio  
  - Supports **LoRa®, (G)FSK, (G)MSK, and BPSK** modulations  
  - Handles application logic, control, and terrestrial LoRaWAN communication  

- **EM2050** module (EchoStar Mobile)  
  - Dual-mode **LoRaWAN Terrestrial/Satellite** communication  
  - Contains **STM32WB15** MCU and **Semtech LR1120** transceiver  
  - Managed via **UART** using AT-style commands  
  - Supports **Satellite S-band (MSS-S)** and **Terrestrial EU868** bands  

- **GNSS (LC76G)**  
  - Dedicated GPS/Beidou chip for location tracking  

- **External Flash (W25Q16JV)** for data logging and FUOTA (Firmware Update Over The Air)  
- **FRAM (MB85RC64TA)** for non-volatile data storage  

#### Environmental Sensors
- **SHTC3 (Internal)** – digital temperature and humidity sensor  
  - Size: 2 × 2 × 0.75 mm DFN  
  - I²C Fast Mode Plus  
  - Optimized for ultra-low power  

- **IIS2MDC** – 3-axis magnetometer  
  - ±50 gauss dynamic range  
  - 16-bit output, I²C/SPI interface  

- **PCBA-DBM-PRO** – audio-level sound sensor  
  - I²C interface  
  - Built-in MEMS microphone  
  - 35–120 dB SPL range, 30 Hz–8 kHz frequency response  
  - ±2 dB accuracy  
  - 1.4 mA @ 3.3V (active), 90 µA (sleep)  
  - Supports A/C/Z weighting, adjustable averaging (fast: 125 ms, slow: 1 s)  
  - Bottom inlet port microphone  

#### Interfaces
- **USB-C** connector for power and DFU (Device Firmware Upgrade)  
- **CP2102** USB-to-UART converter  
- **U.FL** connectors for antennas (Terrestrial LoRa, GNSS)  
- **Grove-type** connectors for I²C and UART  
- **JST** connectors for LiPo battery and Solar Panel  
- **Header** for programming/debug  

#### Power Management
- **AEM10300** energy harvesting IC  
  - Supports solar panel input and MPPT (Maximum Power Point Tracking)  
  - Configurable VMPP/VOC ratio (60–90%) or ZMPP mode  
  - Programmable sampling duration and period  

- **TPS63001** buck-boost converter  
- **TPS22919** load switches  
- **NCP1117-3.3** LDO voltage regulator  

#### Power Sources
- USB 5V  
- Solar panel  
- Supercapacitor or LiPo battery  
- Optional 3.6V primary cell  

The board autonomously manages all inputs to maintain a regulated 3.3V system rail.

---

### 🌦️ Sensor Board

The **external sensor board** extends environmental monitoring through high-precision sensors connected via FPC (**FFC3B07 connector**).

#### Components
- **ZMOD4510** – outdoor air quality sensor  
  - Detects **NO₂** and **O₃** using AI-based signal processing  
  - I²C interface  
  - Includes heater driver for temperature compensation  

- **BME688** – 4-in-1 environmental sensor  
  - Measures **temperature**, **humidity**, **pressure**, and **gas** (VOCs, VSCs)  

- **SHTC3 (External)** – temperature and humidity  

- **TPS22919** load switches for controlled power domains  

---

## ⚙️ Key Active Components

| Category | Component | Description |
|-----------|------------|-------------|
| **Microcontrollers** | STM32WL5MOC | Dual-core (Cortex-M4/M0+), integrated SX1262 sub-GHz radio |
|  | STM32WB15 | MCU inside EM2050 module, controls LR1120 transceiver |
| **Radios** | Semtech SX1262 | Terrestrial LoRaWAN transceiver |
|  | Semtech LR1120 | Dual satellite/terrestrial transceiver (S-band + EU868/US915) |
| **Sensors** | SHTC3 | Temp & humidity, internal/external |
|  | ZMOD4510 | Air quality (NO₂, O₃) |
|  | BME688 | Environmental (gas, pressure, humidity, temperature) |
|  | IIS2MDC | 3-axis magnetometer |
|  | LC76G | GNSS (GPS, Beidou) |
|  | PCBA-DBM-PRO | Audio level sensor |
| **Memory** | W25Q16JV | 16 Mbit SPI NOR flash (FUOTA, logs) |
|  | MB85RC64TA | FRAM (64 kbit) non-volatile memory |
| **Power** | AEM10300 | Energy harvesting IC with MPPT |
|  | TPS63001 | Buck-boost converter |
|  | TPS22919 | Load switches |
|  | NCP1117-3.3 | LDO regulator |
| **Communication** | CP2102 | USB to UART bridge |

---

## 🌍 Design Features

### Dual LoRaWAN Support
- Operates on **terrestrial** and **satellite** LoRaWAN networks  
- Dual transceivers: SX1262 (terrestrial) and LR1120 (satellite)  
- Supports **S-band** for satellite and **868** for terrestrial bands  

### Flexible Architecture
- Modular main and sensor boards connected via FPC  
- U.FL connectors for external antennas  
- Compatible with multiple environmental sensors  

### Energy Autonomy
- Solar energy harvesting via AEM10300  
- Supports supercapacitor or LiPo storage  
- Multiple power sources with automatic selection  

### Low Power Design
- Ultra-low-power MCUs and sensors  
- Intelligent power domain switching (TPS22919)  
- Sleep and deep-sleep operation modes  

### Modularity and Extensibility
- Sensor board design allows expansion and upgrades  
- Ideal for future sensor integrations  

### Certification
- **EM2050** module certified for **ETSI**, **UKCA**, **FCC**, and **ISED** markets  

---

## 🚀 Applications

The uAir-Sat is suitable for:
- Remote asset tracking  
- Fixed environmental monitoring  
- Smart agriculture and forestry  
- Infrastructure telemetry  
- Low-maintenance, solar-powered IoT deployments  

---

## 🛰️ Summary

The **uAir-Sat** combines **dual-core processing**, **dual-band LoRaWAN**, and **multi-sensor environmental monitoring** in a compact, energy-autonomous platform.  
Its hybrid terrestrial and satellite communication capability enables reliable data transmission even in **remote, off-grid** environments.  

It stands as a versatile foundation for long-life IoT nodes requiring **resilience**, **low power**, and **connectivity beyond terrestrial networks**.

---

## Copyright & Licence
© 2025 Portugal Online & Antonio Alves – all rights reserved. Unless otherwise stated, you are hereby granted permission to use, study, modify and distribute the source files, design files and documentation for the uAir-Sat hardware under the terms of the CERN-OHL-P-2.0 licence. You must retain all copyright notices, acknowledgement of the original author, and a reference to this licence in any copy or derivative that you convey. The design is provided “as-is”, without warranty of any kind, and the original author disclaims any liability for how the design may be used or for any resulting product.

If you choose to manufacture, assemble or convey a product based on this design (or a derivative thereof), you remain responsible for ensuring compliance with applicable laws (including export control, radio-frequency, safety certification) and you must not imply endorsement by the original author or use the original author’s name or trademarks without explicit permission. Any modifications you make must be documented and attributed, and the licence text must remain included. By doing so you accept the terms of the CERN-OHL-P-2.0 licence worldwide and in perpetuity.
