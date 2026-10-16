# 🏎️ Proyecto Capstone: RaceYourCode - Bot Autónomo TORCS

¡Bienvenido al repositorio del controlador autónomo para la simulación de carreras **TORCS** (The Open Racing Car Simulator)! Este proyecto implementa un agente de conducción en Python diseñado para navegar circuitos de alta complejidad técnica con máxima estabilidad, control de adherencia y **0% de daños**.

---

## 🏆 Resultados Oficiales (Pista Corkscrew)

En las pruebas de evaluación sobre el exigente circuito de **Corkscrew**, el vehículo completó el recorrido entero registrando telemetría impecable:

| Métrica | Resultado Oficial |
| :--- | :--- |
| **Estado del Vehículo (Damages)** | **0 (Cero Daños - 100% Intacto)** |
| **Tiempo de Vuelta (Lap Time)** | **03:06.36** |
| **Velocidad Máxima (Top Speed)** | **99 km/h** |
| **Velocidad Mínima en Curvas (Min Speed)** | **22 km/h** |

> **Nota:** Se logró el cumplimiento estricto del requerimiento crítico del proyecto Capstone: completar la vuelta de circuito sin sufrir ningún tipo de colisión o daño estructural.

---

## 🛠️ Arquitectura y Estrategia de Control (`torcs_jm_par.py`)

El algoritmo de control se basa en una arquitectura modular de telemetría en tiempo real que procesa los sensores del entorno para tomar decisiones instantáneas sobre aceleración, frenado, dirección y cambios de marcha.

### 1. Sistema de Dirección Dinámica (`calculate_steering`)
* **Vector de Alineación:** Combina el ángulo del vehículo respecto al eje de la pista (`S['angle']`) y la posición lateral (`S['trackPos']`).
* **Amortiguación según Velocidad:** Reduce progresivamente el rango de viraje a altas velocidades para prevenir transferencias bruscas de peso y pérdida de tracción en curvas rápidas.

### 2. Gestión Adaptativa de Aceleración y Frenado (`calculate_throttle` & `apply_brakes`)
* **Detección Anticipada de Curvas:** Modula la velocidad objetivo (hasta 85 km/h en rectas largas y bajadas a 50 km/h al detectar ángulos cerrados).
* **Corte de Seguridad (Safety Cut):** Suprime el acelerador si el ángulo del carro supera límites críticos para evitar deslices de sobreviraje.
* **Frenado Proporcional Preventivo:** Aplica fuerza de frenado gradual antes de ingresar a sectores de alta exigencia como la curva del "Saca-corchos".

### 3. Sistema de Marchas y Rutina de Rescate (`shift_gears`)
* **Relación de Transmisión Óptima:** Selecciona progresivamente marchas de 1ª a 6ª según los rangos de velocidad.
* **Blindaje de Línea de Meta:** Previene falsas lecturas de atasco durante el arranque inicial.
* **Detección y Rescate de Atascos:** Sistema de contador dinámico que activa reversa automática y reincorporación limpia si el vehículo pierde inercia tras un evento inesperado.

---

## 💻 Entorno de Desarrollo y Herramientas

* **Lenguaje:** Python 3.x
* **Simulador:** TORCS (The Open Racing Car Simulator) con cliente UDP.
* **Entorno de Trabajo:** Visual Studio Code
* **Asistencia Local de IA:** Ollama con el modelo IBM Granite (`granite:4.350m`) integrado mediante la extensión Continue.

---

## 🚀 Instrucciones de Ejecución

1. Abrir el simulador **TORCS** y seleccionar la pista **Corkscrew** en modo de práctica.
2. Iniciar la carrera en el simulador (esperar la pantalla de grilla).
3. Ejecutar el controlador en la terminal de Python:
   ```bash
   python torcs_jm_par.py