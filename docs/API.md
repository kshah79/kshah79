# 📡 API – HMI Subsystem (ESP32)

## 🔍 Subsystem Role
The HMI Subsystem, based on an ESP32 module, serves as the primary interface between the user and the full embedded system. It:
- Initiates control messages (e.g., motor speed).
- Requests and processes sensor data.
- Displays real-time information on a local LCD via SPI.
- Publishes environmental data to a cloud server using Wi-Fi (MQTT).
- Handles error reporting across the system.

## 🔄 Message Exchange Summary

| Direction  | Message Type | Description              | Communication | Status |
|------------|--------------|--------------------------|---------------|--------|
| Sent       | 1            | Set System Speed         | UART          | ✅      |
| Sent       | 2            | Request Sensor Data      | UART          | ✅      |
| Sent       | 5            | Cloud Update (JSON)      | Wi-Fi         | ✅      |
| Received   | 3            | Sensor Data Response     | UART          | ✅      |
| Received   | 6            | Error Message            | UART          | ✅      |
| Internal   | —            | LCD Display Update (via SPI) | SPI (Local) | ✅      |

✅ All UART messages conform to the project-wide packet structure (Bytes 0–63) with the message payload occupying Bytes 4–61.

## 📨 Messages Sent (UART)

### Message Type 1 — Set System Speed
| Byte(s)  | Variable Name | Data Type | Min  | Max  | Example |
|----------|---------------|-----------|------|------|---------|
| 1–2      | msg_type      | uint16_t  | 1    | 1    | 1       |
| 3–4      | speed_val     | uint16_t  | 0    | 1000 | 200     |

- **Purpose**: Sends speed command to Motor Driver Subsystem.
- **Recipient**: PIC18F47Q10 (Motor Driver).
- **Notes**: Valid speed range is 0–1000 (mm/s). Messages outside this range are not transmitted.

### Message Type 2 — Request Sensor Data
| Byte(s)  | Variable Name | Data Type | Min  | Max  | Example |
|----------|---------------|-----------|------|------|---------|
| 1–2      | msg_type      | uint16_t  | 2    | 2    | 2       |
| 3        | sensor_id     | uint8_t   | 1    | 2    | 1       |

- **Purpose**: Requests environmental data from Sensor Subsystem.
- **Recipient**: PIC18F47Q10 (Sensor Subsystem).
- **Notes**: sensor_id 1 = DHT11, 2 = SEN0229. Any other ID is ignored.

### Message Type 5 — Cloud Update
| Byte(s)  | Variable Name | Data Type | Min  | Max  | Example |
|----------|---------------|-----------|------|------|---------|
| 1–2      | msg_type      | uint16_t  | 5    | 5    | 5       |
| 3–58     | json_data     | char[55]  | —    | —    | '{"T":25.5,"H":40,"F":10.2}' |

- **Purpose**: Publishes sensor data to MQTT cloud server.
- **Transmission**: Via Wi-Fi (ESP32 client).
- **Notes**: Message body must be valid JSON. Always ≤ 55 chars.

## 📥 Messages Received (UART)

### Message Type 3 — Sensor Data Response
| Byte(s)  | Variable Name  | Data Type | Min  | Max   | Example |
|----------|----------------|-----------|------|-------|---------|
| 1–2      | msg_type       | uint16_t  | 3    | 3     | 3       |
| 3–4      | temperature    | uint16_t  | 0    | 5000  | 2550    |
| 5–6      | humidity       | uint16_t  | 0    | 10000 | 5010    |
| 7–8      | flow_rate      | uint16_t  | 0    | 3000  | 1050    |

- **Purpose**: Receives environment metrics for display and upload.
- **Sender**: Sensor Subsystem (PIC18F47Q10).
- **Notes**: Fixed-point encoding ×100. Parsed, validated, then used for display and cloud publishing.

### Message Type 6 — Error Message
| Byte(s)  | Variable Name  | Data Type | Min  | Max   | Example               |
|----------|----------------|-----------|------|-------|-----------------------|
| 1–2      | msg_type       | uint16_t  | 6    | 6     | 6                     |
| 3        | subsystem_id   | uint8_t   | 1    | 5     | 2                     |
| 4–58     | error_msg      | char[55]  | —    | —     | "Wi-Fi Timeout Retry"  |

- **Purpose**: Reports internal/system-wide errors.
- **Sender**: Any subsystem.
- **Notes**: Displayed on screen if HMI is relevant; otherwise logged to serial console.

## 📺 Internal SPI Message (Not UART)

### LCD Display Update (Local Only)
| Channel | Protocol | Description                          |
|---------|----------|--------------------------------------|
| SPI     | ILI9341  | Displays combined system data       |

- **Inputs**: Parsed data from Messages 1, 3, and 6.
- **Output**: Human-readable string sent via `SPI.write()` to ILI9341.

**Example Display**:
```text
Speed: 200 mm/s
Temp: 25.5°C
Humidity: 50.1%
Flow: 10.5 L/min
