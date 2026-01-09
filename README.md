# P&S Ultimate RSI MTF (Padrino & Soldado) + Divergencias Overlay (Lux)

**P&S Ultimate RSI MTF** es un indicador de lectura de momentum y contexto multi-timeframe basado en el motor “Ultimate RSI” y complementado con un módulo de divergencias que se dibuja únicamente sobre el gráfico de precio (overlay).

<img width="1567" height="878" alt="image" src="https://github.com/user-attachments/assets/e84b0428-51f6-43f8-912e-c928db9fcc47" />

> **Nota:** Este indicador no es una estrategia. No define entradas ni salidas. Solo organiza información de momentum, sesgo relativo y señales de divergencia para apoyar el análisis.

## Objetivo
El objetivo del indicador es entregar en un solo panel:
1.  **Oscilador principal (Soldado):** Basado en Ultimate RSI para medir el impulso del timeframe actual.
2.  **Referencias MTF (Padrino 1 y Padrino 2):** Para aportar contexto direccional y comparación relativa.
3.  **Signal Line:** Para suavizar el impulso y filtrar ruido.
4.  **Divergencias (Overlay):** Tipo Lux (inspiradas en RSI Candlestick Oscillator) dibujadas solo en el precio para mantener el panel limpio.

---

## Qué muestra el indicador

### A) Soldado (Ultimate RSI)
El **Soldado** es el valor del Ultimate RSI calculado en el timeframe del gráfico (por ejemplo, M5).
* **Qué representa:** Momentum inmediato del mercado (impulso y desaceleración) y régimen de fuerza relativa.
* **Lectura por zonas (Default):**
    * *Sobrecomprado (OB):* Zona alta (80).
    * *Sobrevendido (OS):* Zona baja (20).
    * *Zona media:* Región neutral alrededor de 50.
* **Coloración:** Si está activado el color automático, el Soldado cambia de color según la zona en la que se encuentre.

### B) Padrino 1 (MTF)
**Padrino 1** es el mismo Ultimate RSI, pero calculado en un timeframe mayor (por defecto H1) y proyectado sobre el panel.
* **Qué aporta:** Contexto de timeframe mayor (sesgo o guía) y referencia dinámica.
* **Lectura:**
    * *Soldado > Padrino 1:* Momentum actual relativamente fuerte vs el marco mayor.
    * *Soldado < Padrino 1:* Momentum actual relativamente débil vs el marco mayor.

### C) Padrino 2 (MTF Opcional)
**Padrino 2** es un segundo Ultimate RSI en timeframe aún mayor (por defecto H4).
* **Qué aporta:** Una capa extra de "Macro-contexto".
* **Confluencia P1/P2:**
    * *Ignore:* Dibuja sin filtros adicionales.
    * *SoldadoAboveBoth:* Evalúa relación del Soldado respecto a P1 y P2.
    * *P1AboveP2:* Evalúa alineación macro entre P1 y P2.

### D) Signal Line (Soldado)
Media móvil del Soldado para suavizar el oscilador.
* **Qué aporta:** Reduce el ruido y permite comparar el impulso actual contra su promedio.
* **Lectura:**
    * *Soldado > Signal:* Aceleración relativa.
    * *Soldado < Signal:* Debilidad relativa.

### E) KISS / CROSS (Marcadores visuales)
Eventos de interacción entre el Soldado y Padrino 1.
* **KISS:** Marca un evento de "contacto + reacción" del Soldado alrededor de Padrino 1.
* **CROSS:** Marca un cruce del Soldado sobre/bajo Padrino 1 con impulso mínimo.
* *Nota:* En modo "Debug" se muestran letras ("K" o "C"); en modo "Limpio" son símbolos geométricos.

### F) Divergencias Overlay (Lux)
Módulo inspirado en LuxAlgo que detecta divergencias usando pivotes sobre valores derivados del RSI.
* **Overlay:** Se dibujan **únicamente sobre el precio** para mantener el panel limpio.
* **Tipos:** Regular Bullish/Bearish y Hidden Bullish/Bearish.
* **Filtro (Opcional):** "Exigir fuera de Upper/Lower" restringe las divergencias a zonas extremas (ej. 70/30) para evitar señales irrelevantes en zona media.

---

## Parámetros principales

### A) Ultimate RSI (Motor)
* **Fuente (src):** Precio base (Recomendado: HLC3).
* **Length:** Periodo de cálculo (Típico: 14).
* **Method:** Suavizado interno (EMA, SMA, RMA, TMA).

### B) Niveles (OB / OS / Mid)
* **Umbrales:** Define las zonas de régimen (Típico: 80/20).

### C) Sesgo & Filtros
* **Sesgo del Padrino (biasMode):**
    * *Relativo:* Compara posición (Soldado vs Padrino).
    * *Pendiente:* Evalúa la dirección del Padrino.
* **Padrino plano (flatThr / blockFlat):** Detecta y bloquea señales si el Padrino no tiene dirección clara.
* **Filtro Soldado vs Signal:** Requiere alineación del Soldado con su Signal Line para validar eventos.
* **Exigir extremo (requireExtreme):** Limita los eventos KISS/CROSS a zonas de OB/OS.

### D) KISS / CROSS
* **KISS:** Configura umbral de toque (`touchThr`), ventana de reacción (`rejBars`) y separación mínima (`rejMove`).
* **CROSS:** Configura el impulso mínimo (`impulse`) requerido para el cruce.
* **Cooldown:** Evita el "spam" visual de señales consecutivas.

### E) Divergencias Overlay
* **RSI Length / Pivots:** Ajuste de sensibilidad del cálculo de divergencias.
* **Upper/Lower:** Niveles para el filtro de extremos.
* **Estilos:** Configuración visual de líneas y etiquetas.

---

## Interpretación General

| Componente | Qué información entrega |
| :--- | :--- |
| **Soldado** | Momentum del timeframe actual. |
| **Padrinos (MTF)** | Contexto y dirección de marcos mayores. |
| **Signal Line** | Impulso suavizado y continuidad. |
| **KISS / CROSS** | Interacciones relevantes de momentum (Timing). |
| **Divergencias** | Señales de agotamiento, pausa o desacople en el precio. |

---

## Notas y buenas prácticas

El indicador está diseñado para la claridad visual.

**Para reducir ruido:**
1. Mantener activo el filtro **Soldado vs Signal**.
2. Usar **"Exigir extremo"** para reducir marcaciones.
3. Aumentar `cooldownBars`.
4. Activar bloqueo por **Padrino plano**.

**Para mayor sensibilidad:**
1. Desactivar "Exigir extremo".
2. Reducir el impulso mínimo de **CROSS**.
3. Desactivar la restricción de divergencias (`divRestrict OFF`).

---

## Créditos / Licencia
* Motor Ultimate RSI derivado de “Ultimate RSI [LuxAlgo]” (CC BY-NC-SA 4.0).
* Divergencias overlay inspiradas en “RSI Candlestick Oscillator [LuxAlgo]” (CC BY-NC-SA 4.0).

## Descargo
Este indicador es una herramienta de análisis técnico. **No constituye asesoría financiera.**
