---

## Introduction  
This section documents all major components chosen for our final design. New parts considered or added since the last draft are highlighted, and each choice is justified against our product requirements for performance, power, cost, and ease of integration.

---

## 1. Major Component Selection  

### 1.1 Summary of Final Major Components  
| Component               | Part Number / Model    | Qty | Primary Role                                   |
|-------------------------|------------------------|-----|------------------------------------------------|
| Microcontroller (Wi-Fi) | ESP32-WROOM-32         | 1   | Cloud & HMI controller, UART bridge            |
| Microcontroller (Core)  | PIC18F47Q10            | 1   | Sensor polling, I²C master, daisy-chain router |
| TFT Display             | ILI9341 (2.4″ SPI)     | 1   | Local weather & actuator status                |
| Temp/Humidity Sensor    | DHT11                  | 1   | Basic environmental sensing                    |
| Pressure Sensor         | BMP180                 | 1   | Barometric pressure & temperature backup       |
| Water Flow Sensor       | SEN0229                | 1   | Measures chilled-water flow rate               |
| Motor Driver            | IFX92015GAUMA1         | 1   | Actuator control (pump/motor)                  |
| Voltage Regulator       | AP63203WU-7            | 1   | 9 V → 3.3 V switching regulator                 |

> *Note: Excludes passives, connectors, and push-buttons.*

---

### 1.2 Expanded Component Options & Feedback Addressed  

| Category         | New Candidates & Feedback              | Notes on Selection                                  |
|------------------|----------------------------------------|------------------------------------------------------|
| Microcontroller  | • STM32F103C8T6 (rejected)<br>• RP2040 (considered) | 32-bit alternatives had higher cost or lacked Wi-Fi. ESP32’s integrated radio and sufficient RAM won out. |
| Sensors          | • BME280 (rejected)<br>• SHT31 (considered)          | BMP180 added to improve pressure accuracy; DHT11 retained for simplicity and cost. |
| Display          | • SSD1289 3.5″ TFT (rejected)<br>• Nextion HMI (considered) | ILI9341 chosen for SPI simplicity, open-source libraries, and compact SMD modules. |
| Regulator        | • LM1117 (considered)<br>• TPS62840 (considered)     | AP63203WU-7 selected for 3 A capability and high efficiency at our 0.5 A load. |
| Motor Driver     | • L293D (considered)<br>• DRV8833 (considered)       | IFX92015 chosen for its integrated protection features and single-package SMD form factor. |

> **Feedback Incorporated:**  
> - Added BMP180 to address TA suggestion for supporting barometric data.  
> - Included IFX92015GAUMA1 motor driver per lab-demo reliability concerns.

---

## 2. Microcontroller Pinout & Configuration

### 2.1 ESP32-DEVKITC-32UE Pinout  
| Subsystem          | Function                   | Pins Used       |
|--------------------|----------------------------|-----------------|
| **UART (Daisy-Chain)**   | RX / TX                    | GPIO36 (RX), GPIO37 (TX) |
| **SPI (ILI9341)**  | SCK / MOSI / CS / DC / RST | GPIO18, 23, 5, 2, 4    |
| **I²C**            | SDA / SCL                  | GPIO21, 22      |
| **GPIO (Buttons)** | Refresh / Override Flow    | GPIO17, GPIO16  |
| **Power**          | 3.3 V / GND                | —               |
| **Wi-Fi/BT**       | Internal antenna           | —               |

### 2.2 PIC18F47Q10 MCC Configuration  
| Peripheral     | Module        | Pin (RAx/RBx/…) | Configuration Notes                    |
|----------------|---------------|-----------------|----------------------------------------|
| **UART1**      | TX1 / RX1     | RC6 / RC7       | 115 200 baud, 8N1; Daisy-chain comms   |
| **I²C1**       | SDA1 / SCL1   | RC4 / RC3       | Master for DHT11 / BMP180              |
| **SPI**        | SCK / SDI / SDO | RB6 / RB5 / RB7 | Driving ILI9341 via SPI (for Kevin’s board) |
| **CTMU**       | Touch sense   | —               | Unused                                  |
| **ADCs**       | AN0…AN12      | Various         | Unused (except pin-monitoring)         |
| **Power**      | VDD / VSS     | VDD / VSS       | 3.3 V supply from AP63203WU-7          |

> *All pins now match the final schematic and reflect lab‐demo corrections.*

---

## 3. Decision-Making Process  

In assembling this section, I:

1. **Reviewed Product Requirements**  
   - Real-time updates (< 500 ms), low-power, SMD form-factor, educational clarity.

2. **Cataloged Candidate Parts**  
   - Added BMP180 and RP2040 to the list after in-person feedback; discarded options that failed cost or integration checks.

3. **Balanced Trade-Offs**  
   - Prioritized built-in wireless (ESP32) over external modules, and high-efficiency switching regulators over linear solutions.

4. **Aligned with Team Roles**  
   - Ensured each component fit the PCB footprint and could be tested independently by its owner (Kevin → ILI9341 & ESP32).

5. **Validated in Lab Demos**  
   - Verified actual current draw, SPI timings, and I²C reliability before finalizing.

> **Outcome:** Our selected parts meet all electrical, mechanical, and pedagogical goals—and simplify assembly for the Innovation Showcase.

---

## 4. Power Budget  

| Component                | V<sub>oper</sub> (V) | I<sub>avg</sub> (mA) | Power (mW)  |
|--------------------------|----------------------|----------------------|-------------|
| ESP32-WROOM-32           | 3.3                  | 200                  | 660         |
| PIC18F47Q10              | 3.3                  | 15                   | 49.5        |
| ILI9341 TFT Display      | 3.3                  | 80                   | 264         |
| DHT11                    | 3.3                  | 2.5                  | 8.25        |
| BMP180                   | 3.3                  | 0.01                 | 0.033       |
| SEN0229 Flow Sensor      | 5.0                  | 15                   | 75          |
| IFX92015GAUMA1 Driver    | 3.3                  | 5                    | 16.5        |
| **Subtotal (Logic)**     | —                    | —                    | **997.3**   |
| Water Pump (Actuator)    | 9.0                  | 100                  | 900         |
| **Total System Load**    | —                    | —                    | **1897.3**  |
| **Regulator Overhead**   | —                    | —                    | ~~5 % loss~~ ≈ 95     |
| **Grand Total Draw**     | —                    | —                    | **≈ 1992 mW** |

> *Template adapted from original assignment.*

### 4.1 Power Budget Analysis  
- I estimated **average currents** from datasheets and lab measurements.  
- **Regulator overhead** (5 %) accounts for DC/DC conversion losses.  
- **Conclusion:** A single AP63203WU-7 (3 A @ 3.3 V) comfortably supplies the 1 A logic load with margin, and our 9 V supply (rated 1 A) supports the 100 mA pump plus overhead.  
- **Implication:** No additional heat sinks or power modules are required, and the system operates within safety and efficiency targets.

### 4.2 Using the Power Budget to Estimate Needs & Key Conclusions

To build the power budget, I followed these steps:

1. **Gathered Current Draw Data**  
   - **Datasheet Values:** I extracted typical and active‐mode currents from each major component’s datasheet (e.g., ESP32-WROOM-32: 20 mA idle, 240 mA peak during Wi-Fi TX).  
   - **Lab Measurements:** For dynamic loads (ILI9341 backlight, water pump), I measured currents under representative operating conditions, then averaged them over a duty cycle.

2. **Calculated Per-Component Power**  
   - For each device, I multiplied its average current (I<sub>avg</sub>) by its operating voltage (V<sub>oper</sub>) to get power in milliwatts (P = V·I).  
   - Example: ESP32 at 3.3 V × 200 mA → 660 mW.

3. **Summed Logic & Actuator Loads Separately**  
   - **Logic Subsystem:** ESP32, PIC18F, sensors, display, and driver, totaled ~997 mW.  
   - **Actuator (Pump):** 9 V × 100 mA → 900 mW.

4. **Added Regulator Overhead**  
   - Based on the AP63203WU-7 efficiency curve, I assumed a conservative 5 % conversion loss.  
   - Overhead = (Logic Subtotal + Actuator Load) × 0.05 ≈ 95 mW.

5. **Checked Supply Ratings & Margin**  
   - **3.3 V Rail:** AP63203WU-7 rated for 3 A (≈10 W) — far above our ~300 mA (1 W) requirement, providing >10× margin.  
   - **9 V Source:** Rated at 1 A (9 W), comfortably handles the 1.9 W system load with room for sensor bursts and startup surges.

#### Key Conclusions

- **Regulator Choice Validated:** The AP63203WU-7’s headroom ensures minimal thermal stress even under continuous Wi-Fi and backlight use.  
- **Single Supply Sufficiency:** A single 9 V, 1 A adapter supports both logic and pump, simplifying the power architecture and reducing part count.  
- **Thermal & Safety Margins:** With >50 % headroom on both rails, the design remains cool under load and tolerates component variances or aging.  
- **Scalability:** The ample margin allows future additions (extra sensors, LEDs) without reworking the power system.

---
