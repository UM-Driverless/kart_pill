# Kart Medulla - ESP32 WROOM 32 + AS5600 Angle Sensor

GPIO 25 is PWM and GPIO 26 is DIR.

## Hardware Used

* **Microcontroller:** ESP32 WROOM 32
* **Sensor:** AS5600 Magnetic Angle Sensor

## Wiring Connections

### ESP32 WROOM 32 to AS5600

| AS5600 Pin | ESP32 Pin |
| ---------- | --------- |
| SCL        | GPIO 22   |
| SDA        | GPIO 21   |
| VCC        | 3.3V      |
| GND        | GND       |

### ESP32 WROOM 32 to Motor Driver

| Motor Driver Pin | ESP32 Pin |
| ---------------- | --------- |
| PWM              | GPIO 25   |
| DIR              | GPIO 26   |
| VCC              | 5V        |
| GND              | GND       |

### ESP32 WROOM 32 to Orin (UART)

| Orin Pin | ESP32 Pin |
| -------- | --------- |
| TX       | GPIO 18   |
| RX       | GPIO 19   |
| GND      | GND       |

### Other Pins

*   **LED:** GPIO 2 (Onboard LED)

## Software Setup

### PlatformIO Configuration (`platformio.ini`)

```ini
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
monitor_speed = 115200
```

## Flashing and Running

To flash and run the code from the root directory of this repository, use the `-d` flag to specify the project directory:

### Upload and run serial monitor

```bash
platformio run --target upload && platformio device monitor
```

Serial output will be available at **115200 baud** via the USB port.

## Monitoring Serial Output

### Using Screen (Recommended)

First, find your device:
```bash
ls -l /dev/serial/by-id/
```

Then connect using screen:
```bash
screen /dev/serial/by-id/usb-Silicon_Labs_CP2102* 115200
```

Or if you know the device name:
```bash
screen /dev/ttyUSB0 115200
```

To exit screen: `Ctrl+A`, then `K`, then `Y`

### Alternative Method

```bash
stty -F /dev/ttyUSB0 115200 raw -echo && timeout 10 cat /dev/ttyUSB0
```

## Example Serial Monitor Output

```
Freq: 1000.0 Hz  Target: 0.2319 rad  Actual: -3.1416 rad  PID: -1.0000  DIR: 0  PWM: 255.00
Freq: 1000.0 Hz  Target: 0.1726 rad  Actual: -3.1416 rad  PID: -1.0000  DIR: 0  PWM: 255.00
Freq: 1000.0 Hz  Target: 0.1058 rad  Actual: 2.7167 rad  PID: -1.0000  DIR: 0  PWM: 255.00
...
```

### Output Parameters Explained:
- **Freq**: Loop execution frequency in Hz (typically ~1000 Hz)
- **Target**: Desired steering angle in radians (sine wave: ±0.35 rad / ±20°)
- **Actual**: Current steering angle from AS5600 sensor in radians
- **PID**: PID controller output (-1.0 to +1.0)
- **DIR**: Motor direction (0 or 1)
- **PWM**: Motor PWM value (0-255)

The system uses a sine wave target with 20° amplitude and 3-second period to test the PID steering control.

Ensure your wiring is correct and your ESP32's USB port is recognized.

## Chip Information

* **ESP32 WROOM 32**

  * Dual-core Tensilica LX6
  * 4MB Flash, 520KB RAM
  * Operating Voltage: 3.3V
  * Built-in Wi-Fi & Bluetooth

* **AS5600**

  * 12-bit Magnetic Angle Sensor
  * I²C Interface (address: `0x36`)

This guide provides all necessary information to set up and verify the functionality of the AS5600 angle sensor with an ESP32 WROOM 32.

# Reference systems
- Body reference system: X is forward, Y is left, Z is up
- Kart steering angle definition: 0 rad is straight ahead, positive values turn left, negative values turn right. This means a positive rotation angle creates a rotation vector in +Z
- The AS5600 angle sensor outputs a positive angle when the magnet rotates with a vector pointing to the direction of the pcb with no chip.
- 

