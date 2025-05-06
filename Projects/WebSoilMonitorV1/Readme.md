# 🌱 Soil Moisture Monitor with ESP32

This project is a **Wi-Fi-enabled soil moisture logger** built with an ESP32, capable of logging soil readings every 5 minutes and hosting a simple live webpage to display current and historical moisture data.

---

## ✅ Version 1 – Working Prototype

> Below is an image of the working **SoilMoistureMonitor V1** prototype using an ESP32 and analog soil probe.

![SoilMoistureMonitor V1](/Files/SoilMoistureProbeV1.png)

---

## 🔧 Features

- 📡 **Wi-Fi Web Server:** Displays real-time moisture readings.
- 📈 **Logging System:** Stores 12 readings (1 hour worth) at 5-minute intervals.
- 🌐 **Web Interface:** Auto-refreshing webpage with live and historical data.
- 🔌 **Calibrated Sensor Input:** Maps analog values from dry to wet soil into a readable percentage (0–100%).

## 🧠 How It Works

1. **Sensor Input:**  
   The soil sensor is read through an analog pin (`GPIO 34`). Its raw reading is mapped between two calibrated values:  
   - `2695` = dry  
   - `1325` = wet  

2. **Wi-Fi Setup:**  
   The ESP32 connects to the local Wi-Fi network using hardcoded credentials. Once connected, it starts an HTTP server on port `80`.

3. **Data Logging:**  
   Every 5 minutes, the latest soil moisture value is stored in a circular buffer (`logBuffer[]`) that holds the last 12 readings (~1 hour of data).

4. **Web Display:**  
   A connected client can view the real-time moisture value and the past hour's log via a dynamically generated HTML page.  
   - The page auto-refreshes every 5 seconds.
   - The log shows each entry with its index and moisture percentage.

## 📦 Hardware Required

- ESP32 Development Board  
- Soil Moisture Sensor (Analog Output)  
- (Optional) 3.3V power regulator if using battery supply  
- Breadboard & jumper wires

## 🔋 Calibration Note

The moisture values were mapped using:
```cpp
constrain(map(raw, dryBaseline, wetValue, 0, 100), 0, 100);
```
You may need to recalibrate `dryBaseline` and `wetValue` based on your specific sensor or soil conditions.

## 🌐 Sample Webpage Output

```html
<h1>🌱 Soil Moisture Monitor</h1>
<h2>Current Moisture: 63%</h2>
<h3>📊 Moisture Log (Last Hour, 5-min intervals)</h3>
<ul>
  <li>Log 141: 62%</li>
  <li>Log 142: 63%</li>
  ...
</ul>
```

---

### 🔮 Planned Features for V2

![SoilMoistureMonitor V1](/Files/SoilMoistureProbeV1.png)

- 📱 **Mobile-Friendly Interface:** Add touch-friendly graphs and UI elements for phones and tablets.
- 📊 **Chart View:** Real-time and historical data plotted using embedded JavaScript (e.g., Chart.js).
- 🔔 **Threshold Alerts:** Send Wi-Fi push alerts or email when moisture falls below a set threshold.
- 🔋 **Battery Status Bar:** Show battery % and charging state visually on the webpage.
- 🌡 **Multiple Sensor Support:** Interface with more than one soil probe (multi-zone monitoring).
- 🧠 **Smart Watering (Optional):** Activate a relay for a water pump if moisture drops below target.
- 🌐 **Offline Mode:** Store logs locally and upload when Wi-Fi is available.
- 🔧 **Wi-Fi Config Page:** Access a web-based setup portal to update Wi-Fi credentials without code flashing.
