# ESP32-Based Low-Power Sensor System with Deep Sleep Functionality

A battery-powered environmental monitoring system built on the ESP32-WROOM-DA. This project demonstrates real multi-day battery operation while collecting temperature, humidity, battery voltage, current, and power consumption data. The system utilizes deep sleep, a local Wi-Fi web dashboard, and an INA219 power monitor to analyze its own energy efficiency.

## 🌟 Key Features

*   **Ultra-Low Power Operation:** Utilizes 3-minute deep sleep intervals to minimize energy consumption.
*   **Accurate Battery Estimation:** Implements Li-ion open-circuit voltage (OCV) compensation and a dedicated discharge curve to provide a stable battery percentage, eliminating voltage-sag inaccuracies.
*   **Local Web Dashboard:** Hosts a self-contained Wi-Fi Access Point (Hotspot) that serves a responsive HTML/CSS/JS dashboard for live data and historical graphs.
*   **Data Logging:** Saves all readings (including the calculated battery percentage) to a CSV file on the ESP32's internal flash memory (LittleFS).
*   **Power Optimized Web Mode:** Drops CPU frequency to 80MHz, reduces Wi-Fi TX power, and implements a 90-second hard timeout to save battery.
*   **Proven Endurance:** Successfully operated for **3.13 days (75.1 hours)** on a single 3500mAh 18650 battery, retaining 31% capacity at the end of the test.

## 🛠️ Hardware Requirements

*   **Microcontroller:** ESP32-WROOM-DA (or standard ESP32 Dev Module)
*   **Sensor:** DFRobot Gravity Analog SHT30 Temperature & Humidity Sensor (DFR0588)
*   **Power Monitor:** INA219 Current and Voltage Sensor Module
*   **Power Supply:** 18650 Lithium-Ion Battery (e.g., Samsung INR18650-35E) with 1-Channel Battery Box
*   **Charging:** TP4056 Lithium-Ion Battery Charging Module
*   **Power Regulation:** Step-Up Boost Power Supply (3.7V to 5V)
*   **Input/Output:** Tactile Push Button Switch, 5mm LED Indicator, Jumper Wires

## 📁 Repository Structure

*   **`/Firmware`**: Contains the main Arduino sketch (`Low_Power_Sensor_System.ino`), the raw test data (`Low Power.csv`), and firmware-specific instructions.
*   **`/Documentation`**: Contains the comprehensive project documentation website/HTML file detailing the hardware setup, software architecture, and testing results.

## 🚀 Getting Started

To replicate this project or upload the firmware:

1.  **Hardware Assembly:** Wire the components according to the schematic in the Documentation folder. Ensure the INA219 is placed between the TP4056 and the Boost Converter.
2.  **Software Setup:** Open the `/Firmware/Low_Power_Sensor_System.ino` file in the Arduino IDE.
3.  **Libraries:** Install the `Adafruit INA219` and `ArduinoJson` libraries via the Library Manager.
4.  **Upload:** Select the "ESP32-WROOM-DA Module" board and upload the code.
5.  **Operation:** 
    *   By default, the system will wake up every 3 minutes, take a reading, log it to the CSV, and go back to sleep.
    *   **Press the push button** to wake the ESP32 and activate the web dashboard. Connect to the Wi-Fi network `LowPowerSensor` (Password: `12345678`) and navigate to `192.168.4.1` in your browser.

## 📊 Testing Results

During the final continuous test, the device successfully operated across multiple logging sessions for a combined total of **270,344 seconds (75.1 hours)**. Over 1,418 total readings were collected. The battery percentage smoothly decreased from 100% down to 31% at the end of the test, proving the stability of the battery algorithm.

## 👥 Authors

*   **PJ Lyle M. Berongoy**
*   **Kathleen I. Montargo**

## 📄 License / Attribution

This project was developed for educational purposes and uses components provided by [CreateLabz Store](https://createlabz.store/). 
