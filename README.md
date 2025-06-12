# Advanced ESP32 Irrigation Control System - Step by Step

This project implements a step-by-step configuration approach for building an advanced irrigation control system using ESP32-S3 that integrates with Home Assistant.

## 🎯 Project Overview

This is a **test and learning project** that provides progressive configurations, from basic sensor reading to a complete automated irrigation system with advanced features like sprinkler controllers, pump management, and intelligent watering algorithms.

Each directory contains a complete configuration that builds upon the previous one, allowing you to learn and implement features incrementally.

## 📁 Project Structure

```
riego-esphome/
├── basic/                      # Step 1: Basic sensor configuration
│   ├── esp32-s3-config.yaml   # Basic water level and soil moisture sensors
│   └── connections-diagram.md  # Wiring diagram for basic setup
├── basic-sensors/              # Step 2: Enhanced sensor suite
│   ├── esp32-s3-config.yaml   # Added DHT22, power monitoring, display
│   └── connections-diagram.md  # Extended wiring diagram
├── water-pump-controller/      # Step 3: Advanced irrigation system
│   ├── esp32-s3-config.yaml   # Complete sprinkler controller setup
│   └── connections-diagram.md  # Full system wiring
├── secrets.yaml               # WiFi credentials and API keys
├── lista-componentes.md        # Complete component list (Spanish)
└── README.md                   # This file
```

## 🚀 Step-by-Step Configuration Guide

### Step 1: Basic Configuration (`basic/`)
**Goal**: Learn to read water level and basic soil moisture sensors

**Features**:
- 📊 Water tank level monitoring (JSN-SR04T ultrasonic sensor)
- 🌱 2x Soil moisture sensors via ADS1115 ADC
- 📱 WiFi connectivity and Home Assistant integration
- 📈 Basic sensor calibration and filtering

**Hardware Required**:
- ESP32-S3 DevKit
- JSN-SR04T ultrasonic sensor
- ADS1115 16-bit ADC
- 2x Capacitive soil moisture sensors
- Basic wiring components

### Step 2: Enhanced Sensors (`basic-sensors/`)
**Goal**: Add environmental monitoring and power management

**Additional Features**:
- 🌡️ Temperature and humidity monitoring (DHT22)
- ⚡ Power consumption monitoring (INA226)
- 🔋 Battery voltage monitoring
- 📊 Enhanced sensor suite with 4x soil moisture sensors

**Additional Hardware**:
- DHT22 temperature/humidity sensor
- INA226 power monitor
- Battery management components

### Step 3: Complete Irrigation System (`water-pump-controller/`)
**Goal**: Full-featured irrigation controller with ESPHome's sprinkler component

**Advanced Features**:
- 💧 ESPHome sprinkler controller integration
- 🚰 4-plant irrigation system
- 🔄 Automatic pump control with safety features
- ⏱️ Programmable watering schedules
- 🛡️ Advanced protection systems
- 🎛️ Manual control overrides
- 📊 Comprehensive monitoring and logging

**Complete Hardware List**:
- All previous components
- 4x Water pumps (3V DC submersible)
- 4-channel relay module
- Water distribution system (tubing, connectors)
- Complete enclosure and mounting hardware

## 🛠️ Hardware Overview

### Core Components
| Component | Purpose | Quantity |
|-----------|---------|----------|
| **ESP32-S3 DevKit** | Main controller | 1 |
| **JSN-SR04T** | Water level sensor (IP67) | 1 |
| **ADS1115** | 16-bit ADC for soil sensors | 1 |
| **Capacitive Soil Sensors** | Moisture monitoring | 4 |
| **DHT22** | Temperature/humidity | 1 |
| **INA226** | Power monitoring | 1 |
| **4-Channel Relay Module** | Pump control | 1 |
| **3V DC Water Pumps** | Irrigation pumps | 4 |

### Power System
| Component | Purpose |
|-----------|---------|
| **2x 26650 Batteries** | Portable power supply |
| **LM2596 Buck Converter** | Voltage regulation |
| **Battery Holder PCB** | Battery management |

For a complete component list with specifications and sourcing information, see [`lista-componentes.md`](lista-componentes.md).

## 📋 Quick Start Guide

### 1. Prerequisites
```bash
# Install ESPHome
pip install esphome

# Clone this repository
git clone <repository-url>
cd riego-esphome
```

### 2. Configure Secrets
Edit `secrets.yaml` with your credentials:
```yaml
wifi_ssid: "Your_WiFi_SSID"
wifi_pswd: "Your_WiFi_Password"
ota_pswd: "Your_OTA_Password"
```

### 3. Choose Your Starting Point

#### Option A: Start with Basic (Recommended for beginners)
```bash
cd basic
esphome run esp32-s3-config.yaml
```

#### Option B: Jump to Enhanced Sensors
```bash
cd basic-sensors
esphome run esp32-s3-config.yaml
```

#### Option C: Deploy Complete System
```bash
cd water-pump-controller
esphome run esp32-s3-config.yaml
```

### 4. Home Assistant Integration
1. Navigate to **Settings** → **Integrations**
2. Search for "ESPHome"
3. Add the device using the ESP32's IP address
4. Use the API encryption key from the configuration

## 🔧 Configuration Features

### Progressive Learning Approach
Each configuration builds upon the previous one:

1. **Basic**: Learn sensor fundamentals and Home Assistant integration
2. **Enhanced**: Add environmental monitoring
3. **Complete**: Implement professional-grade irrigation control

### Key Technical Features

#### Advanced Sensor Management
- 📊 Multi-point median filtering for stable readings
- 🎯 Automatic calibration with linear interpolation
- 🔄 Adaptive update intervals based on system state
- 🛡️ Error handling and timeout protection

#### Professional Irrigation Control
- 💧 ESPHome's built-in sprinkler component
- 🚰 Plant-based watering with individual schedules
- 🔄 Pump coordination with multiple plants
- ⏱️ Precise timing control with overlap management
- 🛡️ Safety interlocks and emergency stops

#### Home Assistant Integration
- 📱 Complete entity exposure (sensors, switches, controls)
- 🎛️ Manual override capabilities
- 📊 Historical data logging
- 🔔 Alert and notification support
- 🤖 Advanced automation potential

## 📊 Monitoring Capabilities

### Environmental Data
- 🌡️ **Temperature**: Ambient air temperature
- 💧 **Humidity**: Relative humidity percentage
- 🌱 **Soil Moisture**: Individual zone monitoring (4 sensors)
- 💧 **Water Level**: Tank level with percentage calculation

### System Health
- ⚡ **Power Consumption**: Real-time current and voltage monitoring
- 📶 **WiFi Signal**: Connection strength monitoring
- 🔋 **Battery Status**: Voltage and estimated remaining capacity
- 📊 **System Status**: Component health and error reporting

### Irrigation Status
- 🚰 **Active Plant**: Currently watering plant indicator
- ⏱️ **Remaining Time**: Time left in current watering cycle
- 📊 **Daily Usage**: Water usage tracking and statistics
- 🔄 **Pump Status**: Real-time pump operation status

## 🔧 Customization Guide

### Sensor Calibration
Each soil moisture sensor requires individual calibration:

```yaml
calibrate_linear:
  - 3.3 -> 0.0    # Sensor in air (dry) = 0% moisture
  - 1.5 -> 100.0  # Sensor in water (wet) = 100% moisture
```

### Watering Schedules
Configure individual plant parameters:

```yaml
sprinkler:
  valves:
    - valve_switch: "Plant 1"
      run_duration: 900s      # 15 minutes
      valve_switch_id: pump_1
```

### Safety Features
Built-in protection systems:

```yaml
# Low water protection
- platform: template
  name: "Low Water Protection"
  lambda: 'return id(nivel_agua_recipiente).state < 10.0;'
```

## 🧪 Testing and Validation

### Step-by-Step Testing
1. **Basic Configuration**: Verify sensor readings in Home Assistant
2. **Enhanced Sensors**: Confirm all environmental data is accurate
3. **Complete System**: Test manual watering and automatic schedules

### Calibration Process
1. **Soil Sensors**: Test in air, then in water, adjust calibration values
2. **Water Level**: Measure tank height, verify percentage calculation
3. **Power Monitoring**: Validate current readings with known loads

## 🔄 Maintenance

### Regular Tasks
- 🧹 **Weekly**: Clean soil sensors, check water levels
- 🔧 **Monthly**: Verify all connections, calibrate sensors if needed
- 🔄 **Quarterly**: Update ESPHome firmware, review system logs

## 🚀 Future Enhancements

### Possible Future Features
- 📷 Camera monitoring for visual plant health assessment
- 🤖 Machine learning for optimal watering patterns
- Light sensors
- Soil nutrients sensors

## 📚 Learning Resources

### ESPHome Documentation
- [Sprinkler Controller](https://esphome.io/components/sprinkler.html)
- [Sensor Components](https://esphome.io/components/sensor/index.html)
- [Switch Components](https://esphome.io/components/switch/index.html)

### Home Assistant Integration
- [ESPHome Integration](https://www.home-assistant.io/integrations/esphome/)
- [Automation Examples](https://www.home-assistant.io/docs/automation/)


## ⚠️ Important Safety Notes

- ✅ Always verify electrical connections before powering the system
- ✅ Use appropriate IP-rated components for wet environments
- ✅ Implement proper grounding for all electrical components
- ✅ Test manual overrides before relying on automatic operation
- ✅ Monitor system operation during initial deployment

## 📄 License

This project is provided for educational purposes. Always verify local electrical and irrigation codes before implementation.

---

**🌱 Happy Growing!** This step-by-step approach helps you build confidence while creating a professional irrigation system that integrates seamlessly with Home Assistant. 