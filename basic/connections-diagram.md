# Diagrama de Conexiones - ESP32-S3-WROOM1 N8R16 - Configuración Básica

## Especificaciones del Hardware

- **Microcontrolador**: ESP32-S3-WROOM1 N8R16
- **Alimentación**: USB 5V
- **Framework**: ESP-IDF
- **Memoria**: 8MB PSRAM, 16MB Flash

## Conexiones de Componentes

### 1. Alimentación
```
USB 5V → ESP32-S3 Dev Board
├── 5V → ADS1115 VDD
├── 5V → JSN-SR04T VCC
└── 3.3V → Sensores de humedad VCC
```

### 2. Bus I2C para ADS1115
```
ESP32-S3          ADS1115
GPIO8 (SDA)   →   SDA
GPIO9 (SCL)   →   SCL
GND           →   GND
3.3V          →   VDD
                  ADDR → GND (dirección I2C: 0x48)
```

### 3. DHT22 - Sensor de Temperatura y Humedad Ambiental
```
ESP32-S3          DHT22
GPIO6         →   DATA
GND           →   GND
3.3V          →   VCC
                  (Resistor 4.7kΩ entre DATA y VCC)
```

### 4. JSN-SR04T - Sensor Ultrasónico
```
ESP32-S3          JSN-SR04T
GPIO5         →   Trig
GPIO4         →   Echo
GND           →   GND
5V            →   VCC
```

### 5. Sensores de Humedad del Suelo Capacitivos
```
ADS1115           Sensor 1        Sensor 2
A0            →   Signal      
A1            →               →   Signal
GND           →   GND         →   GND
3.3V          →   VCC         →   VCC
```

## Mapa de Pines Utilizado

| Pin GPIO | Función | Componente |
|----------|---------|------------|
| GPIO5    | Trigger | JSN-SR04T |
| GPIO4    | Echo    | JSN-SR04T |
| GPIO6    | DATA    | DHT22 |
| GPIO8    | SDA     | I2C Bus (ADS1115) |
| GPIO9    | SCL     | I2C Bus (ADS1115) |

## Notas de Implementación

### DHT22 - Sensor de Temperatura y Humedad
- **Tipo**: Digital, protocolo OneWire
- **Rango de temperatura**: -40°C a +80°C (±0.5°C precisión)
- **Rango de humedad**: 0-100% RH (±2-5% precisión)
- **Alimentación**: 3.3V - 5V
- **Resistor pull-up**: 4.7kΩ entre DATA y VCC (requerido)
- **Tiempo de respuesta**: 2 segundos

### Sensores de Humedad del Suelo
- **Tipo**: Capacitivos (recomendados sobre resistivos)
- **Ventajas**: No se corroen, más duraderos
- **Calibración**: Valores en la configuración necesitan ajuste según sensores específicos
- **Rango típico**: 1.5V (agua) - 3.3V (aire seco)

### JSN-SR04T
- **Rango de medición**: 20cm - 600cm
- **Frecuencia**: 40KHz
- **Precisión**: ±1cm
- **Configurado para**: Recipiente de 20cm de altura (ajustable en código)

### ADS1115
- **Resolución**: 16-bit
- **Canales**: 4 (usando A0 y A1)
- **Gain**: 6.144V (rango de entrada ±6.144V)
- **Dirección I2C**: 0x48 (ADDR a GND)

## Configuración de Home Assistant

Los siguientes sensores aparecerán automáticamente en Home Assistant:

1. **Temperatura Ambiente** (°C) - DHT22
2. **Humedad Ambiente** (%) - DHT22
3. **Nivel Agua Recipiente** (%)
4. **Humedad Suelo Sensor 1** (%)
5. **Humedad Suelo Sensor 2** (%)
6. **WiFi Signal Strength** (dBm)
7. **Status** (binary sensor)

## Instrucciones de Configuración

1. **Configurar secrets.yaml**:
   - Crear archivo `secrets.yaml` con las credenciales WiFi
   - Generar claves API y OTA
   - Configurar contraseñas de punto de acceso

2. **Calibrar sensores de humedad**:
   - Medir voltaje en aire seco y en agua
   - Ajustar valores en `calibrate_linear`

3. **Ajustar altura del recipiente**:
   - Modificar `altura_recipiente` en el filtro lambda del JSN-SR04T

4. **Verificar conexión DHT22**:
   - Asegurar resistor pull-up de 4.7kΩ
   - Verificar conexiones de alimentación y datos

## Comandos de Compilación

```bash
# Compilar y subir
esphome run esp32-s3-config.yaml

# Solo compilar
esphome compile esp32-s3-config.yaml

# Ver logs
esphome logs esp32-s3-config.yaml
```