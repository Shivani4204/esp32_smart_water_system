## 📸 Hardware Prototype (Real Built System)
<p align="center">
  <img src="docs/hardware/whole%20project.jpeg" width="32%" alt="Complete System Setup" />
  <img src="docs/hardware/L298N%20.jpeg" width="32%" alt="L298N Motor Driver Circuit" /> 
  <img src="docs/hardware/ultrasonic%20sensor.jpeg" width="32%" alt="HC-SR04 Ultrasonic Sensor" />
</p>
<p align="center">
  <b>Left:</b> Full system assembly (ESP32 + pump + sensors) | <b>Middle:</b> L298N motor driver handling 12V pump control | <b>Right:</b> HC-SR04 water level sensing module
</p>

<i><b>Note:</b> This is a solo-built hardware project (no team) — from circuit design to firmware deployment.</i>

---
ESP32 Smart Water Management System

An intelligent water management system built with ESP32 microcontroller that monitors water levels, flow rates, pressure, and power consumption while automatically controlling a water pump with safety features.

📋 Table of Contents

Features
Hardware Components
System Architecture
Pin Configuration
Installation
Usage
Safety Features
Contributing
License

✨ Features

Automatic Water Level Control: Maintains optimal water levels using ultrasonic sensing
Real-time Flow Monitoring: Tracks water flow rate and total volume dispensed
Pressure Monitoring: Continuous air pressure monitoring for system diagnostics
Power Consumption Tracking: Current sensor integration for energy efficiency analysis
Vibration Detection: Safety shutdown on abnormal vibrations
Dual Control Modes:

Automatic mode with intelligent decision-making
Manual mode for direct pump control


Serial Command Interface: Easy interaction via terminal
Safety Interlocks: Multiple fail-safe mechanisms to prevent damage

🔧 Hardware Components
ComponentModel/TypeQuantityPurposeMicrocontrollerESP32 DevKit1Main controllerMotor DriverL298N1Pump controlUltrasonic SensorHC-SR041Water level measurementWater Flow SensorYF-S2011Flow rate monitoringPressure SensorBMP180/MPX57001Air pressure measurementCurrent SensorACS712-5A1Power consumption monitoringVibration SensorSW-4201Abnormal vibration detectionDC Motor/Pump12V1Water pumpingPower Supply12V 2A Adapter1System power
🏗️ System Architecture
┌─────────────┐
│   ESP32     │
│  (Master)   │
└──────┬──────┘
       │
       ├──────► HC-SR04 (Ultrasonic) ──► Water Level
       ├──────► YF-S201 (Flow Sensor) ──► Flow Rate
       ├──────► Pressure Sensor ──────► Air Pressure
       ├──────► ACS712 (Current) ─────► Power Usage
       ├──────► SW-420 (Vibration) ───► Safety Monitor
       └──────► L298N ─────────────────► 12V Pump
📌 Pin Configuration
Digital Pins
cppTRIG_PIN (GPIO 5)         - Ultrasonic trigger
ECHO_PIN (GPIO 18)        - Ultrasonic echo
PUMP_IN1 (GPIO 26)        - Motor driver input 1
PUMP_IN2 (GPIO 27)        - Motor driver input 2
PUMP_ENA (GPIO 25)        - Motor PWM speed control
FLOW_SENSOR (GPIO 19)     - Flow sensor pulse input
VIBRATION (GPIO 21)       - Vibration sensor digital
Analog Pins
cppPRESSURE_SENSOR (GPIO 34) - Pressure analog input
CURRENT_SENSOR (GPIO 35)  - Current analog input
🚀 Installation
Prerequisites

Arduino IDE (1.8.x or later) or PlatformIO
ESP32 board support installed
USB to Serial driver for ESP32

Setup Steps

Clone the repository

bash   git clone https://github.com/yourusername/esp32-water-management.git
   cd esp32-water-management

Install ESP32 Board Support

Open Arduino IDE
Go to File → Preferences
Add to Additional Board Manager URLs:



     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json

Go to Tools → Board → Board Manager
Search "ESP32" and install


Connect Hardware

Follow the pin configuration diagram
Ensure proper power supply connections
Double-check L298N wiring for motor


Upload Code

Open water_management_system.ino
Select board: ESP32 Dev Module
Select correct COM port
Click Upload


Open Serial Monitor

Set baud rate to 115200
Start interacting with the system



💻 Usage
Serial Commands
CommandFunctionyStart pump (manual mode)nStop pump (manual mode)aEnable automatic modemEnable manual modesShow detailed sensor status
Example Session
========== SYSTEM STATUS ==========
Mode: AUTOMATIC
Water Level: 35.2 cm
Flow Rate: 2.3 L/min
Total Volume: 12.5 L
Pressure: 14.7 PSI
Current: 1.2 A
Vibration: Normal
Pump Status: RUNNING
===================================
Automatic Mode Logic

Pump starts when water level < 10 cm
Pump stops when water level > 50 cm
Emergency stop on vibration detection or overcurrent

## 🔧 Challenges & Debugging
**Problem:** Pump not triggering despite correct code  
**Root Cause:** L298N logic ground not common with ESP32 ground  
**Fix:** Added common ground wire between boards  
**Lesson:** Always verify reference voltage levels in motor driver circuits

**Problem:** Ultrasonic sensor giving erratic readings (-1 values)  
**Root Cause:** Power supply ripple affecting sensor  
**Fix:** Added 100µF capacitor across VCC-GND of HC-SR04  
**Lesson:** Decoupling capacitors critical for sensor stability

🛡️ Safety Features

Overcurrent Protection: Automatically stops pump if current exceeds 5A
Vibration Detection: Shuts down on abnormal mechanical vibrations
Sensor Validation: Filters invalid ultrasonic readings
Timeout Protection: Prevents sensor lockups with timeout mechanisms
Manual Override: Emergency manual control available anytime

📊 Customization
Adjust these constants in the code for your specific setup:
cppconst int MIN_WATER_LEVEL = 10;   // Start pump threshold (cm)
const int MAX_WATER_LEVEL = 50;   // Stop pump threshold (cm)
const int PUMP_SPEED = 255;       // PWM speed (0-255)
🔍 Troubleshooting
Pump won't start

Check L298N connections and power supply
Verify motor wires are properly connected
Ensure IN1/IN2 signals are reaching driver

Ultrasonic sensor shows -1

Check TRIG and ECHO pin connections
Ensure sensor has clear line of sight
Verify 5V power to sensor

Flow sensor not registering

Confirm water is actually flowing
Check interrupt pin connection
Verify sensor orientation (arrow shows flow direction)


🤝 Contributing
Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

👨‍💻 Author
Shivani odedra

GitHub: @Shivani4024
Email: shivaniodedra60@gmail.com

🙏 Acknowledgments

ESP32 Community - For extensive documentation
Arduino Community - For sensor libraries

📈 Future Enhancements

 WiFi connectivity for remote monitoring
 Mobile app integration
 Data logging to SD card or cloud
 Multiple pump support
 pH and temperature monitoring
 Machine learning for predictive maintenance


⭐ If you find this project useful, please consider giving it a star!
