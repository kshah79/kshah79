# EGR 314 - Component Selection Report  

## Name: Kevin Shah  
## Course: EGR 314  
## Assignment: Component Selection  

📄 **[View Full Report](https://docs.google.com/document/d/16eBhtJ1a93Trgb88zd__rfECLNrKxZGtmtAWUIEOJiY/edit?usp=sharing)**  

---

## Introduction  

This report presents multiple solutions for each major component of our design. Factors such as **cost, surface-mount compatibility, ease of integration, power efficiency, and performance** have been considered and documented. Each selected component is justified based on the project requirements.  

---

## Major Component Selection  

### 🔹 2.1 Microcontroller Selection  

The **microcontroller** is the core of our system, responsible for **processing sensor data, managing communication, and executing tasks between modules**.  

#### **Microcontroller Options**  

| Microcontroller           | Description                                      | Image  | Pros                                            | Cons                                          | Cost  | Link   |
|---------------------------|--------------------------------------------------|--------|-------------------------------------------------|-----------------------------------------------|------|--------|
| **Microchip PIC18F47Q10**  | 8-bit MCU, UART interfaces, SMD Package         | ![PIC18F47Q10](PIC.jpeg) | Low power, UART support, modular design       | Less processing power (compared to 32-bit)   | $0.00 | [Link](#) |
| **ESP32-WROOM-32**         | 32-bit MCU with Wi-Fi/Bluetooth                 | ![ESP32](esp32.jpeg) | High performance, wireless communication      | Higher power consumption                     | $0.00 | [Link](#) |
| **STM32F103C8T6**          | 32-bit ARM Cortex-M3                            | ![STM32](STM32.jpeg) | Most powerful, widely used in industry        | Complex to program, higher cost              | $6.08 | [Link](#) |

##### ✅ **Optimal Choice:**  
The **ESP32** was chosen for its **efficient power usage, UART support, and MPLAB X compatibility**. Additionally, its **SMD packaging** adheres to EGR 314 specifications.  

---

### 🔹 2.5 LCD Display Selection  

#### **LCD Display Options**  

| LCD Display        | Description                                    | Image  | Pros                              | Cons                                | Cost  | Link  |
|--------------------|----------------------------------------------|--------|---------------------------------|---------------------------------|------|-------|
| **ILI9341**        | 2.4-inch TFT LCD with SPI interface          | ![ILI9341](ILI9341.jpeg) | High resolution (240x320), full-color, SPI interface | Higher power consumption compared to monochrome displays | $13.99 | [Link](https://www.amazon.com/DIANN-ILI9341-Display-320x240-Screen/dp/B0BNQD38T2) |
| **ST7789**        | 1.3-inch TFT LCD with SPI interface          | ![ST7789](ST7789.jpeg) | High contrast, compact size, supports SPI | Small display size | $10.50 | [Link](https://www.adafruit.com/product/4313) |
| **Nextion NX3224T028** | 2.8-inch intelligent TFT touchscreen      | ![Nextion](Nextion.jpeg) | Touchscreen support, easy UI development, built-in processor | Higher cost, requires Nextion Editor software | $25.00 | [Link](https://www.itead.cc/nextion-nx3224t028.html) |

##### ✅ **Optimal Choice:**  
The **ILI9341** was selected for its **high resolution, color display, and SPI interface**, making it suitable for applications requiring detailed visual output.

---

## 🔹 ESP32 Table  

- **Model:** ESP32-DEVKITC-32UE  
- **Max Current:** 500 mA  

| Module   | Available | Needed | Associated Pins       |
|----------|-----------|--------|-----------------------|
| **UART**  | 3         | 1      | GPIO1, GPIO3          |
| **SPI**   | 3         | 1      | GPIO19, GPIO23, GPIO18, GPIO5 |
| **I2C**   | 2         | 1      | GPIO21, GPIO22        |

---

## 📌 Personal Responsibilities  

I am responsible for **managing the LCD module, ensuring integration with the microcontroller, setting up the SPI interface, and optimizing power usage**.  

Additionally, I work with other subsystems to ensure **data integration, signal integrity, and firmware compatibility**.  

---

## 📌 Conclusion  

This report provides the **rationale behind key component selections** based on **performance, cost, efficiency, and compatibility**. The inclusion of the **ILI9341** LCD display enhances our system's capability to provide detailed and colorful visual feedback, aligning with the project's objectives.  

---


