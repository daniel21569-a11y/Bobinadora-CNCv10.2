# Bobinadora CNC v10.2

<div align="center">

![Version](https://img.shields.io/badge/version-10.2-blue.svg)
![Platform](https://img.shields.io/badge/platform-ESP32--S3-green.svg)
![License](https://img.shields.io/badge/license-MIT-orange.svg)

**Sistema de bobinado automático CNC con ESP32-S3 y pantalla táctil**

[Características](#-características) •
[Instalación](#-instalación) •
[Uso](#-uso) •
[Arquitectura](#️-arquitectura)

</div>

---

## 📋 Descripción

Bobinadora CNC v10.2 es un sistema completo de control para bobinado automático de transformadores y nido de abeja, implementado en ESP32-S3 con interfaz gráfica LVGL sobre pantalla táctil capacitiva.

### ✨ Características Principales

**Modos de Bobinado:**
- 🔄 **Transformador**: Bobinado tradicional capa por capa con control preciso
- 🍯 **Nido de Abeja**: Patrón entrecruzado con desfase angular configurable

**Hardware:**
- 🖥️ ESP32-S3 (Dual-Core @240MHz)
- 📱 Pantalla táctil capacitiva 4.8" (800x480)
- 🎯 Control de 2 motores stepper (X: carrete, Y: mandril)
- 🏠 Sensores de límite (endstops)
- ⚡ Drivers A4988/DRV8825/TMC2208 compatibles

**Funcionalidades:**
- ✅ Control de velocidad variable (RPM)
- ✅ Homing automático optimizado (3.3x más rápido)
- ✅ Detención automática en capas
- ✅ Control manual de ejes
- ✅ Persistencia de configuraciones (EEPROM)
- ✅ Interfaz táctil moderna con teclado numérico
- ✅ Cálculos automáticos de pasos

---

## 🚀 Instalación

### Requisitos

**Software:**
- Arduino IDE 2.x o superior
- ESP32 Board Support Package (v2.0.14+)
- Librerías: `lvgl` (v9.x), `Arduino_GFX`, `TAMC_GT911`

**Hardware:**
- Placa ESP32-S3 con >= 8MB PSRAM
- Display compatible (JC4827W543 o similar)
- 2x Drivers de motor stepper
- 2x Motores stepper NEMA17/23
- Fuente de alimentación (12-24V para motores)

### Configuración

1. **Ajustar hardware** (si es necesario):
   - Editar `config.h` para tus pines específicos
   - Ajustar `PASOS_POR_MM_X` y `PASOS_POR_VUELTA_Y`

2. **Compilar**:
   - Tools → Board → ESP32S3 Dev Module
   - Tools → PSRAM → "OPI PSRAM"
   - Sketch → Upload

---

## 🎮 Uso

### Inicio Rápido

1. Al encender, se ejecuta homing automático en eje X
2. Pantalla principal → **CONFIGURAR** → Seleccionar modo
3. Ajustar parámetros del bobinado
4. **Bobinar →** → **▶ Iniciar**

### Parámetros

**Transformador:**
- Diámetro hilo, Ancho carrete, Vueltas totales, Velocidad (RPM)

**Nido de Abeja:**
- Diámetro hilo, Diámetro/Ancho carrete, Desfase (°), Vueltas, Velocidad

---

## 🏗️ Arquitectura

### Dual-Core Optimizado

```
┌──────────────────────────────┐
│     ESP32-S3 Dual-Core       │
├──────────────┬───────────────┤
│  CORE 0      │  CORE 1       │
│  (UI/Logic)  │  (Motors)     │
├──────────────┼───────────────┤
│ • LVGL UI    │ • STEP/DIR    │
│ • Touch      │ • Timing      │
│ • Serial     │ • Priority    │
└──────────────┴───────────────┘
```

**Core 1** dedicado EXCLUSIVAMENTE a timing crítico de motores.

### Algoritmo Honeycomb

```cpp
// Acumulador incremental:
acumulador += ratio_pre_calculado;
while (acumulador >= 1.0) {
    set_dir_x(dirección);  // ✅ En cada paso
    generar_pulso_X();
    acumulador -= 1.0;
}
```

---

## 📊 v10.2 - Notas de Versión

**🎯 Mejoras:**

1. **Honeycomb Optimizado**
   - Dirección en CADA paso (sin temblores)
   - Ratio pre-calculado (sin divisiones)
   - Movimientos suaves perfectos

2. **Homing Mejorado**
   - 3.3x más rápido (300μs)
   - Retroceso exacto 5mm

**🐛 Resuelto:**
- Motor X errático en nido de abeja
- División flotante en loop crítico

---

## 🛠️ Calibración

En `config.h`:

```cpp
constexpr float PASOS_POR_MM_X = 320.0f;   // Ajustar según setup
constexpr int PASOS_POR_VUELTA_Y = 25600;  // Microstepping
constexpr float RPM_ACCELERATION = 100.0f; // Rampa
```

---

## 📝 Licencia

MIT License - Uso libre con atribución.

---

## 🙏 Créditos

- LVGL, Arduino_GFX, TAMC_GT911, ESP32 Community

---

<div align="center">

**Hecho con ❤️ para la comunidad maker**

</div>
