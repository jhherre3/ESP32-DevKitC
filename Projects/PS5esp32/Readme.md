# 🎮 PS5 Controller to ESP32 (Bluetooth Demo)

This Arduino sketch connects a **PS5 DualSense controller** to an **ESP32 DevKitC** via Bluetooth. It reads button presses and joystick movements, then logs those inputs to the Serial Monitor.

---

## 📦 Features

- Detects button presses: Cross, Circle, Triangle, Square, L1, R1, L2, R2, and D-Pad directions
- Reads joystick input from both analog sticks
- Applies a dead zone to reduce noise from small unintentional movements
- Only logs input changes to avoid cluttering the Serial Monitor

---

## 🛠️ Built With

- **Arduino IDE**
- **ESP32 DevKitC**
- Library: [`ps5-esp32`](https://github.com/rodneybakiskan/ps5-esp32)

Install the library by downloading it from GitHub and placing it in your `Arduino/libraries` directory.

---

## 📲 Setup

1. Clone this project or copy the code to a new Arduino sketch.
2. Replace the Bluetooth MAC address in the line below with your controller's address:
   ```cpp
   ps5.begin("7C:66:EF:30:4A:4F"); // Your controller's MAC address
   ```
3. Connect your ESP32 board via USB.
4. In the Arduino IDE:
   - Select your board: `Tools > Board > ESP32 Dev Module`
   - Select the correct COM port
5. Upload the code and open the Serial Monitor (115200 baud).

---

## 📷 Use Cases

This project is ideal for:

- Controlling servos or motors using joystick input
- Building a remote-controlled robot with a gamepad
- Learning how Bluetooth Classic HID devices interface with microcontrollers

---

## 🧠 Notes

- `L2Value()` and `R2Value()` can be added to capture analog trigger pressure (0–255).
- This uses **Bluetooth Classic**, not BLE.
- Some USB pairing might be needed before wireless pairing works. (connect to PC read Bluetooth Address)
- Requires ESP32 with Bluetooth support (e.g., DevKitC, WROOM modules).

---

## 📄 License

This project is open source and available under the MIT License.

---

## ✍️ Author

Jeremiah Herrera – [@jhherre3](https://github.com/jhherre3)
