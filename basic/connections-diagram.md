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

### 3. JSN-SR04T - Sensor Ultrasónico
```
ESP32-S3          JSN-SR04T
GPIO5         →   Trig
GPIO4         →   Echo
GND           →   GND
5V            →   VCC
```

### 4. Sensores de Humedad del Suelo Capacitivos
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
| GPIO8    | SDA     | I2C Bus (ADS1115) |
| GPIO9    | SCL     | I2C Bus (ADS1115) |

## Notas de Implementación

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

1. **Nivel Agua Recipiente** (%)
2. **Humedad Suelo Sensor 1** (%)
3. **Humedad Suelo Sensor 2** (%)
4. **WiFi Signal Strength** (dBm)
5. **Status** (binary sensor)

## Instrucciones de Configuración

1. **Editar credenciales WiFi** en `esp32-s3-basico.yaml`:
   - Cambiar `TU_WIFI_SSID` por tu red WiFi
   - Cambiar `TU_WIFI_PASSWORD` por tu contraseña WiFi

2. **Generar clave de encriptación API**:
   - Ejecutar: `esphome config.yaml secrets`
   - O usar: https://esphome.io/components/api.html#encryption

3. **Calibrar sensores de humedad**:
   - Medir voltaje en aire seco y en agua
   - Ajustar valores en `calibrate_linear`

4. **Ajustar altura del recipiente**:
   - Modificar `altura_recipiente` en el filtro lambda del JSN-SR04T

## Comandos de Compilación

```bash
# Compilar y subir
esphome run esp32-s3-basico.yaml

# Solo compilar
esphome compile esp32-s3-basico.yaml

# Ver logs
esphome logs esp32-s3-basico.yaml
``` 