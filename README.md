P&S Ultimate RSI MTF (Padrino & Soldado) + Divergencias Overlay
---------------------------------------------------------------------

P&S Ultimate RSI MTF es un indicador de sesgo + timing diseñado para scalping y day trading.
Une tres ideas en un solo script:

1) Ultimate RSI (motor LuxAlgo) para un oscilador más estable que el RSI clásico.
2) Multi-Timeframe (MTF) para definir tendencia/sesgo con una línea “Padrino”.
3) Divergencias tipo Lux (inspiradas en RSI Candlestick Oscillator) dibujadas SOLO en el gráfico de precio (overlay), para no ensuciar el panel del oscilador.

Idea central:
- El Padrino define la dirección (qué operar).
- El Soldado define el momento (cuándo entrar).
- La Signal Line filtra el impulso (evita entradas flojas).
- Las divergencias overlay dan contexto de agotamiento/reversión (no son gatillo por sí solas).


1) Componentes del indicador
============================

A) Soldado (Ultimate RSI)
------------------------
Es el Ultimate RSI del timeframe actual (ej: M5). Representa el momentum inmediato.
Color por régimen:
- Sobrecomprado (OB): parte alta (por defecto 80).
- Sobrevendido (OS): parte baja (por defecto 20).
- Zona media: neutral (cerca de 50).

Uso:
- Buscar timing cuando el precio llega a una zona (POI).
- No es una señal sola: necesita contexto + regla del Padrino.

B) Padrino 1 (MTF)
------------------
Es el Ultimate RSI calculado en un timeframe mayor (por defecto H1).
Funciona como:
- Sesgo direccional (filtro principal).
- “Techo/suelo dinámico” del oscilador: el Soldado reacciona contra esta guía.

Regla:
- Preferir compras cuando el Soldado trabaja por encima del Padrino.
- Preferir ventas cuando el Soldado trabaja por debajo del Padrino.

C) Padrino 2 (MTF opcional)
---------------------------
Segundo Padrino (por defecto H4) para confluencia.
Modos de confluencia:
- Ignore: no exige nada extra.
- SoldadoAboveBoth: exige Soldado por encima/debajo de P1 y P2.
- P1AboveP2: exige relación macro entre P1 y P2.

D) Signal Line (Soldado)
------------------------
Es una media móvil del Soldado (suavizado). Su propósito es filtrar impulso y reducir “whipsaws”.

Lectura práctica:
- Soldado > Signal: impulso alcista activo (mejor para longs).
- Soldado < Signal: impulso bajista activo (mejor para shorts).

Cómo se usa:
- No reemplaza al Padrino; lo complementa.
- Si el sesgo está claro, la Signal ayuda a elegir el momento “con fuerza” y evita entradas cuando el Soldado está débil o sin continuidad.

Recomendación:
- Mantener el filtro activado si tu problema es entrar en falsos arranques.
- Desactivarlo si buscas máxima sensibilidad (más señales, menos filtro).


2) Señales del sistema (KISS / CROSS)
=====================================

El indicador marca dos eventos de timing (si están habilitados):

A) KISS (Beso / Retesteo)
-------------------------
Ocurre cuando el Soldado “toca” la zona del Padrino 1 y luego rechaza con intención a favor del sesgo.

Condiciones (resumen):
- Contacto dentro de un umbral (touchThr).
- Reacción dentro de una ventana de barras (rejBars).
- Separación mínima posterior (rejMove).
- Alineado con el sesgo del Padrino.

Interpretación:
- KISS = entrada de retesteo (quirúrgica), suele dar mejor R:R si el POI es bueno.

B) CROSS (Cruce con impulso)
----------------------------
Ocurre cuando el Soldado cruza el Padrino 1 con fuerza (impulse mínimo en el cambio del Soldado).

Condiciones (resumen):
- Cruce real (crossover / crossunder).
- Impulso mínimo para evitar cruces lentos y falsos.

Interpretación:
- CROSS = entrada por momentum (breakout/continuación o cambio fuerte de control).


3) Filtros de calidad (importantes)
===================================

A) Sesgo del Padrino (Bias)
---------------------------
Dos modos:
- Relativo: Long si Soldado > Padrino / Short si Soldado < Padrino.
- Pendiente: Long si la pendiente del Padrino es positiva / Short si es negativa.

B) Padrino plano (opcional)
---------------------------
Si activas “Bloquear señales si Padrino plano”, el indicador evita señales cuando el Padrino no tiene dirección clara (pendiente muy baja).
Útil en mercado errático o rango sucio.

C) Exigir extremo (calidad)
---------------------------
Si está activo:
- BUY solo si el Soldado está en OS.
- SELL solo si el Soldado está en OB.
Esto sube la calidad, pero reduce la cantidad de trades.

D) Cooldown
-----------
Evita señales consecutivas demasiado cercanas (control anti-spam).
Útil para no sobreoperar en chop.


4) Divergencias Overlay (Lux) — SOLO en el gráfico
==================================================

Este módulo detecta divergencias con metodología inspirada en LuxAlgo, usando pivotes sobre valores “high/low” del RSI (mejor que usar solo RSI de cierre).

Tipos que puede mostrar:
- Regular Bullish
- Regular Bearish
- Hidden Bullish
- Hidden Bearish

Importante:
- Las divergencias se dibujan SOLO en el precio (force_overlay=true).
- No se dibujan dentro del panel del oscilador, para mantenerlo limpio.

Filtro de divergencias (recomendado):
- “Filtro: exigir fuera de Upper/Lower” limita divergencias a extremos (por defecto 70/30).
- Reduce señales basura y deja las divergencias más relevantes.

Cómo leerlas bien:
- Divergencia no es entrada automática.
- Funciona como “alerta de agotamiento” o “pista de reversión/pausa”, especialmente si coincide con un POI y con pérdida de estructura o rechazo claro.


5) Workflow recomendado (cómo operarlo)
=======================================

Paso 1 — Sesgo (qué operar)
---------------------------
1) Observa el Padrino 1 (H1 por defecto).
2) Define dirección:
- Solo buscar longs si el sesgo favorece longs.
- Solo buscar shorts si el sesgo favorece shorts.
3) Si el Padrino está plano/mixto: baja agresividad o espera confirmación.

Paso 2 — Zona (POI en precio)
-----------------------------
Marca una zona lógica donde el precio suele reaccionar:
- Máximos/mínimos relevantes, liquidez, zonas de consolidación, desequilibrios, mitigación, etc.

Paso 3 — Timing (ejecutar)
--------------------------
En el POI, ejecuta SOLO si aparece:
- KISS a favor del Padrino, o
- CROSS con impulso a favor del Padrino.

Paso 4 — Confirmación opcional (calidad)
----------------------------------------
- Signal Line: confirma impulso (Soldado vs Signal).
- Divergencia overlay: refuerza idea de agotamiento/reversión (sobre todo en extremos).

Paso 5 — Gestión
----------------
- Invalidación: pérdida clara del POI o cambio de estructura inmediata.
- Objetivos: liquidez previa / siguiente zona / extremos opuestos.
- No perseguir: si llegó tarde, esperar retesteo.


6) Setups (5 niveles: agresivo → conservador)
=============================================

Setup 1 — Agresivo (reversal rápido)
- Contexto: divergencia regular (bull/bear) en extremo + POI claro.
- Gatillo: primer KISS a favor del giro o vela fuerte de rechazo.
- Filtro Signal: opcional (puede estar OFF para entrar antes).

Setup 2 — Semi agresivo (KISS en POI)
- Contexto: sesgo del Padrino definido.
- POI: zona de reacción.
- Gatillo: KISS confirmado dentro de la ventana (rejBars) + separación (rejMove).
- Filtro Signal: ON (mejora la limpieza).

Setup 3 — Balanceado (CROSS con impulso)
- Contexto: sesgo del Padrino definido.
- Gatillo: CROSS con impulso >= impulse y cierre a favor.
- Entrada: cierre de vela o pequeño retesteo inmediato.
- Filtro Signal: ON.

Setup 4 — Conservador (CROSS + retesteo)
- Contexto: sesgo del Padrino definido.
- Gatillo: CROSS con impulso + esperar retesteo del nivel/zona (en precio) antes de entrar.
- Filtro Signal: ON.
- Mejor para evitar falsas rupturas.

Setup 5 — Muy conservador (confluencia P1/P2 + extremos)
- Contexto: Padrino 2 activo y alineado (stack).
- Condición: Soldado alineado con ambos (o P1>P2 según modo).
- Exigir extremo: ON (solo operar OB/OS).
- Divergencia overlay: usada como filtro extra (no como gatillo único).


7) Defaults recomendados (tal como viene el script)
===================================================

Motor (Soldado)
- Fuente: HLC3
- Length: 14
- Method: EMA

Signal Line
- Smooth: 14
- Method: EMA
- (Filtro Soldado vs Signal: ON por defecto)

Padrino 1 (MTF)
- TF: 60 (H1)
- Length: 14
- Method: EMA

Padrino 2 (opcional)
- TF: 240 (H4)
- Method: RMA
- Confluencia: Ignore (por defecto)

KISS / CROSS
- KISS threshold: 1.5
- Ventana KISS: 10
- Separación mínima: 2.0
- Impulso CROSS: 8.5
- Cooldown: 8

Divergencias overlay
- divLen: 14
- pivot: 10
- filtro 70/30: ON
- se dibujan solo en el gráfico de precio


8) Notas y buenas prácticas
===========================

- Este indicador es sesgo + timing. No reemplaza estructura ni zonas.
- En rango: extremos + divergencia + KISS suelen funcionar mejor.
- En tendencia: sesgo del Padrino + CROSS con impulso suele ser superior; evita contra-tendencia.
- Si el mercado está errático: activa “bloquear si Padrino plano” y sube cooldown.
- Si no hay gatillo (KISS/CROSS) en POI: no hay trade.

Licencia / créditos:
- Motor Ultimate RSI derivado de “Ultimate RSI [LuxAlgo]” (CC BY-NC-SA 4.0).
- Divergencias overlay basadas en “RSI Candlestick Oscillator [LuxAlgo]” (CC BY-NC-SA 4.0).

Descargo:
Este indicador es una herramienta de análisis técnico. No es asesoría financiera.
