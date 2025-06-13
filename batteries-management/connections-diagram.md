# Battery Monitor ESP32-S3 - Connections Diagram (Actualizado)

## Sistema y Esquema de Conexión INA226

Este sistema monitorea dos baterías 26650 en serie usando un ESP32-S3-WROOM1 y un sensor INA226 para medición precisa de corriente y voltaje, reportando a Home Assistant.

---

## Esquema de Conexión INA226 (High-Side Sensing)

```
Batería + (Pack) ─────┬───── VIN+ (INA226)
                      │
                      ├───── VBUS (INA226)
                      │
                  [Shunt R1: 0.1Ω]
                      │
                      └───── VIN- (INA226)
                            │
                            └─── Hacia carga
                            
Batería - (Pack) ─────────── GND (Sistema, ESP32, INA226)
```

**Basado en el esquema del PCB:**
- **IN+** (Pin 10) → Conecta al positivo de la batería (antes del shunt R1)
- **VBUS** (Pin 5) → Conecta al positivo de la batería (mismo punto que IN+)
- **Shunt R1 (0.1Ω)** → Entre IN+ e IN-
- **IN-** (Pin 9) → Después del shunt, hacia la carga
- **Batería -** → GND del sistema
- **VCC** (Pin 6) → 3.3V o 5V del ESP32
- **GND** (Pin 8) → GND común del sistema
- **SDA** (Pin 4) → GPIO del ESP32 para I2C
- **SCL** (Pin 3) → GPIO del ESP32 para I2C

---

## Tabla de Conexiones según PCB (Header P1)

| Pin P1 | Señal  | Pin INA226 | Conectar a                       | Notas                        |
|--------|--------|------------|----------------------------------|------------------------------|
| 1      | VCC    | Pin 6      | 3.3V o 5V del ESP32             | Alimentación del sensor      |
| 2      | GND    | Pin 8      | GND común del sistema            | Tierra común                 |
| 3      | SCL    | Pin 3      | GPIO9 del ESP32 (ejemplo)        | I2C Clock                    |
| 4      | SDA    | Pin 4      | GPIO8 del ESP32 (ejemplo)        | I2C Data                     |
| 5      | ALERT  | Pin 1      | GPIO opcional del ESP32          | Alerta programable           |
| 6      | VBUS   | Pin 5      | Batería + (antes del shunt)      | Voltaje del bus              |
| 7      | IN-    | Pin 9      | Después del shunt R1             | Entrada negativa             |
| 8      | IN+    | Pin 10     | Batería + (antes del shunt)      | Entrada positiva             |

---

## Configuración de Dirección I2C

El PCB incluye jumpers soldables (SJ1) para configurar la dirección I2C:
- **A1** y **A0** con resistencias pull-up R2, R3, R4, R5, R6 (10kΩ)
- **Dirección por defecto**: 0x40 (A1=0, A0=0)
- **Otras direcciones posibles**: 0x41, 0x44, 0x45 según configuración de jumpers

---

## Detalle de Pines ESP32-S3-WROOM1

| ESP32-S3 Pin | Función   | Conectado a      | Notas                |
|--------------|-----------|------------------|----------------------|
| GPIO8        | I2C SDA   | P1 Pin 4 (SDA)   | I2C Data             |
| GPIO9        | I2C SCL   | P1 Pin 3 (SCL)   | I2C Clock            |
| 3.3V         | Power     | P1 Pin 1 (VCC)   | Sensor power         |
| GND          | Ground    | P1 Pin 2 (GND)   | System ground        |
| GPIO48       | Status LED| LED interna      | Opcional             |
| USB/VIN      | Power In  | 5V de buck       | Fuente ESP32         |
| GPIO_X       | Alert     | P1 Pin 5 (ALERT) | Opcional - Alertas   |

---

## Componentes del PCB

- **U1**: INA226 (Sensor de corriente y voltaje)
- **R1**: Shunt de 0.1Ω para medición de corriente
- **R2-R6**: Resistencias pull-up de 10kΩ para I2C y configuración
- **C1**: Capacitor de desacoplamiento 100nF
- **P1**: Header de 8 pines para conexiones externas
- **SJ1**: Jumper soldable para configuración de dirección I2C

---

## Notas Importantes del PCB
- **VBUS e IN+** están conectados internamente al mismo punto (positivo de batería)
- **El shunt R1** está integrado en el PCB entre IN+ e IN-
- **Las resistencias pull-up** para I2C ya están incluidas en el PCB
- **El capacitor C1** proporciona estabilidad de alimentación
- **Los jumpers A0/A1** permiten cambiar la dirección I2C si es necesario

---

## Home Assistant Integration

El dispositivo aparecerá automáticamente en Home Assistant con los siguientes sensores:
- `sensor.battery_level` - Porcentaje de batería (%)
- `sensor.battery_voltage` - Voltaje de batería (V)
- `sensor.battery_current` - Corriente de batería (A)
- `sensor.battery_power` - Potencia de batería (W)
- `binary_sensor.battery_charging` - Estado de carga
- `binary_sensor.low_battery_warning` - Alerta de batería baja
- `text_sensor.battery_status` - Estado general de la batería

---

## Seguridad y Pruebas
- Verifica siempre la polaridad antes de energizar
- Usa fusible de 5A para protección
- Mide con multímetro antes de conectar el ESP32
- El shunt R1 ya está integrado en el PCB (0.1Ω)
- Consulta el datasheet del INA226 para más detalles técnicos 