# 📡 ESP-NOW Hello World — Two ESP32 Demo


![ESP-NOW Demo](https://github.com/jhherre3/ESP32-DevKitC/blob/main/Files/espnowtest1.png)

> Image showing two ESP32 boards connected via USB and communicating via ESP-NOW.


This project demonstrates a simple "Hello World" communication between two ESP32 boards using [ESP-NOW](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/network/esp_now.html), a fast and connectionless peer-to-peer wireless protocol developed by Espressif.

---

## 🧠 What is ESP-NOW?

ESP-NOW allows multiple ESP32 devices to communicate directly with each other without needing Wi-Fi or a router. It is **low-latency**, **low-power**, and ideal for sensor networks, remote control, and fast data exchange.

---

## 🧰 What You Need

- ✅ Two ESP32 boards (e.g. DevKit v1)
- ✅ Two USB cables
- ✅ Arduino IDE with **ESP32 Board Package** installed
- ✅ Serial Monitor access on both computers

---

## 🧪 Step 1: Get Each ESP32's MAC Address

Upload this sketch to each ESP32 **separately** to find their unique MAC addresses:

```cpp
#include <WiFi.h>

void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  delay(100);
  Serial.println("ESP32 MAC Address:");
  Serial.println(WiFi.macAddress());
}

void loop() {}
```

Example output:
```
ESP32 MAC Address:
FC:B4:67:F5:4C:A0
```

Use the MAC of the **receiver board** in the sender sketch (see below).

---

## 📤 Sender Code (TX ESP32)

This ESP32 sends a `"Hello World"` message every 2 seconds via ESP-NOW.

**Replace the MAC below** with your actual **receiver's MAC address**.

```cpp
#include <esp_now.h>
#include <WiFi.h>

// ✅ Receiver MAC address
uint8_t receiverMacAddress[] = { 0xFC, 0xB4, 0x67, 0xF5, 0x51, 0xFC };

void OnDataSent(const uint8_t *mac_addr, esp_now_send_status_t status) {
  Serial.print("Send Status: ");
  Serial.println(status == ESP_NOW_SEND_SUCCESS ? "Success" : "Fail");
}

void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  WiFi.disconnect();

  if (esp_now_init() != ESP_OK) {
    Serial.println("❌ ESP-NOW Init Failed");
    return;
  }

  esp_now_register_send_cb(OnDataSent);

  esp_now_peer_info_t peerInfo = {};
  memcpy(peerInfo.peer_addr, receiverMacAddress, 6);
  peerInfo.channel = 0;
  peerInfo.encrypt = false;

  if (esp_now_add_peer(&peerInfo) != ESP_OK) {
    Serial.println("❌ Failed to add peer");
    return;
  }

  Serial.println("📤 Sending messages to RX...");
}

void loop() {
  const char *message = "Hello World from TX!";
  esp_err_t result = esp_now_send(receiverMacAddress, (uint8_t *)message, strlen(message));

  if (result == ESP_OK) {
    Serial.println("✅ Message Sent");
  } else {
    Serial.println("❌ Send Error");
  }

  delay(2000);
}
```

---

## 📥 Receiver Code (RX ESP32)

This ESP32 listens for messages and prints them along with the sender's MAC address.

```cpp
#include <esp_now.h>
#include <WiFi.h>

void OnDataRecv(const uint8_t *mac, const uint8_t *incomingData, int len) {
  char macStr[18];
  snprintf(macStr, sizeof(macStr), "%02X:%02X:%02X:%02X:%02X:%02X",
           mac[0], mac[1], mac[2], mac[3], mac[4], mac[5]);

  Serial.print("📥 Received from ");
  Serial.print(macStr);
  Serial.print(": ");
  Serial.write(incomingData, len);
  Serial.println();
}

void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  WiFi.disconnect();

  if (esp_now_init() != ESP_OK) {
    Serial.println("❌ ESP-NOW Init Failed");
    return;
  }

  esp_now_register_recv_cb(OnDataRecv);
  Serial.println("✅ Ready to receive ESP-NOW messages...");
}

void loop() {
  // Nothing here, receiving is interrupt-driven
}
```

---

## 🚦 How to Test

1. Upload the **MAC address script** to both ESP32s and record their MACs.
2. Update the sender code with the **receiver's MAC address**.
3. Upload the **receiver code** to one board.
4. Upload the **sender code** to the other board.
5. Open **both Serial Monitors** at 115200 baud.
6. Observe the `"Hello World"` messages appearing on the receiver's monitor every 2 seconds.

---

## 📌 Notes

- Make sure both ESP32s are using the **same Wi-Fi channel** (default `0` works).
- ESP-NOW does **not require Wi-Fi credentials**.
- Communication range is typically **up to 200+ meters** line-of-sight.

---

## ✅ Next Steps

- 🔄 Add **bidirectional communication**
- 🔐 Add **encryption and pairing**
- 📦 Send **custom structs/data packets**
- 🌐 Combine with Wi-Fi or web interface



