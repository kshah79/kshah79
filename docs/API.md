# API Reference

This page documents the final communication interfaces for our Weather Monitoring & Actuator Control system. It covers both the on-board UART message protocol (between PCBs) and the MQTT-based cloud API. All definitions conform to the EGR 314 class specification and our team’s final message formats.

---

## 1. UART Message Protocol

### 1.1 Frame Structure  
Each inter-PCB message is a fixed 64-byte frame:

| Byte(s) | Field           | Description                                      |
|:-------:|-----------------|--------------------------------------------------|
| 0–1     | Header          | `0x41 0x5A`                                      |
| 2       | Source ID       | `uint8_t` (`‘E’=0x45`, `‘K’=0x4B`, `‘S’=0x53`, `‘T’=0x54`) |
| 3       | Destination ID  | `uint8_t`                                        |
| 4–5     | Message Type    | `uint16_t` (see types below)                     |
| 6–61    | Payload         | Up to 56 bytes of data (type-specific encoding)  |
| 62–63   | Footer          | `0x59 0x42`                                      |

### 1.2 Message Types & Payloads

1. **Set Water Flow**  
   - **Type:** `0x0001`  
   - **Payload (bytes 6–7):** `uint16_t` flow rate in L/min × 100 (e.g., 10.5 L/min → 1050).  
   - **Use:** ESP32 or Web → Motor board.

2. **Request Sensor Data**  
   - **Type:** `0x0002`  
   - **Payload (byte 6):** `uint8_t` sensor ID (1 = DHT11, 2 = BMP180).  
   - **Use:** ESP32 → Sensor board.

3. **Sensor Data Response**  
   - **Type:** `0x0003`  
   - **Payload:**  
     - Bytes 6–7: `uint16_t` temperature (°C × 100)  
     - Bytes 8–9: `uint16_t` humidity (% × 100)  
     - Bytes 10–11: `uint16_t` water flow (L/min × 100)  
   - **Use:** Sensor board → ESP32/HMI/Motor board.

4. **LCD Update Command**  
   - **Type:** `0x0004`  
   - **Payload (bytes 6–61):** ASCII text string (max 56 chars) to display, e.g.  
     ```
     Temp: 25.5°C, Hum: 50.1%, Flow: 10.5 L/min
     ```
   - **Use:** ESP32 → HMI board.

5. **Cloud Update**  
   - **Type:** `0x0005`  
   - **Payload (bytes 6–61):** ASCII-encoded JSON (max 56 bytes), e.g.:  
     ```json
     {"temp":2550,"hum":5010,"flow":1050}
     ```
   - **Use:** ESP32 → Cloud gateway.

6. **Error Message**  
   - **Type:** `0x0006`  
   - **Payload:**  
     - Byte 6: `uint8_t` subsystem ID (1 = PIC18, 2 = ESP32, 3 = Sensor, 4 = Pump, 5 = LCD)  
     - Bytes 7–62: ASCII error description (max 56 chars)  
   - **Use:** Any board → ESP32/HMI/Web for fault reporting.

---

## 2. MQTT Cloud API

### 2.1 Topic Structure  
| Topic                               | Direction    | Payload Format                        | Description                                            |
|-------------------------------------|--------------|---------------------------------------|--------------------------------------------------------|
| `egr314/teamxyz/request/data`       | Subscribed   | *empty*                               | Trigger on-demand sensor poll                           |
| `egr314/teamxyz/response/data`      | Published    | JSON: `{ "temp":<int>, "hum":<int>, "flow":<int> }` | Periodic or on-demand sensor readings (×100 scaling)   |
| `egr314/teamxyz/command/flow`       | Subscribed   | JSON: `{ "flow":<int> }`              | Remote pump on/off commands (L/min × 100)               |
| `egr314/teamxyz/status/motor`       | Published    | JSON: `{ "state":"on"|"off"|"rev" }`  | Acknowledge of motor state changes                      |
| `egr314/teamxyz/status/error`       | Published    | JSON: `{ "subsys":<int>, "msg":"<str>" }` | Error notifications with subsystem ID and message      |

### 2.2 Payload Field Definitions

- **temp**: Ambient temperature (°C × 100)  
- **hum**: Relative humidity (% × 100)  
- **flow**: Water flow rate (L/min × 100)  
- **state**: Motor status string (one of `"on"`, `"off"`, `"rev"`)  
- **subsys**: Subsystem ID matching UART Error Message IDs  
- **msg**: Human-readable error description  

### 2.3 Quality & Safety

- All JSON messages are published with QoS 1 to ensure reliable delivery.  
- Retained flag is **off** for request/command topics and **on** for status topics to support late-joining clients.  
- Error topics use a distinct namespace (`status/error`) to allow dashboard widgets to filter only fault conditions.

---

> **Note:** All identifiers and topic names use **lowercase** and **no spaces**, per the class naming conventions. Scaling factors (×100) ensure integer-only encoding on resource-constrained MCUs.
