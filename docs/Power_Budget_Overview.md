# Power Budget Calculation ⚡  

Below is the **power budget analysis** for our system, detailing voltage, current, and power consumption for each component.  

## Component Power Breakdown  

| **Component**                  | **Voltage (V)**              | **Current (A)**        | **Power (W) = V × A** |
|--------------------------------|------------------------------|------------------------|------------------------|
| **TFT LCD (320x240)**          | 3.3V                         | 0.12A                  | 0.396W                 |
| **ESP32-S3-WROOM-1-N4**        | 3.3V                         | 0.24A                  | 0.792W                 |
| **AP63203WU-7 (Buck Reg.)**    | 12V input → 3.3V output      | 0.3A*                  | 1.188W                 |
| **LED Debug (2x LEDs)**        | 3.3V                         | 0.02A (each)           | 0.132W                 |
| **10µF Capacitor (C1)**        | Passive                      | Negligible             | 0W                     |
| **10µF 50V Capacitor (C2)**    | Passive                      | Negligible             | 0W                     |
| **0.1µF 25V Capacitor (C3)**   | Passive                      | Negligible             | 0W                     |
| **2×22µF Capacitor (C4)**      | Passive                      | Negligible             | 0W                     |
| **Inductor (3.9µH, L1)**       | Passive                      | Negligible             | 0W                     |
| **Resistor 10kΩ (R1, R2)**     | 3.3V                         | 0.33mA (each)          | 0.002W                 |
| **Resistor 220kΩ (R3, R4)**    | 3.3V                         | 0.015mA (each)         | 0.00005W               |
| **Fuse Block (02981001ZXT)**   | 32V Max                      | Passive                | 0W (unless tripped)    |
| **Through Hole Barrel Jack     | 12V Input                    | -                      | 0W (Power Input)       |
| **Jumpers (J1, J2)**           | -                            | Passive                | 0W                     |
| **DOWNSTREAM Connector (P1)**  | -                            | -                      | 0W                     |
| **UPSTREAM Connector (P2)**    | -                            | -                      | 0W                     |

## **Total Power Consumption**  

### **At 3.3V Load:**  
- **Total Current Draw:** ~0.68A  
- **Total Power (3.3V Side):** ~2.24W  

### **At 12V Input (Before Conversion Losses):**  
- **Buck Converter Efficiency (~85%)**  
- **Total Input Power:** ~2.64W  
- **Total Input Current (12V Side):** **0.22A**  

---

This power budget ensures efficient power distribution and conversion across all components. Feel free to review and provide feedback! 🚀  
