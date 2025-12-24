# Paralysed Patient Communication & Fall Detection Glove

**Assistive wearable glove system** designed to help paralyzed or severely mobility-impaired patients communicate basic needs and alert caregivers during falls.

This project uses an Arduino, flex sensors, MPU6050 accelerometer, I2C LCD, buzzer, and optional GSM module for SMS alerts.

## Features

- **Gesture-based communication** via 4 flex sensors (bend fingers to select messages)
- **Fall detection** using MPU6050 accelerometer
- **Real-time feedback** on 16×2 LCD
- **Audible alerts** via buzzer with different tones for each message
- **SMS notification** to caregiver (via GSM module – e.g., SIM800L/SIM900)
- Displays current temperature (from MPU6050) for monitoring

## Hardware Requirements

| Component              | Quantity | Notes / Suggested Model         |
|------------------------|----------|---------------------------------|
| Arduino Uno / Nano     | 1        | Uno recommended for prototyping |
| MPU6050 (6-axis IMU)   | 1        | GY-521 module                   |
| Flex Sensors           | 4        | 2.2" or 4.5" bend sensors       |
| 16×2 LCD with I2C      | 1        | PCF8574 backpack (address 0x27) |
| Active/Passive Buzzer  | 1        | Passive for better tones        |
| GSM Module             | 1        | SIM800L / SIM900 (with SIM card)|
| Voltage Dividers       | 4× 10kΩ resistors | For flex sensors               |
| Pull-up resistors      | 2× 4.7kΩ | For I2C bus (if not built-in)   |
| Power Supply           | 1        | 5V 2A+ for GSM; separate LiPo recommended |

## Pin Connections

| Component              | Arduino Pin | Description                          |
|------------------------|-------------|--------------------------------------|
| Flex Sensor 1          | A0          | Analog input                         |
| Flex Sensor 2          | A1          | Analog input                         |
| Flex Sensor 3          | A2          | Analog input                         |
| Flex Sensor 4          | A3          | Analog input                         |
| MPU6050 SDA            | A4          | I2C Data                             |
| MPU6050 SCL            | A5          | I2C Clock                            |
| LCD I2C SDA            | A4          | Shared I2C bus                       |
| LCD I2C SCL            | A5          | Shared I2C bus                       |
| Buzzer                 | D9          | PWM-capable for tone()               |
| GSM RX                 | D10         | SoftwareSerial RX                    |
| GSM TX                 | D11         | SoftwareSerial TX                    |
| 5V                     | 5V          | VCC for all modules (except GSM)     |
| GND                    | GND         | Common ground                        |

**Important**:  
- Use **4.7kΩ pull-up resistors** on SDA and SCL to 5V if your modules don’t have them.  
- **Flex sensors** require a voltage divider (10kΩ resistor from 5V to sensor, sensor to analog pin, other end to GND).  
- **GSM module** needs a **separate 3.7–4.2V power source** (LiPo or 5V 2A adapter) — do **not** power from Arduino 5V pin.

## Supported Messages (Flex Sensor Gestures)

| Finger (Sensor) | Message Displayed         | Buzzer Tone | SMS (if GSM connected) |
|-----------------|----------------------------|-------------|------------------------|
| 1 (A0)          | "Food or Water"            | 500 Hz      | Yes                    |
| 2 (A1)          | "Emergency"                | 1000 Hz     | Yes                    |
| 3 (A2)          | "Need Fresh Air"           | 1500 Hz     | Yes                    |
| 4 (A3)          | "Washroom"                 | 2000 Hz     | Yes                    |

## Fall Detection

- Triggers when acceleration exceeds configurable thresholds on any axis
- Displays "Fall Detected!" + axis information
- Sends SMS alert (if GSM is connected)
- High-pitched buzzer tone

## Installation

1. Install required Arduino libraries:
   - **Adafruit MPU6050** (or MPU6050 by Electronic Cats)
   - **LiquidCrystal_I2C** (by Frank de Brabander)
   - **SoftwareSerial** (built-in)

2. Wire the components according to the pin table above.

3. Upload the provided code (ensure correct I2C address for your LCD – default 0x27).

4. Calibrate flex sensor thresholds in the code (array `thresholds[]`).

5. Insert a working SIM card into the GSM module and update the phone number in `triggerFallAlarm()`.

## Code

Full source code is available in this repository:  
[See `pglove.ino`](pglove.ino)

## Future Improvements

- Add calibration routine for MPU6050
- Bluetooth/WiFi (ESP32) for smartphone alerts
- Voice output module (DFPlayer Mini)
- Battery-powered version with low-power mode
- Machine-learning-based gesture/fall detection
- 
## Prototype
![Project Prototype](https://raw.githubusercontent.com/phonot/paralysed_healthcare_glove/main/prototype.png)

Live display showing HR, RR, PPG waveform with detected peaks, and quality metrics.
## License

MIT License – feel free to use, modify, and share.

---
