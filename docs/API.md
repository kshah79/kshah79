# Subsystem: Human-Machine Interface (HMI)

The following messages represent communication between the HMI and other subsystems in the weather monitoring system. These structures are used within the class messaging protocol and occupy **bytes 4–61** of the packet. Framing bytes, sender, and receiver are handled outside of this message structure.

---

## Message Type 10 – Request Weather Data Refresh

**Direction:** Sent from HMI to Central Node  
**Purpose:** Request new data from sensors, triggered by user interaction.

| Byte(s) | Variable Name  | Data Type | Min Value | Max Value | Example |
|---------|----------------|-----------|-----------|-----------|---------|
| 1       | message_type   | uint8_t   | 10        | 10        | 10      |
| 2       | refresh_type   | uint8_t   | 0         | 3         | 1       |

**Explanation:**
- `refresh_type` values:
  - 0 → Full refresh
  - 1 → Temperature only
  - 2 → Humidity only
  - 3 → Pressure only

---

## Message Type 21 – Sensor Data Response

**Direction:** Received by HMI from Central Node  
**Purpose:** Display updated weather sensor data.

| Byte(s) | Variable Name | Data Type | Min Value | Max Value | Example |
|---------|----------------|-----------|-----------|-----------|---------|
| 1       | message_type   | uint8_t   | 21        | 21        | 21      |
| 2–3     | temperature    | int16_t   | -400      | 850       | 225     |
| 4–5     | humidity       | uint16_t  | 0         | 1000      | 450     |
| 6–7     | pressure       | uint16_t  | 800       | 1200      | 1013    |

**Notes:**
- `temperature`: tenths of degrees Celsius (e.g., 225 → 22.5°C)
- `humidity`: tenths of percent
- `pressure`: tenths of hPa

---

## Message Type 32 – System Status Broadcast

**Direction:** Broadcast from Central Node to all subsystems  
**Purpose:** Communicate overall system health status.

| Byte(s) | Variable Name | Data Type | Min Value | Max Value | Example |
|---------|----------------|-----------|-----------|-----------|---------|
| 1       | message_type   | uint8_t   | 32        | 32        | 32      |
| 2       | system_ok      | uint8_t   | 0         | 1         | 1       |

**Explanation:**
- `system_ok`:
  - 0 → System Error
  - 1 → System Operational

---

## Notes

- All messages listed are limited to the **message data area** (bytes 4–61) of the standard packet.
- Values are bounded to **C-compatible data types** to ensure consistency across MicroPython and C implementations.
- Any changes to these structures must be coordinated with team members, especially those responsible for the Central Node subsystem.
