# ESP32-WROOM-DA-Based-Low-Power-Sensor-System  Firmware

This folder contains the main Arduino firmware for the **ESP32 Low Power Sensor System** project.

The firmware controls the ESP32-WROOM-DA module and manages environmental sensing, battery monitoring, low-power operation, data storage, and the local web dashboard.

---

## Main Firmware File

### `Low_Power_Sensor_System.ino`

This is the main Arduino sketch responsible for operating the complete system.

The firmware performs:

- Temperature and humidity measurement using the **DFRobot Gravity: Analog SHT30 Temperature & Humidity Sensor (DFR0588)**
- Battery voltage, current, and power monitoring using the **INA219 Current and Voltage Sensor Module**
- Accurate battery percentage estimation using an OCV (Open Circuit Voltage) compensation algorithm and Li-ion discharge curve
- Accurate uptime tracking across deep sleep cycles using RTC memory
- Deep sleep power management for extended battery life
- Wi-Fi Access Point creation with optimized CPU and TX power for reduced battery drain
- Web server operation with a 90-second automatic timeout
- Battery history logging using LittleFS
- CSV data generation for battery performance analysis

---

# Hardware Used

The firmware is designed for:

- ESP32-WROOM-DA Module
- INA219 Current and Voltage Sensor Module
- DFRobot Gravity: Analog SHT30 Temperature & Humidity Sensor (DFR0588)
- Samsung INR18650-35E 3500mAh Rechargeable Lithium-Ion Battery
- Tactile Push Button Switch
- 5mm LED Indicator

---

## Pin Configuration

| Component | ESP32-WROOM-DA Pin |
|---|---|
| INA219 SDA | GPIO21 |
| INA219 SCL | GPIO22 |
| SHT30 Temperature Output | GPIO35 |
| SHT30 Humidity Output | GPIO34 |
| LED Indicator | GPIO25 |
| Push Button | GPIO27 |

---

## Low Power Operation

The ESP32 periodically wakes from deep sleep (every 3 minutes), collects sensor measurements, saves the data, and returns to sleep mode to reduce battery consumption.

To maximize battery life, the firmware implements several power-saving features:
- **CPU Downclocking:** Drops the CPU speed to 80MHz during Web Mode.
- **Wi-Fi Power Reduction:** Reduces Wi-Fi transmit power to 11dBm during Web Mode.
- **Low-Battery Guard:** Skips Wi-Fi activation if the battery drops below 3.45V, preventing brownouts.
- **Accurate Runtime:** Accumulates and saves uptime in RTC memory before every sleep cycle.

This allows the system to demonstrate real multi-day operation. In the final endurance test, the system successfully operated for **3.13 days (75.1 hours)** and still retained 31% of its battery capacity.

---

## Battery Monitoring

The INA219 sensor measures:
- Battery voltage
- Current consumption
- Power usage

Because the system load causes a voltage drop (sag), the firmware applies a load-compensation calculation using `BATTERY_INTERNAL_R` to estimate the true resting (open-circuit) voltage. This estimated voltage is mapped against a piecewise-linear Li-ion discharge curve to provide a stable and highly accurate battery percentage.

---

## Temperature and Humidity Measurement

The project uses the **DFRobot Gravity: Analog SHT30 Temperature & Humidity Sensor (DFR0588)**.

The DFR0588 is an analog-output version of the SHT30 sensor. Unlike a standard digital SHT30 module, this sensor provides calibrated analog voltage outputs that represent temperature and humidity measurements.

The ESP32-WROOM-DA reads these analog voltage signals using its ADC pins and converts them into temperature and humidity values using the sensor transfer equations.

The measurement process is:

```text
DFR0588 Sensor
      ↓
Analog Voltage Output
      ↓
ESP32 ADC Reading
      ↓
Voltage Conversion Formula
      ↓
Temperature and Humidity Values
