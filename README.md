# 🔌 ESP32-DevKitC – MCU & Pin Reference

This repository is a personal reference and hardware overview for working with the **ESP32-DevKitC**, a compact and powerful development board based on the **ESP32-WROOM-32** SoC.

---

## 🧠 MCU Overview – ESP32-WROOM-32

- Dual-core 32-bit Xtensa LX6 @ 240 MHz
- 520 KB SRAM, 4 MB Flash (external)
- Integrated Wi-Fi + Bluetooth 4.2 (Classic + BLE)
- 12-bit ADC (18 channels), 2x DAC
- 10 capacitive touch inputs, SPI, I2C, UART, PWM

📘 Official docs: [ESP32 DevKitC Documentation](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32/esp32-devkitc/index.html)

---

## 🧭 Pin Layout

![ESP32 DevKitC Pinout](Files/esp32_devkitC_v4_pinlayout.png)

Use this image to identify GPIO capabilities and reference them while developing with sensors, actuators, and displays.

---

## 📌 GPIO Usage Guide

### ✅ Safe GPIOs for General Use
- `GPIO2`, `GPIO4–5`, `GPIO12–19`, `GPIO21–23`, `GPIO25–27`, `GPIO32–33`

### ⚠️ Use with Caution
- `GPIO0`, `GPIO2`, `GPIO15` – Affects boot mode
- `GPIO6–11` – Reserved for flash memory (do not use)
- `GPIO1`, `GPIO3` – USB serial RX/TX

### 🔄 Special Function Pins
| Feature   | GPIO Pins                   |
|-----------|-----------------------------|
| **ADC1**  | GPIO32–39 (safe with Wi-Fi) |
| **ADC2**  | GPIO0, 2, 4, 12–15, 25–27    |
| **DAC**   | GPIO25 (DAC1), GPIO26 (DAC2)|
| **Touch** | GPIO0, 2, 4, 12–15, 27, 32, 33|

---

## 🗂️ Files Directory

The `Files/` folder includes:

- ✅ **Official schematic** for ESP32-DevKitC
- ✅ **PCB layout and design files** from Espressif
- 📸 `esp32_devkitC_v4_pinlayout.png` – high-resolution pinout diagram

These assets are useful for hardware development, troubleshooting, and integration into custom PCB designs.

---

## 🧪 Coming Soon in This Repo

- Simple peripheral demos (LEDs, buttons, ADC, PWM)
- PS5 controller interface over Bluetooth
- GPIO test code and logic analyzer captures

---

## 🔗 Useful Resources

- [ESP32 DevKitC Overview](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32/esp32-devkitc/)
- [ESP32 Datasheet (PDF)](https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf)

---

## ✍️ Maintained by Jhherre3
