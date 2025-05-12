**System:** ESP32 + A4988 + 42BYGHM809 Stepper Motor  

---

## Overview

This test demonstrates basic control of a bipolar stepper motor (NEMA 17 - 42BYGHM809) using an A4988 stepper motor driver module interfaced with an ESP32 development board. The objective was to validate wiring, logic signal handling, and motor rotation (including directional control) under full-step resolution.

---

## System Architecture

### Hardware Components

- **Microcontroller:** ESP32 Dev Module  
- **Driver:** A4988 Stepper Motor Driver  
- **Stepper Motor:** 42BYGHM809 (4-wire bipolar)  
- **Breadboard & Jumper Wires**  
- **External Power Supply:** 12V DC (VMOT to GND)  

---

## Pinout Connections

| A4988 Pin | Connection                       |
|-----------|----------------------------------|
| DIR       | ESP32 GPIO27                    |
| STEP      | ESP32 GPIO26                    |
| RESET     | Connected to SLEEP (tied HIGH) |
| SLEEP     | Connected to RESET             |
| MS1       | GND (Full-step mode)           |
| MS2       | GND                             |
| MS3       | GND                             |
| ENABLE    | GND (Driver always enabled)     |
| 1A, 1B, 2A, 2B | Connected to stepper motor coils |
| VMOT      | 12V Power Supply (+)            |
| GND (Motor Power) | Power Supply GND        |
| VDD       | 3.3V from ESP32 (logic level)   |
| GND (Logic) | Common ground with ESP32      |

> **Note:** Motor wires were identified by continuity and paired for proper coil alignment:  
> - Coil A: Red & Blue  
> - Coil B: Green & Black  

---

## Software Logic

ESP32 firmware toggles the STEP pin every 5 ms to step the motor, while the DIR pin changes every 5 seconds to reverse direction.

```cpp
#define STEP_PIN 26
#define DIR_PIN 27

void setup() {
  pinMode(STEP_PIN, OUTPUT);
  pinMode(DIR_PIN, OUTPUT);
}

void loop() {
  digitalWrite(DIR_PIN, HIGH);
  for (int i = 0; i < 1000; i++) {
    digitalWrite(STEP_PIN, HIGH);
    delay(5);
    digitalWrite(STEP_PIN, LOW);
    delay(5);
  }

  digitalWrite(DIR_PIN, LOW);
  for (int i = 0; i < 1000; i++) {
    digitalWrite(STEP_PIN, HIGH);
    delay(5);
    digitalWrite(STEP_PIN, LOW);
    delay(5);
  }
}
```

---

## Test Results

- **Motor responded to both STEP and DIR signals.**  
- **Full-step movement confirmed with correct MS1–MS3 logic levels.**  
- **Motor rotated smoothly in both directions.**  
- **12V external power supply was sufficient to maintain torque and speed.**

---

## Notes & Observations

- MS1, MS2, MS3 were grounded to select **full-step mode** for simplicity.
- RESET and SLEEP pins were tied together and pulled HIGH to enable operation.
- Logic signals were 3.3V, consistent with ESP32 output levels.
- Proper heat dissipation was considered for A4988 during prolonged operation.

---

## Future Improvements

- Add microstepping resolution tests (1/2, 1/4, 1/8, 1/16)
- Implement acceleration profiles for smoother motion.
- Integrate with encoder for closed-loop validation.
- Add UI interface or Bluetooth control for real-time stepper manipulation.

---

## Photos


![Stepper Motor Test Setup](https://github.com/jhherre3/ESP32-DevKitC/blob/main/Projects/StepperMotorControl/stepper.jpg)
 

