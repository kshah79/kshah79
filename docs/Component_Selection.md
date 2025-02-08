# EGR 314 - Component Selection Report

## Name: Kevin Shah  
## Course: EGR 314  
## Assignment: Component Selection  

## Link to the Docs: [Click here](https://docs.google.com/document/d/1qigu1PS6sEkJsZvPCGCvZsLTG8wVTScNkyyonee9E0w/edit?usp=sharing)
---

## Introduction

This report includes multiple solutions for each major component of our design. I have considered and documented all the factors like cost, surface-mount compatibility, ease of integration, power efficiency, and performance. Each selected component is justified with a strong rationale based on the project requirements.

---

## Major Component Selection

### 2.1 Microcontroller Selection

The microcontroller is the most crucial part responsible for processing sensor data, managing communication, and executing the tasks between different modules and components.

#### Microcontroller Options

| Microcontroller            | Description                                            | Photo | Pros                                             | Cons                                             | Cost | Link      |
|----------------------------|--------------------------------------------------------|-------|--------------------------------------------------|--------------------------------------------------|------|-----------|
| **Microchip PIC18F47Q10**   | 8-bit microcontroller, provides UART interfaces, SMD Package | ![Microchip PIC18F47Q10](link_to_image) | Runs on low power, supports UART communication, suitable for modular system | Less processing power (Compared to 32-bit MCUs) | $0.00 | [Link](#) |
| **ESP32-WROOM-32**          | 32-bit MCU with Wi-Fi and Bluetooth                    | ![ESP32](link_to_image) | High performance, wireless communication        | Requires more power (compared to PIC)          | $0.00 | [Link](#) |
| **STM32F103C8T6**           | 32-bit ARM Cortex-M3                                   | ![STM32](link_to_image) | Most powerful, efficient, widely used in industry | Complex to program, additional cost            | $6.08 | [Link](#) |

#### Microcontroller Requirements and Pin Allocation

| Peripheral   | Requirement                                    |
|--------------|------------------------------------------------|
| **UART**     | Minimum 3 for communication between modules    |
| **I2C**      | Required for LCD and sensor communication      |
| **ADC**      | Required for water turbine sensor input        |
| **GPIO**     | Needed for additional controls and indicators  |
| **PWM**      | Needed for LED indicators or actuators         |
| **Power & GND** | Stable 3.3V and 5V power                    |

##### Optimal Choice:
The **PIC18F47Q10** was chosen for its efficient power consumption, UART communication support, and MPLAB X compatibility. Additionally, this component is offered in an SMD packaging, ensuring it adheres to EGR 314 specifications.

---

### 2.2 Wi-Fi Module Selection

Since our system required wireless communication, I evaluated multiple Wi-Fi enabled systems.

#### Wi-Fi Module Options

| Wi-Fi Module           | Description                                            | Photo | Pros                                             | Cons                                             | Cost   | Link   |
|------------------------|--------------------------------------------------------|-------|--------------------------------------------------|--------------------------------------------------|--------|--------|
| **ESP32-WROOM-32**      | Integrated Wi-Fi and Bluetooth, used in many IoT devices | ![ESP32-WROOM-32](link_to_image) | High performance, dual core, class-provided      | Requires custom PCB design                       | $0.00  | [Link](#) |
| **ESP32-ESP8266-12E-32**| Lower cost Wi-Fi module with UART support               | ![ESP8266](link_to_image) | High performance, power-efficient, low cost      | Limited flexibility                             | $10.00 | [Link](#) |
| **RN4020 Bluetooth**    | Bluetooth module                                       | ![RN4020](link_to_image) | Uses less power, cheaper than Wi-Fi options      | No Wi-Fi, limited communication options         | $11.61 | [Link](#) |

##### Optimal Choice:
**ESP32-WROOM-32** was selected due to its built-in Wi-Fi and compatibility with the system. Also, it is provided in the class, so no additional cost is required.

---

### 2.3 Temperature and Humidity Sensor Selection

The system required a reliable sensor to measure external conditions.

#### Sensor Options

| Sensor           | Description                                           | Photo | Pros                                               | Cons                            | Cost   | Link    |
|------------------|-------------------------------------------------------|-------|----------------------------------------------------|---------------------------------|--------|---------|
| **DHT11**        | Digital temperature and humidity sensor              | ![DHT11](link_to_image) | Low cost, easy to interface, widely available      | Less accurate than DHT22        | $7.00  | [Link](#) |
| **DHT22**        | Improved accuracy temperature and humidity sensor     | ![DHT22](link_to_image) | Better accuracy and temperature range             | Slightly more expensive         | $8.90  | [Link](#) |
| **SHT31**        | High-accuracy sensor with I2C communication          | ![SHT31](link_to_image) | Most accurate, fast response time                 | Most expensive                  | $9.70  | [Link](#) |

---

### 2.4 Water Flow Sensor Selection

For monitoring the flow of water, I compared several sensors.

#### Water Flow Sensor Options

| Sensor                | Description                      | Photo | Pros                                             | Cons                                          | Cost   | Link      |
|-----------------------|----------------------------------|-------|--------------------------------------------------|-----------------------------------------------|--------|-----------|
| **DFRobot SEN0229**    | 5V water turbine sensor          | ![DFRobot](link_to_image) | Compact, easy to interface                       | Requires 5V power                            | $8.16  | [Link](#) |
| **YF-S201**            | Hall effect-based water flow sensor | ![YF-S201](link_to_image) | Improved accuracy                                | Requires additional conditioning circuit      | $9.50  | [Link](#) |
| **FS300A**             | High precision industrial-grade sensor | ![FS300A](link_to_image) | Most accurate, fast response time               | Most expensive                               | $11.00 | [Link](#) |

##### Optimal Choice:
**DFRobot SEN0229** was selected due to its sufficient precision and ease of integration with the system. It supports SMD integration.

---

### 2.5 LCD Display Selection

For displaying real-time sensor data, we compared different LCD options.

#### LCD Display Options

| LCD Display            | Description                      | Photo | Pros                                             | Cons                                          | Cost   | Link    |
|------------------------|----------------------------------|-------|--------------------------------------------------|-----------------------------------------------|--------|---------|
| **East Rising ERM1602FS-6** | 16x2 character LCD with I2C interface | ![East Rising](link_to_image) | Low cost, easy interface                        | Requires external I2C module                   | $2.55  | [Link](#) |
| **HD44780 1602**       | Standard 16x2 character LCD      | ![HD44780](link_to_image) | Widely supported, cheap                         | Requires more GPIO pins                       | $3.00  | [Link](#) |
| **SSD1306 OLED**       | High-contrast OLED display       | ![SSD1306](link_to_image) | Compact, modern look                            | More expensive                                | $3.70  | [Link](#) |

##### Optimal Choice:
The **EastRising ERM1602FS-6** was chosen due to its alignment with our design and minimal power usage.

---

## ESP32 Table

#### ESP32-WROOM-32 Information

- Model: ESP32-DEVKITC-32UE
- Product Page: [Link](#)
- Datasheet: [Link](#)
- Technical Reference Manual: [Link](#)
- Unit Cost: $10.00
- Maximum Current: 500 mA

| Module               | Available | Needed | Associated Pins               |
|----------------------|-----------|--------|-------------------------------|
| **UART**             | 3         | 1      | GPIO1, GPIO3                  |
| **SPI**              | 3         | 1      | GPIO19, GPIO23, GPIO18, GPIO5 |
| **I2C**              | 2         | 1      | GPIO21, GPIO22                |
| **GPIO**             | 39        | Varies | Various                       |
| **ADC**              | 18        | 1      | GPIO34                        |


(Image can be viewed on the link, which is at the top of the document)
---

## Personal Responsibilities

As an essential team member, I oversee the management of the LCD module in our project. My responsibilities include ensuring effective communication between the microcontroller and the display, setting up the I2C interface, and developing firmware to display relevant data. Additionally, I manage the power requirements of the LCD module, ensuring it functions optimally within the system’s power constraints.

I also collaborate with other subsystems to ensure smooth data integration, troubleshoot signal integrity issues, and confirm the LCD's compatibility with the ESP32 microcontroller.

---

## Pinout Diagrams

### Microchip PIC18F47Q10 Pinout Diagram


(Image can be viewed on the link, which is at the top of the document)
---

### ESP32-WROOM-32 Pinout Diagram

(Image can be viewed on the link, which is at the top of the document)
---

### Conclusion

This report outlines the rationale behind the selection of key components for our design, focusing on factors such as performance, power efficiency, and compatibility. Each choice was made to meet the specific requirements of the project.

---

