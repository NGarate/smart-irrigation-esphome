# Diagrama de Conexiones - Sistema de Riego Automático

## Esquema General del Sistema

```
[Baterías 26650] → [Buck Converter] → [ESP32-S3] + [Sensores] + [Relés] → [Bombas]
                                    ↓
                              [WiFi] → [Home Assistant]
```

## Alimentación Principal

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Batería 1     │    │                  │    │                 │
│    26650        │◄──►│   Buck Converter │◄──►│     ESP32-S3    │
│   3.7V / 5000mAh│    │    LM2596        │    │       3.3V      │
└─────────────────┘    │   3V-40V → 1.5V  │    └─────────────────┘
┌─────────────────┐    │      -35V        │
│   Batería 2     │    │                  │
│    26650        │◄──►│   Ajustado a 3.3V│
│   3.7V / 5000mAh│    └──────────────────┘
└─────────────────┘
```

## Conexiones ESP32-S3

### Comunicación I2C (Sensores Digitales)
```
ESP32-S3          INA226 (Monitor Potencia)
GPIO21 (SDA) ────► SDA
GPIO22 (SCL) ────► SCL
3.3V ─────────────► VCC
GND ──────────────► GND
                   Dirección: 0x40
```

### Comunicación SPI (ADS1115)
```
ESP32-S3          ADS1115 (ADC 16-bit)
GPIO18 (CLK) ────► SCL
GPIO23 (MOSI) ───► SDA  
GPIO19 (MISO) ───► ADDR (a GND para 0x48)
3.3V ─────────────► VDD
GND ──────────────► GND
```

### Sensores Analógicos (Humedad Suelo)
```
ADS1115                    Sensores Capacitivos
A0 ─────────────────────► Sensor Humedad Planta 1
A1 ─────────────────────► Sensor Humedad Planta 2  
A2 ─────────────────────► Sensor Humedad Planta 3
A3 ─────────────────────► Sensor Humedad Planta 4

Cada sensor:
VCC ────► 3.3V
GND ────► GND
```

### Sensor Digital DHT22
```
ESP32-S3          DHT22 (Temp/Humedad)
GPIO4 ───────────► Data
3.3V ─────────────► VCC  
GND ──────────────► GND
```

### Sensor Ultrasónico JSN-SR04T
```
ESP32-S3          JSN-SR04T (Nivel Agua)
GPIO5 ───────────► Trigger
GPIO17 ──────────► Echo
5V ───────────────► VCC (usar Buck converter a 5V)
GND ──────────────► GND
```

## Sistema de Control de Bombas

### Módulo Relés 4 Canales
```
ESP32-S3          Módulo Relés (5V)
GPIO32 ──────────► IN1 (Control Bomba 1)
GPIO33 ──────────► IN2 (Control Bomba 2)
GPIO25 ──────────► IN3 (Control Bomba 3)
GPIO26 ──────────► IN4 (Control Bomba 4)
5V ───────────────► VCC (del Buck converter)
GND ──────────────► GND
```

### Conexión de Bombas
```
Relé 1 (NO-COM)    Bomba Planta 1 (3V)
VCC+ ─────────────► VCC
COM ──────────────► GND

Relé 2 (NO-COM)    Bomba Planta 2 (3V)  
VCC+ ─────────────► VCC
COM ──────────────► GND

Relé 3 (NO-COM)    Bomba Planta 3 (3V)
VCC+ ─────────────► VCC  
COM ──────────────► GND

Relé 4 (NO-COM)    Bomba Planta 4 (3V)
VCC+ ─────────────► VCC
COM ──────────────► GND
```

## Distribución de Voltajes

```
┌─────────────────┐
│ Buck Converter  │
│   Salidas:      │
├─────────────────┤
│ 3.3V ──► ESP32  │ 
│      ──► DHT22  │
│      ──► INA226 │
│      ──► ADS1115│
│      ──► Sensores│
├─────────────────┤
│ 5V ───► JSN-SR04T│
│    ───► Módulo   │
│         Relés   │
├─────────────────┤  
│ 3V ───► Bombas  │
│         (x4)    │
└─────────────────┘
```

## Layout Físico Recomendado

```
┌──────────────────────────────────────────┐
│                CAJA PRINCIPAL            │
├──────────────────────────────────────────┤
│  [Bat1] [Bat2]    [Buck Conv]   [ESP32]  │
│                                          │
│  [INA226] [ADS1115]  [Módulo Relés 4Ch] │
│                                          │
│  [DHT22]                    [Conexiones] │
└──────────────────────────────────────────┘
                    │
    ┌───────────────┼───────────────┐
    │               │               │
    ▼               ▼               ▼
[Depósito]    [Plantas 1-4]   [JSN-SR04T]
[con Agua]    [c/Sensores]    [en Depósito]
```

## Cableado Detallado por Componente

### INA226 (Monitoreo de Potencia)
| Pin | Conexión | Descripción |
|-----|----------|-------------|
| VCC | 3.3V     | Alimentación |
| GND | GND      | Tierra |
| SDA | GPIO21   | Datos I2C |
| SCL | GPIO22   | Reloj I2C |
| VIN+ | VCC Buck | Entrada voltaje a medir |
| VIN- | GND Buck | Referencia voltaje |

### ADS1115 (Convertidor ADC)
| Pin | Conexión | Descripción |
|-----|----------|-------------|
| VDD | 3.3V     | Alimentación |
| GND | GND      | Tierra |
| SCL | GPIO18   | Reloj SPI |
| SDA | GPIO23   | Datos SPI |
| A0  | Sensor 1 | Humedad Planta 1 |
| A1  | Sensor 2 | Humedad Planta 2 |
| A2  | Sensor 3 | Humedad Planta 3 |
| A3  | Sensor 4 | Humedad Planta 4 |

### DHT22 (Temperatura/Humedad)
| Pin | Conexión | Descripción |
|-----|----------|-------------|
| VCC | 3.3V     | Alimentación |
| GND | GND      | Tierra |
| Data| GPIO4    | Datos digitales |

### JSN-SR04T (Ultrasónico)
| Pin | Conexión | Descripción |
|-----|----------|-------------|
| VCC | 5V       | Alimentación |
| GND | GND      | Tierra |
| Trig| GPIO5    | Trigger |
| Echo| GPIO17   | Echo |

### Módulo Relés 4 Canales
| Pin | Conexión | Descripción |
|-----|----------|-------------|
| VCC | 5V       | Alimentación |
| GND | GND      | Tierra |
| IN1 | GPIO32   | Control Bomba 1 |
| IN2 | GPIO33   | Control Bomba 2 |
| IN3 | GPIO25   | Control Bomba 3 |
| IN4 | GPIO26   | Control Bomba 4 |

## Notas de Seguridad

⚠️ **Importante:**
- Usar cables apropiados para cada voltaje
- Proteger conexiones de la humedad
- Verificar polaridad antes de conectar
- Usar fusibles en líneas de alimentación
- Aislar conexiones eléctricas del agua

## Lista de Cables Necesarios

### Para Conexiones:
- Cable dupont hembra-hembra (20cm) x20
- Cable dupont macho-hembra (15cm) x15  
- Cable rígido AWG22 para alimentación
- Cable flexible AWG24 para señales
- Conectores JST-XH para sensores
- Conectores de tornillo para relés

### Para Sistema Hidráulico:
- Manguera 4mm diámetro interior x 5m
- Conectores T para manguera 4mm x4
- Conectores rectos 4mm x8
- Abrazaderas pequeñas x10

## Herramientas Requeridas

- Soldador y estaño
- Multímetro
- Pelacables
- Destornilladores pequeños
- Pistola de calor (termocontráctil)
- Taladro (para caja de proyecto) 