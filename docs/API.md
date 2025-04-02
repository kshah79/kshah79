<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>API – HMI Subsystem</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 2rem; line-height: 1.6; }
    h1, h2, h3 { color: #2a4d69; }
    table { border-collapse: collapse; width: 100%; margin-bottom: 2rem; }
    th, td { border: 1px solid #ccc; padding: 8px; text-align: center; }
    th { background-color: #f4f4f4; }
    code { background: #f0f0f0; padding: 2px 4px; border-radius: 4px; }
  </style>
</head>
<body>

  <h1>HMI Subsystem API</h1>
  <p>This page documents the messages sent and received by the HMI subsystem as part of the modular weather monitoring system.</p>

  <h2>Message Type 10 — Request Weather Data Refresh</h2>
  <table>
    <thead>
      <tr>
        <th>Byte</th><th>Variable Name</th><th>Data Type</th><th>Min Value</th><th>Max Value</th><th>Example</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>1</td><td>message_type</td><td><code>uint8_t</code></td><td>10</td><td>10</td><td>10</td></tr>
      <tr><td>2</td><td>refresh_type</td><td><code>uint8_t</code></td><td>0</td><td>3</td><td>1</td></tr>
    </tbody>
  </table>
  <p><strong>Purpose:</strong> Sent from HMI to Central Node to request updated sensor data.</p>
  <ul>
    <li>0 = Full refresh</li>
    <li>1 = Temperature only</li>
    <li>2 = Humidity only</li>
    <li>3 = Pressure only</li>
  </ul>

  <h2>Message Type 21 — Sensor Data (to HMI)</h2>
  <table>
    <thead>
      <tr>
        <th>Byte</th><th>Variable Name</th><th>Data Type</th><th>Min Value</th><th>Max Value</th><th>Example</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>1</td><td>message_type</td><td><code>uint8_t</code></td><td>21</td><td>21</td><td>21</td></tr>
      <tr><td>2-3</td><td>temperature</td><td><code>int16_t</code></td><td>-400</td><td>850</td><td>225</td></tr>
      <tr><td>4-5</td><td>humidity</td><td><code>uint16_t</code></td><td>0</td><td>1000</td><td>450</td></tr>
      <tr><td>6-7</td><td>pressure</td><td><code>uint16_t</code></td><td>800</td><td>1200</td><td>1013</td></tr>
    </tbody>
  </table>
  <p><strong>Purpose:</strong> Sent from Central Node to HMI to update displayed data.</p>
  <p><strong>Units:</strong></p>
  <ul>
    <li>Temperature in tenths of °C (e.g., 225 = 22.5°C)</li>
    <li>Humidity in tenths of %</li>
    <li>Pressure in tenths of hPa</li>
  </ul>

  <h2>Message Type 32 — System Status Broadcast</h2>
  <table>
    <thead>
      <tr>
        <th>Byte</th><th>Variable Name</th><th>Data Type</th><th>Min Value</th><th>Max Value</th><th>Example</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>1</td><td>message_type</td><td><code>uint8_t</code></td><td>32</td><td>32</td><td>32</td></tr>
      <tr><td>2</td><td>system_ok</td><td><code>uint8_t</code></td><td>0</td><td>1</td><td>1</td></tr>
    </tbody>
  </table>
  <p><strong>Purpose:</strong> Broadcast by Central Node. HMI uses this to display system health (OK or Error).</p>

  <hr />
  <p><strong>Note:</strong> All messages conform to the class-wide protocol, occupying bytes 4–61 of the total packet. Sender/receiver IDs and framing bytes are handled externally.</p>

</body>
</html>
