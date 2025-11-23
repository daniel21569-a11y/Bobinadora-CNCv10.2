# Changelog

Todas las modificaciones notables del proyecto se documentan en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [10.2] - 2025-11-23

### 🎯 Added
- Algoritmo de acumulador incremental optimizado para modo nido de abeja
- Variable cacheada `step_ratio_X_per_Y_honeycomb` para evitar divisiones flotantes
- Dirección establecida EN CADA paso del motor X (replicando arquitectura transformador)

### ⚡ Changed
- **BREAKING**: Modo honeycomb completamente reescrito con nuevo algoritmo
- Homing 3.3x más rápido (300μs vs 1000μs)
- Retroceso de homing aumentado a 5mm exactos (1600 pasos @ 320 pasos/mm)
- Header de versión actualizado a v10.2 en UI

### 🐛 Fixed
- **CRÍTICO**: Motor X errático/tembloroso en modo nido de abeja
- División flotante costosa ejecutándose en cada paso Y
- Pin DIR no establecido correctamente en pasos consecutivos
- Interferencia UI → motores durante scroll de pantalla
- Pérdida de precisión en cálculo de posición X

### 🔧 Technical
- `motor_task_optimized.h` versión 10.3
- Eliminación de delays condicionales (5μs → 2μs cuando necesario)
- Pre-cálculo de ratio en `init_honeycomb_cycle()`
- Uso de ratio cacheado en `process_honeycomb_step()`

---

## [10.1] - 2025-11-22

### Added
- Arquitectura dual-core optimizada
- Tarea de logging dedicada en Core 0
- Separación completa Core 0 (UI) / Core 1 (motores)

### Changed
- Eliminados todos los `Serial.print` de Core 1
- Optimización de loops críticos de generación de pulsos
- Rampa de aceleración/desaceleración mejorada

### Fixed
- Pérdida de pasos a alta velocidad
- Interferencias entre LVGL y generación de pulsos

---

## [10.0] - 2025-11-20

### Added
- Implementación inicial con LVGL
- Modo transformador funcional
- Modo nido de abeja básico
- Interfaz táctil completa
- Persistencia de configuraciones
- Control manual de ejes
- Homing automático

### Technical
- ESP32-S3 dual-core base
- Display 800x480 táctil capacitivo
- Drivers stepper A4988/DRV8825
- FreeRTOS task management

---

## Tipos de Cambios

- `Added` → Nuevas funcionalidades
- `Changed` → Cambios en funcionalidades existentes
- `Deprecated` → Funcionalidades marcadas como obsoletas
- `Removed` → Funcionalidades eliminadas
- `Fixed` → Corrección de bugs
- `Security` → Correcciones de seguridad
