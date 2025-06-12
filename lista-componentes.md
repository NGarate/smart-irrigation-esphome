# Lista Completa de Componentes - Sistema de Riego Automático

## 🔧 Componentes Principales (Ya Disponibles)

### Electrónicos
| Componente | Especificaciones | Cantidad | Uso |
|------------|------------------|----------|-----|
| **ESP32-S3 DevKit** | 240MHz, WiFi, Bluetooth, 45 GPIO | 1 | Controlador principal |
| **Baterías 26650** | LII-51S, 3.7V, 5000mAh | 2 | Alimentación portátil |
| **Soporte PCB Baterías** | Para 2x 26650 | 1 | Montaje de baterías |
| **Buck Converter LM2596** | 3-40V → 1.5-35V, 3A | 1 | Regulación de voltaje |
| **INA226** | Monitor I2C, 16-bit, ±81.92V | 1 | Monitoreo consumo |
| **DHT22** | Temp: -40~80°C, Hum: 0-100% | 1 | Ambiente |
| **JSN-SR04T** | Ultrasónico, 20cm-600cm, IP67 | 1 | Nivel de agua |
| **ADS1115** | ADC 16-bit, 4 canales, I2C | 1 | Lectura sensores |
| **Sensores Capacitivos** | Humedad suelo, analógico | 4 | Una por planta |
| **Módulo Relés 4Ch** | 5V, estado sólido, optoacoplado | 1 | Control bombas |
| **Bombas Sumergibles** | 3V DC, caudal ~1L/min | 4 | Una por planta |

### Hidráulicos
| Componente | Especificaciones | Cantidad | Uso |
|------------|------------------|----------|-----|
| **Manguera** | Diámetro interior 4mm, flexible | 5m | Distribución agua |

## 🛒 Componentes Adicionales Necesarios

### Conectores y Cables
| Componente | Especificaciones | Cantidad | Precio Est. |
|------------|------------------|----------|-------------|
| **Cables Dupont H-H** | 20cm, 40 pines | 1 set | €3-5 |
| **Cables Dupont M-H** | 15cm, 40 pines | 1 set | €3-5 |
| **Cable AWG22** | Rígido, rojo/negro | 2m c/u | €2-3 |
| **Cable AWG24** | Flexible, multicolor | 5m | €3-4 |
| **Conectores JST-XH** | 2.54mm, 2-5 pines | 10 unid | €5-8 |
| **Conectores Tornillo** | Para relés, 2 pines | 4 unid | €2-3 |
| **Termoretráctil** | Varios diámetros | 1 set | €3-5 |

### Sistema Hidráulico
| Componente | Especificaciones | Cantidad | Precio Est. |
|------------|------------------|----------|-------------|
| **Conectores T 4mm** | Para derivaciones | 4 unid | €2-3 |
| **Conectores Rectos** | Empalmes 4mm | 8 unid | €3-4 |
| **Abrazaderas Mini** | Para manguera 4mm | 10 unid | €2-3 |
| **Válvulas Anti-retorno** | 4mm, prevenir reflujo | 4 unid | €8-12 |
| **Filtro Agua** | Pequeño, inline 4mm | 1 unid | €3-5 |
| **Depósito Agua** | 10-20L, con tapa | 1 unid | €15-25 |

### Caja y Montaje
| Componente | Especificaciones | Cantidad | Precio Est. |
|------------|------------------|----------|-------------|
| **Caja Estanca** | IP65, 200x150x75mm | 1 unid | €10-15 |
| **Prensaestopas** | M12, IP68 | 6 unid | €6-10 |
| **Separadores PCB** | M3, varios tamaños | 1 set | €3-5 |
| **Tornillos M3** | Varios largos | 1 set | €2-3 |
| **Bridas Plásticas** | Organización cables | 20 unid | €2-3 |
| **Espuma Protectora** | Para interior caja | 1 plancha | €3-5 |

### Herramientas Necesarias
| Herramienta | Uso | Precio Est. |
|-------------|-----|-------------|
| **Soldador 30W** | Conexiones electrónicas | €15-25 |
| **Estaño 0.8mm** | Soldadura | €3-5 |
| **Multímetro** | Verificación circuitos | €10-20 |
| **Pelacables** | Preparación cables | €5-10 |
| **Destornilladores** | Montaje | €5-10 |
| **Taladro + Brocas** | Agujeros caja | €20-40 |
| **Pistola Calor** | Termoretráctil | €15-25 |

## 🌱 Sistema de Plantas (Por Implementar)

### Por Planta (x4)
| Componente | Especificaciones | Cantidad Total | Precio Est. |
|------------|------------------|----------------|-------------|
| **Macetas** | 20-30cm diámetro | 4 | €20-40 |
| **Sustrato** | Tierra para plantas | 40L | €15-25 |
| **Plantas** | Según preferencia | 4 | €20-60 |
| **Goteros** | Para manguera 4mm | 4 | €4-8 |

## 💡 Mejoras Opcionales

### Alimentación Alternativa
| Componente | Especificaciones | Precio Est. |
|------------|------------------|-------------|
| **Panel Solar** | 10W, 12V | €25-40 |
| **Controlador Carga** | PWM/MPPT 10A | €15-30 |
| **Batería Adicional** | 26650 x2 + holder | €20-35 |

### Sensores Adicionales
| Componente | Especificaciones | Precio Est. |
|------------|------------------|-------------|
| **Cámara ESP32** | Monitoreo visual | €10-20 |
| **Sensor Luz** | BH1750, luminosidad | €3-5 |

## 🛍️ Dónde Conseguir Componentes

### Tiendas Online (España)
- **Electrónicos**: 
  - Amazon España
  - AliExpress (tiempos largos)
  - Mouser Electronics
  - Farnell España

## 💰 Resumen de Costos

### Componentes Adicionales Mínimos
| Categoría | Costo Estimado |
|-----------|----------------|
| Cables y Conectores | €20-35 |
| Sistema Hidráulico | €25-45 |
| Caja y Montaje | €25-40 |
| Herramientas Básicas | €40-70 |
| Sistema Plantas | €60-130 |
| **TOTAL ADICIONAL** | **€170-320** |

### Mejoras Opcionales
| Categoría | Costo Estimado |
|-----------|----------------|
| Panel Solar | €40-70 |
| Sensores Extra | €35-85 |
| **TOTAL MEJORAS** | **€75-155** |

## 📋 Lista de Compra Prioritaria

### Prioridad 1 (Esencial)
- [x] Cables Dupont (H-H y M-H)
- [x] Cable AWG22 rojo/negro
- [x] Conectores JST-XH
- [x] Caja estanca IP65
- [ ] Prensaestopas
- [x] Conectores T y rectos 4mm
- [ ] Abrazaderas manguera
- [x] Depósito agua

### Prioridad 2 (Importante)
- [x] Soldador y estaño
- [x] Multímetro
- [ ] Válvulas anti-retorno
- [x] Macetas y sustrato
- [x] Goteros

### Prioridad 3 (Mejoras)
- [ ] Panel solar

## ⚠️ Consideraciones Importantes

1. **Compatibilidad**: Verificar voltajes y corrientes
2. **Calidad**: Preferir componentes IP65+ para exterior
3. **Garantía**: Conservar facturas para posibles devoluciones
4. **Cantidad**: Comprar 10-20% extra de conectores/cables
5. **Timing**: Componentes de AliExpress tardan 2-4 semanas

---
**💡 Tip**: Comenzar con lo mínimo funcional y añadir mejoras gradualmente 