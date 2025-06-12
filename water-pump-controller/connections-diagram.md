# ESP32-S3 Water Pump Controller - Connections Diagram (Test Configuration with Current Monitoring)

## Overview
This document provides complete wiring instructions for connecting an ESP32-S3-WROOM1 N8R16 development board to a 4-channel 5V relay module to control two 3V water pumps **using the ESP32-S3's onboard 3.3V power supply for testing purposes** with **INA226 current monitoring**.

## Components Required

### Main Components
- **ESP32-S3-WROOM1 N8R16 Development Board**
  - 16MB Flash, 8MB PSRAM
  - USB-C connector for power and programming
  - Onboard 3.3V regulator (600mA max output)
- **4-Channel 5V Relay Module with Optocoupler**
  - Operating voltage: 5V DC
  - Trigger voltage: 3.3V-5V
  - Load capacity: 10A 250V AC / 10A 30V DC
- **INA226 Current/Power Monitor Module**
  - I2C interface
  - Bidirectional current sensing
  - 0.1Ω shunt resistor (typical)
  - 3.3V/5V compatible
- **2x 3V Water Pumps**
  - Operating voltage: 3V DC (compatible with 3.3V)
  - Current consumption: **Maximum 200mA each for safe operation**
- **USB Power Supply**
  - 5V, minimum 2A capacity
  - USB-C cable for ESP32-S3

### Additional Components
- **Jumper wires** (Male-to-Female, Male-to-Male)
- **Breadboard** (recommended for INA226 connections)

## Pin Assignments

### ESP32-S3 to Relay Module Connections
| ESP32-S3 Pin | Relay Module Pin | Purpose |
|--------------|------------------|---------|
| GPIO1        | IN1             | Pump 1 Control |
| GPIO2        | IN2             | Pump 2 Control |
| GPIO3        | IN3             | Spare Channel 3 |
| GPIO4        | IN4             | Spare Channel 4 |
| 5V (VIN)     | VCC             | Relay Power |
| GND          | GND             | Common Ground |

### ESP32-S3 to INA226 Connections
| ESP32-S3 Pin | INA226 Pin | Purpose |
|--------------|------------|---------|
| GPIO8        | SDA        | I2C Data |
| GPIO9        | SCL        | I2C Clock |
| 3.3V         | VCC        | INA226 Chip Power |
| 3.3V         | VBUS       | Bus Voltage Reference |
| GND          | GND        | Common Ground |

### INA226 Current Sensing Connections
| INA226 Pin | Connection | Purpose |
|------------|------------|---------|
| VIN+       | ESP32-S3 3.3V | High-side input |
| VIN-       | To Relay COM pins | Low-side output |
| GND        | ESP32-S3 GND | Common Ground |
| VCC        | ESP32-S3 3.3V | Chip Power Supply |
| VBUS       | ESP32-S3 3.3V | Bus Voltage Measurement Reference |

## Wiring Diagram (Test Configuration with Current Monitoring)

```
ESP32-S3-WROOM1-N8R16          4-Channel Relay Module
┌─────────────────────┐        ┌─────────────────────┐
│                     │        │                     │
│  GPIO1 ────────────┼────────┼─► IN1               │
│  GPIO2 ────────────┼────────┼─► IN2               │
│  GPIO3 ────────────┼────────┼─► IN3 (spare)       │
│  GPIO4 ────────────┼────────┼─► IN4 (spare)       │
│                     │        │                     │
│  5V (VIN) ─────────┼────────┼─► VCC               │
│  GND ──────────────┼────────┼─► GND ──────────────┼──┐
│                     │        │                     │  │
│  3.3V ─────────────┼────────┼─────────────────────┼──┼─► INA226 VIN+
│  3.3V ─────────────┼────────┼─────────────────────┼──┼─► INA226 VCC
│  3.3V ─────────────┼────────┼─────────────────────┼──┼─► INA226 VBUS
│                     │   ┌────┼─────────────────────┼──┘
│  GPIO8 (SDA) ──────┼───┼────┼─────────────────────┼──► INA226 SDA
│  GPIO9 (SCL) ──────┼───┼────┼─────────────────────┼──► INA226 SCL
│                     │   │    │                     │
│  USB-C ◄───USB Power Supply  │                     │
└─────────────────────┘   │    └─────────────────────┘
                          │             │
                    ┌─────▼─────┐       │
                    │  INA226   │       │
                    │  Current  │       │
                    │  Monitor  │       │
                    │           │       │
                    │ VIN- ─────┼───────┼────┐
                    │ GND ──────┼───────┼────┼───┐
                    │ VCC ──────┼───────┘    │   │
                    │ VBUS ─────┼────────────┘   │
                    └───────────┘                │
                                    ┌────────┴───┼───┐
                                    │                │   │
                               ┌────▼───┐       ┌────▼───┼───┐
                               │ CH1    │       │ CH2    │   │
                               │ COM/NO │       │ COM/NO │   │
                               └────┬───┘       └────┬───┘   │
                                    │                │       │
                             ┌──────▼──────┐  ┌──────▼──────┐│
                             │  3V Pump 1  │  │  3V Pump 2  ││
                             │     (+)     │  │     (+)     ││
                             └─────┬───────┘  └─────┬───────┘│
                                   │                │        │
                                   └────────┬───────┘        │
                                            │                │
                                   ┌────────▼────────────────▼┘
                                   │     ESP32-S3 GND        │
                                   │         (-)             │
                                   └─────────────────────────┘
```

## Detailed Connection Instructions

### Step 1: ESP32-S3 to Relay Module
1. **Power Connections:**
   - Connect ESP32-S3 **5V (VIN)** to Relay Module **VCC**
   - Connect ESP32-S3 **GND** to Relay Module **GND**

2. **Control Signal Connections:**
   - Connect ESP32-S3 **GPIO1** to Relay Module **IN1** (Pump 1)
   - Connect ESP32-S3 **GPIO2** to Relay Module **IN2** (Pump 2)
   - Connect ESP32-S3 **GPIO3** to Relay Module **IN3** (Spare)
   - Connect ESP32-S3 **GPIO4** to Relay Module **IN4** (Spare)

### Step 2: ESP32-S3 to INA226 Current Monitor
1. **I2C Connections:**
   - Connect ESP32-S3 **GPIO8** to INA226 **SDA**
   - Connect ESP32-S3 **GPIO9** to INA226 **SCL**

2. **Power Connections (CRITICAL - Three connections needed):**
   - Connect ESP32-S3 **3.3V** to INA226 **VCC** (chip power supply)
   - Connect ESP32-S3 **3.3V** to INA226 **VBUS** (bus voltage reference)
   - Connect ESP32-S3 **GND** to INA226 **GND**

### Step 3: INA226 Current Sensing Setup
1. **High-Side Connection:**
   - Connect ESP32-S3 **3.3V** to INA226 **VIN+**

2. **Low-Side Connection (Current Measurement Path):**
   - Connect INA226 **VIN-** to both **CH1 COM** and **CH2 COM** on relay module

### Step 3.1: INA226 Complete Connection Summary
**CRITICAL: The INA226 requires FOUR connections to 3.3V rail:**
1. **VCC** ← 3.3V (chip power supply)
2. **VBUS** ← 3.3V (bus voltage measurement reference)  
3. **VIN+** ← 3.3V (high-side current measurement)
4. **GND** ← GND (common ground)

**Current measurement path:**
5. **VIN-** → Relay COM pins (low-side current measurement)

### Step 4: Relay Module to Water Pumps (Current Monitored)
1. **Pump 1 Connection:**
   - Connect **CH1 NO** to **Pump 1 Positive (+)**
   - Connect **Pump 1 Negative (-)** to **ESP32-S3 GND**

2. **Pump 2 Connection:**
   - Connect **CH2 NO** to **Pump 2 Positive (+)**
   - Connect **Pump 2 Negative (-)** to **ESP32-S3 GND**

### Step 5: Power Supply Connections
1. **ESP32-S3 Power:**
   - Connect USB Power Supply to ESP32-S3 USB-C port

## How Current Monitoring Works

### Current Flow Path
1. **3.3V from ESP32-S3** → **INA226 VIN+**
2. **INA226 VIN-** → **Relay COM pins**
3. **Relay NO pins** → **Pump positive terminals** (when activated)
4. **Pump negative terminals** → **ESP32-S3 GND**

### Bus Voltage Reference Path
- **3.3V from ESP32-S3** → **INA226 VBUS** (voltage measurement reference)

### What Gets Measured
- **Current:** Total current flowing to both pumps (via VIN+/VIN- shunt)
- **Bus Voltage:** Measured at VBUS pin (should be ~3.3V)
- **Power:** Calculated power consumption (Bus Voltage × Current)
- **Shunt Voltage:** Voltage drop across 0.1Ω shunt (for debugging)

## INA226 Configuration Details

### I2C Address
- **Default:** 0x40
- **Alternative addresses:** 0x41-0x4F (by bridging A0, A1 pins)

### Measurement Settings
- **Shunt Resistance:** 0.1Ω (typical for INA226 modules)
- **Max Current:** 0.8A (safety limit for 3.3V rail)
- **Update Rate:** 1 second
- **Averaging:** 2-second throttle for stable readings

### Current Thresholds
- **IDLE:** < 0.05A (no pumps running)
- **NORMAL:** 0.05A - 0.3A (typical single pump operation)
- **MODERATE:** 0.3A - 0.5A (dual pump operation)
- **HIGH:** > 0.5A (overcurrent condition - alert triggered)

## Safety Considerations (Enhanced with Monitoring)

### Electrical Safety
- **Current Monitoring:** Real-time current measurement prevents overload
- **Overcurrent Alert:** Automatic alert when current exceeds 0.5A
- **Voltage Monitoring:** Tracks 3.3V rail stability
- **Safety Margin:** 0.8A maximum configured (vs 0.6A ESP32-S3 limit)

### Current Monitoring Benefits
- **Real-time Feedback:** See actual pump current consumption
- **Trend Analysis:** Monitor current over time
- **Fault Detection:** Detect pump malfunctions or blockages
- **System Health:** Verify 3.3V rail stability

## Testing Procedure (Enhanced with Current Monitoring)

### Initial Current Verification
1. **Baseline Measurement:**
   - Power on system without pumps connected
   - Verify idle current < 0.05A
   - Check bus voltage = ~3.3V

2. **Single Pump Test:**
   - Connect one pump
   - Activate pump and monitor current
   - Verify current within expected range (50-200mA)

3. **Dual Pump Test:**
   - Connect second pump
   - Test simultaneous operation
   - Monitor total current (should be < 400mA)

### Current Analysis
1. **Expected Current Ranges:**
   - **Idle:** 0-50mA (system overhead)
   - **Single Pump:** 50-200mA
   - **Dual Pump:** 100-400mA
   - **Alert Threshold:** >500mA

2. **Voltage Drop Analysis:**
   - Monitor bus voltage during pump operation
   - Voltage should remain > 3.0V
   - Significant drops indicate overload

## Home Assistant Monitoring

### Available Sensors
- **`sensor.pump_current`** - Real-time current consumption (A)
- **`sensor.pump_power`** - Power consumption (W)
- **`sensor.pump_voltage`** - Bus voltage (V)
- **`sensor.current_status`** - Text status (IDLE/NORMAL/MODERATE/HIGH)
- **`binary_sensor.overcurrent_alert`** - Alert when current > 0.5A

### Monitoring Dashboard Suggestions
1. **Current Graph:** Track current over time
2. **Power Consumption:** Monitor total system power
3. **Voltage Stability:** Ensure stable 3.3V supply
4. **Alert Integration:** Notifications for overcurrent conditions

## Troubleshooting (Enhanced with Current Monitoring)

### Current-Related Issues

#### High Current Reading (>0.5A)
- **Check pump condition:** Blocked or seized pumps draw excessive current
- **Verify connections:** Ensure proper wiring
- **Test pumps individually:** Isolate problematic pump

#### No Current Reading
- **Check I2C connections:** Verify SDA/SCL wiring
- **Verify INA226 address:** Scan I2C bus (scan: true in config)
- **Check current path:** Ensure current flows through VIN+/VIN-

#### Inconsistent Readings
- **Check connections:** Ensure solid connections
- **Verify shunt resistance:** Should be 0.1Ω
- **Check averaging settings:** Increase throttle_average if needed

### Voltage Drop Issues
- **Monitor bus voltage during operation**
- **If voltage drops below 3.0V:** Reduce pump load
- **Check USB power supply capacity**

## Component Specifications (Updated with INA226)

### INA226 Current Monitor
- **Interface:** I2C (3.3V/5V compatible)
- **Current Range:** ±819.2mA (with 0.1Ω shunt)
- **Resolution:** 2.5µA per LSB
- **Accuracy:** ±0.1% (typical)
- **Update Rate:** Configurable (1ms to 8.244s)
- **Address Range:** 0x40-0x4F (configurable)

### Power Consumption Analysis
- **ESP32-S3 Base:** ~80-150mA
- **Relay Module:** ~60mA (4 channels idle)
- **INA226:** ~1mA
- **Single Pump:** 50-200mA
- **Dual Pumps:** 100-400mA
- ****Total System:** 250-650mA (within USB supply limits)

## Important Notes (Updated)

### Current Monitoring Benefits
- ✅ **Real-time current monitoring**
- ✅ **Overcurrent protection**
- ✅ **System health monitoring**
- ✅ **Fault detection capabilities**
- ✅ **Data logging for analysis**

### System Limitations
- ❌ **Still limited to 400mA total pump current**
- ❌ **Short-duration operation only (1-second pulses)**
- ❌ **Not suitable for continuous operation**

### Production Upgrade Path
When upgrading to external power supply:
1. **Relocate current monitoring** to external 3V supply rail
2. **Keep same INA226 configuration**
3. **Update current thresholds** for higher capacity
4. **Maintain all monitoring features**

This enhanced configuration provides comprehensive current monitoring while maintaining the simple test setup, giving you full visibility into your pump current consumption and system health. 