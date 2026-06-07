---
layout: post
title: "Automatizando el Huerto: Riego Inteligente con ESP32 (NASA Edition)"
date: 2026-06-07 20:00:00 +0200
categories: [Maker, IoT, ESP32, Domótica]
tags: [arduino, automatizacion, hardware, huerto, sensores]
image: "https://github.com/d4nysj/Esp32_Riego_Automatico/blob/main/Images/1.png?raw=true"
---

Mantener un huerto en perfectas condiciones exige constancia, sobre todo cuando llega el verano y los riegos no perdonan un solo descuido. Después de varios prototipos, he consolidado la versión definitiva de mi sistema para mantener todo verde y optimizado: el **Riego Automático ESP32 v3.0 "NASA Edition"**.

Quería huir de los típicos temporizadores de grifo comerciales que son cerrados y limitados. La idea era diseñar un hardware y firmware a medida que no solo regara a sus horas, sino que tomara decisiones basándose en la humedad real de la tierra y me permitiera auditar todo desde el teléfono.

![Dashboard Web del ESP32](https://github.com/d4nysj/Esp32_Riego_Automatico/blob/main/Images/1.png?raw=true)

### ¿Qué hace a esta versión tan especial?

He programado el firmware en C++ estructurando el código de forma robusta para evitar sorpresas. Estas son las características clave que le he implementado:

* **Sincronización NTP y Zona Horaria Autónoma**: Se conecta a `pool.ntp.org` y aplica automáticamente el horario de Madrid (CET/CEST). Olvídate de ajustar la hora manualmente en los cambios de estación.
* **Decisión basada en Datos**: Mediante un sensor capacitivo analógico conectado al `GPIO34`, el ESP32 evalúa si la tierra ya está lo suficientemente húmeda. Si es así, cancela inteligentemente el riego programado para ahorrar agua.
* **Persistencia NVS Flash**: Usando la librería `Preferences`, el ESP32 guarda toda la configuración, así como el conteo total de riegos y los litros estimados que ha bombeado. Si hay un corte de luz, el sistema se levanta y retoma su estado anterior sin perder un solo dato.
* **Seguridad Anti-inundaciones**: Implementé un doble *Watchdog*. Si por algún bug o desconexión el relé (conectado al `GPIO26`) se queda encendido más de los segundos estipulados de seguridad, el sistema corta la corriente forzosamente.

### Interfaz Minimalista Integrada

Todo el panel de control está embebido directamente en la memoria del microcontrolador. Sin depender de nubes externas ni suscripciones. Usando HTML, CSS puro y un poco de JS asíncrono, he diseñado una web en formato "Dark Mode" estilo terminal de comandos.

![Pestaña de Diagnóstico y Logs](https://github.com/d4nysj/Esp32_Riego_Automatico/blob/main/Images/2.png?raw=true)

Desde esta interfaz se pueden configurar las horas de activación, forzar un riego manual, consultar los **logs del sistema** (guardados en un buffer circular en RAM) y ver las métricas de hardware como la temperatura de la CPU, uso de RAM y calidad de la señal WiFi.

![Historial Completo](https://github.com/d4nysj/Esp32_Riego_Automatico/blob/main/Images/3.png?raw=true)

Si te interesa montar tu propia placa o echarle un vistazo al código fuente para adaptarlo a tus necesidades, lo he publicado todo abierto en mi repositorio. 

Puedes encontrar el proyecto completo en **[GitHub: Esp32_Riego_Automatico](https://github.com/d4nysj/Esp32_Riego_Automatico)**.

¡Nos vemos en el próximo proyecto de hardware!
