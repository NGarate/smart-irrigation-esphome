# ESP32-S3 4-Plant WiFi Irrigation System - Connections Diagram

## 🔌 Overview

This document provides detailed wiring instructions for the ESP32-S3 based 4-plant irrigation system with WiFi connectivity and Home Assistant integration.

## 📋 Components List

### Main Controller
- **ESP32-S3 N16R8 WROOM-1 DevKit** - Main microcontroller with WiFi and Bluetooth
- **4-Channel Relay Module** - 5V relay board for pump control
- **Power Supply** - 5V/3A for system power

### Sensors
- **4x Capacitive Soil Moisture Sensors** - Analog soil moisture detection
- **1x ADS1115 ADC Module** - 16-bit ADC for precise moisture sensor readings
- **1x JSN-SR04T Ultrasonic Sensor** - Waterproof distance sensor for water tank level

### Actuators
- **4x 3V Water Pumps** - Small submersible pumps for each plant
- **Connecting wires** - Dupont jumpers and hookup wire

## 🔗 Complete Wiring Diagram

```
ESP32-S3 N16R8 WROOM-1 DevKit
┌──────────────────────────────────────┐
│                                      │
│  ┌─────┐  ┌─────┐  ┌─────┐  ┌─────┐  │
│  │ USB │  │ RST │  │ BOOT│  │ EN  │  │
│  └─────┘  └─────┘  └─────┘  └─────┘  │
│                                      │
│ GPIO3  ○───────────────────────────○ │ → JSN-SR04T Echo Pin
│ GPI46  ○───────────────────────────○ │ → JSN-SR04T Trigger Pin
│ GPIO6  ○                           │ │
│ GPIO7  ○                           │ │
│ GPIO8  ○───────────────────────────○ │ → ADS1115 SDA (I2C Data)
│ GPIO9  ○───────────────────────────○ │ → ADS1115 SCL (I2C Clock)
│ GPIO10 ○                           │ │
│ GPIO11 ○───────────────────────────○ │ → Relay CH1 (Plant 1 Pump)
│ GPIO12 ○───────────────────────────○ │ → Relay CH2 (Plant 2 Pump)
│ GPIO13 ○───────────────────────────○ │ → Relay CH3 (Plant 3 Pump)
│ GPIO14 ○───────────────────────────○ │ → Relay CH4 (Plant 4 Pump)
│                                      │
│ 5V     ○───────────────────────────○ │ → Relay Module & Water Tank Level Sensor VCC 
│ GND    ○───────────────────────────○ │ → Common Ground
│                                      |
└──────────────────────────────────────┘

ADS1115 ADC Module
┌─────────────────────┐
│ A0 ○ A1 ○ A2 ○ A3 ○ │ → Soil Sensors 1-4 (Analog)
│ SDA○ SCL○ VCC○ GND○ │
└─────────────────────┘
     │    │    │    │
     │    │   3.3V GND
     │   GPIO9 (SCL)
    GPIO8 (SDA)
```

## 📊 Detailed Connections Table

### Power Connections
| ESP32-S3 Pin | Component | Wire Color | Notes |
|--------------|-----------|------------|--------|
| 5V | Relay Module VCC | Red | 5V power for relays |
| 3.3V | All Sensors VCC | Red | 3.3V for sensors |
| GND | Common Ground | Black | Shared ground |

### ADS1115 ADC Module (I2C)
| ESP32-S3 Pin | ADS1115 Pin | Wire Color | Function |
|--------------|-------------|------------|----------|
| GPIO8 | SDA | Green | I2C Data |
| GPIO9 | SCL | Yellow | I2C Clock |
| 3.3V | VCC | Red | Power supply |
| GND | GND | Black | Ground |

### Soil Moisture Sensors (Capacitive) → ADS1115
| ADS1115 Pin | Sensor | Component Pin | Wire Color |
|-------------|--------|---------------|------------|
| A0 | Plant 1 Sensor | AOUT | Blue |
| A1 | Plant 2 Sensor | AOUT | Yellow |
| A2 | Plant 3 Sensor | AOUT | Green |
| A3 | Plant 4 Sensor | AOUT | White |
| VCC | All Sensors | VCC | Red |
| GND | All Sensors | GND | Black |

### Water Tank Level Sensor (JSN-SR04T)
| ESP32-S3 Pin | JSN-SR04T Pin | Wire Color | Function |
|--------------|---------------|------------|----------|
| GPI03 | Trig(RX) | Yellow | Trigger pulse |
| GPI46 | Echo(TX) | Blue | Echo return |
| 5V | VCC | Red | Power supply |
| GND | GND | Black | Ground |

### 4-Channel Relay Module
| ESP32-S3 Pin | Relay Pin | Wire Color | Controls |
|--------------|-----------|------------|----------|
| GPIO11 | IN1 | Blue | Plant 1 Pump |
| GPIO12 | IN2 | Yellow | Plant 2 Pump |
| GPIO13 | IN3 | Green | Plant 3 Pump |
| GPIO14 | IN4 | White | Plant 4 Pump |
| 5V | VCC | Red | Relay power |
| GND | GND | Black | Ground |

### Water Pumps (3V DC)
| Pump | Relay Channel | Connection | Notes |
|------|---------------|------------|--------|
| Plant 1 Pump | Relay 1 | COM ↔ NO | Normally Open contact |
| Plant 2 Pump | Relay 2 | COM ↔ NO | Normally Open contact |
| Plant 3 Pump | Relay 3 | COM ↔ NO | Normally Open contact |
| Plant 4 Pump | Relay 4 | COM ↔ NO | Normally Open contact |

## 🔧 Component Specifications

### ESP32-S3 N16R8 WROOM-1 DevKit
- **MCU**: ESP32-S3 (Xtensa dual-core LX7)
- **Built-in**: WiFi 802.11 b/g/n, Bluetooth 5 LE
- **Flash**: 16MB
- **PSRAM**: 8MB
- **RAM**: 512KB SRAM
- **GPIO**: 45 digital I/O pins
- **ADC**: 12-bit, multiple channels
- **Operating Voltage**: 3.3V
- **Input Voltage**: 5V (USB) or 3.3V-3.6V (pins)

### ADS1115 ADC Module
- **Type**: External 16-bit ADC with built-in pull-up resistors
- **Interface**: I2C (3.3V/5V compatible)
- **Channels**: 4 single-ended or 2 differential
- **Resolution**: 16-bit (65,536 levels)
- **Sample Rate**: Up to 860 SPS
- **Address**: 0x48 (default, ADDR pin to GND)
- **Gain Setting**: ±6.144V (configurable)
- **Accuracy**: Superior to ESP32 built-in ADC
- **Built-in Features**: Pull-up resistors, voltage reference

### Capacitive Soil Moisture Sensors
- **Type**: Capacitive (corrosion-resistant)
- **Operating Voltage**: 3.3V-5V (via ADS1115)
- **Output**: Analog voltage (0-3.3V) → ADS1115
- **Measurement Range**: 0-100% soil moisture
- **Response Time**: ~1 second
- **Advantages**: No corrosion, long-lasting
- **Connection**: Via ADS1115 for improved accuracy

### JSN-SR04T Ultrasonic Sensor
- **Type**: Waterproof ultrasonic distance sensor
- **Operating Voltage**: 3.3V-5V
- **Measurement Range**: 20cm - 600cm
- **Accuracy**: ±1cm
- **Frequency**: 40kHz
- **Beam Angle**: 75°
- **Waterproof Rating**: IP67

### 4-Channel Relay Module
- **Operating Voltage**: 5V
- **Relay Type**: SPDT (Single Pole Double Throw)
- **Contact Rating**: 10A @ 250V AC, 10A @ 30V DC
- **Trigger**: Active LOW (inverted logic)
- **Isolation**: Optocoupler isolated
- **LED Indicators**: Power and relay status

### 3V Water Pumps
- **Operating Voltage**: 3V DC
- **Current**: 50-200mA per pump
- **Flow Rate**: ~1 L/min
- **Type**: Submersible micro pump
- **Tubing**: 4mm inner diameter

## 🔌 Physical Assembly Instructions

### Step 1: Prepare the ESP32-S3
1. **Initial Setup**: Connect ESP32-S3 to computer via USB
2. **Flash Firmware**: Upload the ESPHome configuration
3. **Test Connection**: Verify WiFi connectivity

### Step 2: Wire Soil Moisture Sensors
1. **Sensor 1**: Connect to GPIO0 (analog), 3.3V, and GND
2. **Sensor 2**: Connect to GPIO1 (analog), 3.3V, and GND
3. **Sensor 3**: Connect to GPIO2 (analog), 3.3V, and GND
4. **Sensor 4**: Connect to GPIO3 (analog), 3.3V, and GND
5. **Calibration**: Place sensors in dry soil and water for calibration

### Step 3: Wire Water Tank Sensor
1. **Trigger Pin**: GPIO5 to JSN-SR04T Trig
2. **Echo Pin**: GPIO4 to JSN-SR04T Echo
3. **Power**: 3.3V and GND to JSN-SR04T
4. **Mounting**: Install sensor at top of water tank, pointing down

### Step 4: Connect ADS1115 ADC Module
1. **I2C Connections**:
   - GPIO8 → ADS1115 SDA
   - GPIO9 → ADS1115 SCL
   - 3.3V → ADS1115 VCC
   - GND → ADS1115 GND
2. **Sensor Connections**: Connect soil moisture sensors to ADS1115 A0-A3
3. **Address**: Default address 0x48 (ADDR pin to GND)

### Step 5: Connect Relay Module
1. **Control Signals**: 
   - GPIO12 → Relay IN1 (Plant 1)
   - GPIO13 → Relay IN2 (Plant 2)
   - GPIO14 → Relay IN3 (Plant 3)
   - GPIO21 → Relay IN4 (Plant 4)
2. **Power**: 5V and GND to relay module VCC and GND
3. **Pump Wiring**: Connect pumps to relay outputs (COM and NO pins)

### Step 6: Power System Wiring
1. **Main Power**: Connect 5V/3A power supply
2. **Distribution**: 
   - 5V → ESP32-S3 VIN and Relay Module VCC
   - 3.3V → ADS1115 and JSN-SR04T VCC pins
   - GND → Common ground for all components

## 📡 WiFi Configuration

### WiFi Network Setup
1. **Router**: Ensure your WiFi router is working and accessible
2. **Home Assistant**: ESPHome integration should be installed
3. **Network Discovery**: ESP32-S3 will connect to your WiFi network
4. **Auto Discovery**: Device should appear in Home Assistant automatically

### Network Parameters
- **Network**: IEEE 802.11ax (WiFi 6)
- **Frequency**: 2.4 GHz
- **Range**: 30-100m (depending on environment and router)
- **Connection**: Direct connection to your home network
- **API**: Encrypted communication with Home Assistant

## 🏠 Home Assistant Integration

### Available Entities

#### Sensors
- `sensor.plant_1_soil_moisture` - Plant 1 soil moisture percentage
- `sensor.plant_2_soil_moisture` - Plant 2 soil moisture percentage  
- `sensor.plant_3_soil_moisture` - Plant 3 soil moisture percentage
- `sensor.plant_4_soil_moisture` - Plant 4 soil moisture percentage
- `sensor.water_tank_level` - Water tank level percentage
- `sensor.system_uptime` - Device uptime

#### Controls (Number Inputs)
- `number.plant_X_moisture_threshold` - Moisture threshold for watering (10-80%)
- `number.plant_X_pump_duration` - Pump run time in seconds (5-300s)
- `number.plant_X_recheck_interval` - Time between checks in seconds (300-86400s)
- `number.plant_X_calibration_dry` - Dry soil voltage for calibration (0-3.3V)
- `number.plant_X_calibration_wet` - Wet soil voltage for calibration (0-3.3V)

#### Manual Controls (Buttons)
- `button.water_plant_1_manual` - Manual watering for Plant 1
- `button.water_plant_2_manual` - Manual watering for Plant 2
- `button.water_plant_3_manual` - Manual watering for Plant 3  
- `button.water_plant_4_manual` - Manual watering for Plant 4
- `button.restart_system` - Restart the ESP32-C6

#### Status Indicators (Binary Sensors)
- `binary_sensor.low_water_level` - Water tank low warning
- `binary_sensor.plant_X_needs_water` - Individual plant watering indicators

## ⚙️ Configuration and Calibration

### Initial Setup
1. **Physical Installation**: Mount sensors in soil, position tank sensor
2. **Power On**: Connect power and verify all components receive power
3. **WiFi Connection**: Device will connect to your WiFi network automatically
4. **Home Assistant Discovery**: Device should appear in ESPHome integration automatically

### Sensor Calibration
1. **Access Controls**: Find calibration number inputs in Home Assistant
2. **Dry Calibration**: 
   - Remove sensor from soil (air dry)
   - Note voltage reading
   - Set "Calibration Dry" value
3. **Wet Calibration**:
   - Place sensor in water
   - Note voltage reading  
   - Set "Calibration Wet" value
4. **Verification**: Check that moisture readings make sense (0-100%)

### Automatic Watering Setup
1. **Moisture Thresholds**: Set desired soil moisture level (typically 30-50%)
2. **Pump Duration**: Set watering time (start with 30 seconds)
3. **Recheck Interval**: Set time between checks (start with 1 hour = 3600 seconds)
4. **Monitor**: Watch system operation and adjust as needed

## 🔍 Troubleshooting

### WiFi Connection Issues
- **Symptom**: Device not appearing in Home Assistant
- **Solution**: 
  - Verify WiFi credentials in secrets.yaml are correct
  - Check WiFi signal strength at device location
  - Restart Home Assistant ESPHome integration
  - Check router settings and firewall

### Sensor Reading Problems
- **Symptom**: Incorrect moisture readings
- **Solution**:
  - Recalibrate sensors using dry/wet method
  - Check wiring connections
  - Verify 3.3V power supply is stable
  - Clean sensor contacts

### Pump Not Activating
- **Symptom**: No water flow when watering triggered
- **Solution**:
  - Check relay wiring (ensure correct NO/COM connections)
  - Verify pump power connections
  - Test relays manually via Home Assistant
  - Check water tank level

### Water Tank Sensor Issues
- **Symptom**: Incorrect or no water level readings
- **Solution**:
  - Check JSN-SR04T wiring (Trig/Echo pins)
  - Verify sensor mounting (facing down, unobstructed)
  - Adjust tank height parameter in configuration
  - Clean sensor face

## 🛡️ Safety Considerations

### Electrical Safety
- **Water Protection**: Keep ESP32-S3 and relay module away from water
- **Power Supply**: Use appropriate 5V/3A power supply
- **Grounding**: Ensure proper grounding of all components
- **Enclosure**: Consider weatherproof enclosure for outdoor use

### System Safety
- **Low Water Protection**: System prevents pumping when tank is low
- **Duration Limits**: Pump duration is limited to prevent overflow
- **Manual Override**: All automatic functions can be manually controlled
- **Monitoring**: Status indicators help identify issues quickly

## 📈 Performance Optimization

### Sensor Accuracy
- **Filtering**: Built-in median filtering reduces noise
- **Update Intervals**: Balanced between responsiveness and power consumption
- **Calibration**: Regular recalibration maintains accuracy

### Power Efficiency
- **Zigbee**: Lower power consumption than WiFi
- **Sleep Modes**: Consider deep sleep for battery operation
- **Efficient Pumps**: 3V pumps chosen for low power consumption

### Network Reliability
- **Mesh Network**: Zigbee provides self-healing mesh
- **Fallback WiFi**: WiFi available for OTA updates and debugging
- **Local Control**: System operates independently of network

## 🔄 Maintenance Schedule

### Weekly
- Check water tank level and refill as needed
- Inspect plant health and growth

### Monthly  
- Clean soil moisture sensors
- Verify pump operation and flow rates
- Check all electrical connections

### Seasonally
- Recalibrate soil moisture sensors
- Update watering schedules for seasonal needs
- Inspect and clean water tank

### Annually
- Replace pumps if showing wear
- Update ESPHome firmware
- Review and optimize watering schedules 