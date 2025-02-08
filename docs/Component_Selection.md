Name: Kevin Shah	
Course: EGR 314
Assignment: Component Selection


Introduction:
This report includes multiple solutions for each major component of our design. I have considered and documented all the factors like cost, surface-mount compatibility, ease of integration, power efficiency and performance. Each selected component is justified with a strong rationale based on the project requirements.

Major Component Selection

2.1 Microcontroller Selection: The microcontroller is the most crucial part responsible for processing sensor data and managing communication and executing the tasks between different modules and components.


Microcontroller
Description
Photo
Pros
Cons
Cost
Link
Microchip PIC18F47Q10
8 Bit microcontroller, provides UART interfaces, SMD Package

Runs on low power, supports UART Communication, suitable for our modular system
Less processing power (Compared to 32 bit MCUs)
$0.00 
Link
ESP32-WROOM-32
32 Bit MCU with Wi-Fi and Bluetooth power

High performance, and wireless communication
Requires more power (compared to microchip PIC)
$0.00
Link
STM32F103C8T6
32 Bit ARM Cortex - M3

Most powerful than the other 2, efficient and widely used in industry appliances
Complex to program and will require additional cost to purchase as it is not provided in the class
$6.08
Link


Microcontroller Requirements and Pin Allocation


Peripheral
Requirement
UART
Minimum 3 as it is required for communication between modules
I2C
Required for LCD and sensor Communication
ADC
Required for water turbine sensor input
GPIO
Needed for additional controls and indicators
PWM
Needed for LED indicators or actuators
Power and GND
Stable 3.3V and 5V power 



Optimal Choice: The PIC18F47Q10 was chosen because of its efficient power consumption, UART communication requirements, and MPLAB X compatibility. Additionally, this component is offered in an SMD packaging, guaranteeing adherence to EGR 314 specifications.

2.2 Wi-Fi Module: Since our system required wireless communication, I have evaluated multiple Wi-Fi enabled systems.


Wi_Fi Module
Description
Photo
Pros
Cons
Cost
Link
ESP32-WROOM-32
Integrated Wi-Fi and Bluetooth module which is used in many IOT appliances

High performance, Dual Core. Provided in the class
Requires custom PCB design with respect to our system.
$0.00 
Link
ESP32-ESP8266-12E-32
Lower cost WiFi module with UART Support

High performance, and wireless communication
Power efficiency, low cost
$10.00
Link
RN4020 Bluetooth module
Bluetooth module

Uses less power to run (compared to other two)
No WiFi, limited flexibility
$11.61
Link


ESP32 pinout and Allocation:

Peripheral
ESP32 Pin
UART RX/TX
GPIO1, GPIO3
I2C SDA, SCL
GPIO21, GPIO22
SPI
GPIO5, GPIO18, GPIO19, GPIO23
ADC Input
GPIO34
PWM Output
GPIO25
Power And Ground
3.3V, GND



Optimal Choice: ESP32-WROOM-32 is selected due to its built-in Wi-Fi and compatibility with the system. Also it has been provided in the class so no additional cost to purchase it. This module will directly be integrated onto the PCB in an SMD format.

2.3 Temperature and Humidity Sensor: The system required a reliable sensor to measure the external conditions.


Sensor
Description
Photo
Pros
Cons
Cost
Link
DHT11
Digital Temperature and humidity sensor

Low cost, easy interface and widely available
Less accurate than DHT22
$7.00 
Link
DHT22
Higher precision than DHT11, and a better temperature range.

Improved accuracy
Slightly more expensive
$8.90
Link
SHT31
High-accuracy sensor with I2C Communication

Most accurate among the others and fast response time
Most expensive.
$9.70
Link


2.4 Water Flow Sensor Selection: To monitor the flow of water.

Sensor
Description
Photo
Pros
Cons
Cost
Link
DFRobot SEN0229
5V Water turbine sensor

Compact, Easy to interface
Requires 5V power source
$8.16 
Link
YF-S201
Hall effect-based water flow 

Improved accuracy
Required additional conditioning circuit
$9.50
Link
FS300A
High precision industrial grade sensor

Most accurate among the others and fast response time
Most expensive.
$11.00
Link


Optimal Choice: DFRobot SEN0229 was chosen because of has sufficient precision and simplicity of integration with our system. Integration that is compatible with SMD must be supported by the design.

2.5 LCD Display Selection: To display real time sensor data, we compared different LCD options.


LCD Display
Description
Photo
Pros
Cons
Cost
Link
East Rising ERM1602FS-6
16*2 character LCD with I2C interface

Low cost, easy interface and widely available
Requires external I2C module
$2.55 
Link
HD44780 1602
Standard 16*2 character LCD

Widely supported
Requires more GPIO pins
$3.00
Link
SSD1306 OLED
High-contrast OLED Display

Compact and modern
More expensive
$3.70
Link


Optimal Choice:  EastRising ERM1602FS-6 – Chosen due to its alignment with our design and minimal power usage. The screen needs to be connected directly without utilizing a pre-assembled daughterboard.

ESP32 Table


ESP 32 Info
ESP32-WROOM-32
Model
ESP32-DEVKITC-32UE
Product Page URL
Product Page Link
ESP32-WROOM-32 Datasheet URL
Link
ESP32-WROOM-32 Technical Reference Manual URL
Technical Reference manual link
Vendor Link
Vendor Link
Code examples
Code Example Link
External Resources URL
Eternal Resource from Arduino Link
Unit Cost
$10.00
Absolute Maximum Current for entire IC
500 mA
Supply Voltage Range
Min. 2.3V/ Nominal 3.3V / Max. 3.6V
Maximum GPIO Current (per Pin)
40mA
Supports External Interrupts ?
Yes
Required Programming Hardware, cost, and URL
USB to Serial Adapter



Module
# Available
Needed
Associated Pins
UART
3
1
GPIO1 / GPIO3
SPI
3
1
GPIO19 / GPIO23 / GPIO18 / GPIO5
I2C
2
1
GPIO21 / GPIO22
GPIO
39
Varies
*
ADC
18
1
GPIO34
LED PWM
Multiple
1
GPIO25
Motor PWM
Multiple
1
*
USB Programmer
1
1
USB D+ / USB D-


Parametric Result for ESP32-WROOM-32





PIC Table

PIC Info
Microchip PIC18F47Q10
Model
PIC18F47Q10
Product Page URL
Product Page Link
Datasheet URL(s)
Datasheet Link
Application Notes URL(s)
Application Notes Link
Vendor link
Vendor Link
Code Examples
Microchip MPLAB Examples
External Resources URL(s)
PIC18 Tutorials on YouTube
Unit cost
$15.30
Absolute Maximum Current for entire IC
250 mA
Supply Voltage Range
Min: 1.8V / Nominal: 3.3V / Max: 5.5V
Maximum GPIO current (per pin)
25 mA
Supports External Interrupts?
Yes
Required Programming Hardware, Cost, URL
PICkit4 (~$50)





Module
# Available
Needed
Associated Pins
GPIO
36
Varies
Varies
ADC
14
1
ANx Pins
UART
2
1
TX/RX Pins
SPI
1
1
SCK / MOSI / MISO
I2C
1
1
SDA / SCL
PWM
Multiple
1
SPI
ICSP
1
1
PGD / PGC


Link of the parametric Result for PIC18F47Q10


Personal Responsibilities: 
As an essential team member, I oversee the management of the LCD module in our project. My duties involve maintaining effective communication between the microcontroller and the display, setting up the I2C interface, and developing firmware to present pertinent data. Furthermore, I will manage the power needs for the LCD module, making sure it functions effectively within the system's limitations.

In addition to the display, I am also tasked with working alongside other subsystems to guarantee smooth data integration. This involves troubleshooting signal integrity problems, enhancing refresh rates for improved clarity, and confirming compatibility with the ESP32 microcontroller. I will additionally participate in testing and verifying the LCD's performance in various environmental conditions, guaranteeing reliability and responsiveness in the final product.

MPLABX Project for PIC18F47Q10








Entire Image




The Microchip PIC18F47Q10 is a 40-pin microcontroller crafted for efficient, low-power performance applications. 
The left portion of the pinout features analog and digital I/O ports, including RA0-RA7, RE0-RE2, and RC0-RC3, which can be set up for multiple roles such as ADC, GPIO, and communication interfaces. 
The right side includes RB0-RB7 and RD0-RD7, offering extra GPIO, PWM, and peripheral connectivity choices. 
VDD and VSS provide power and ground connections for the microcontroller. 
The MCLR (pin 1) serves the purpose of external reset functionality. 
This pin configuration provides effective data handling, energy distribution, and communication functionalities, making it a perfect option for embedded systems that need UART, SPI, I2C, ADC, and PWM integration.


Pinout Diagram for ESP32-WROOM-32

Peripheral Pin Allocation For ESP32-WROOM-32


Peripheral
Assigned Pin(s)
Notes
Power (3.3V)
3V3
Main power supply
Ground (GND)
GND
Ground connection
UART TX
GPIO1
Transmit Data (TX)
UART RX
GPIO3
Receive Data (RX)
I2C SDA
GPIO21
Data Line for I2C
I2C SCL
GPIO22
Clock Line for I2C
SPI MISO
GPIO19
Master In Slave Out
SPI MOSI
GPIO23
Master Out Slave In
SPI CLK
GPIO18
Clock Line for SPI
SPI CS
GPIO5
Chip Select for SPI
ADC Input
GPIO34
Used for analog sensor input
DAC Output
GPIO25
Digital-to-Analog output
PWM Output (LED)
GPIO26
PWM signal for LEDs
PWM Output (Motor)
GPIO27
PWM signal for motor control



The ESP32-WROOM-32 microcontroller needs precise pin assignment to guarantee effective communication and operation among all peripherals. 
The 3.3V (3V3) power supply delivers the required operating voltage, whereas the GND pin acts as the ground reference. 
UART communication uses GPIO1 (TX) and GPIO3 (RX) to enable serial data transmission.
In I2C communication, GPIO21 (SDA) and GPIO22 (SCL) manage data transfer and clock synchronization. 
SPI communication, crucial for fast data transfers, employs GPIO19 (MISO), GPIO23 (MOSI), GPIO18 (CLK), and GPIO5 (CS) for master-slave communication. 
The analog input (ADC) is controlled by GPIO34, facilitating sensor connections, whereas GPIO25 (DAC Output) offers digital-to-analog conversion capabilities for audio and additional analog uses. 
PWM outputs, such as GPIO26 (for LED control) and GPIO27 (for motor control), provide seamless modulation for lighting and activation functions. 
This pin layout enhances the ESP32's functionality, guaranteeing effective power management, smooth communication, and strong peripheral support for the project.


