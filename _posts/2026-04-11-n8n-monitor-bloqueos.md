---
title: "Evitando la Fatiga de Alertas: Monitor de Bloqueos Dinámicos con n8n"
date: 2026-04-12 11:00:00 +0200
categories: [Proyectos, Automatización]
tags: [n8n, javascript, telegram, osint]
image:
  path: https://images.unsplash.com/photo-1551808525-51a94da548ce?q=80&w=1000
  alt: Código en pantalla monitorizando redes
---

## El Problema del Polling y el Spam de Alertas

Cuando monitorizamos infraestructuras, listas negras de IPs o bloqueos dinámicos a nivel de ISP, el enfoque novato es simple: programar un script cada 10 minutos y enviar un mensaje si hay un bloqueo. 

¿El problema? Si el bloqueo dura 5 horas, tu equipo de respuesta a incidentes (o tú mismo) recibirá 30 alertas repetidas. Esto genera **fatiga de alertas**; los operadores terminan silenciando el canal y se pierden los incidentes críticos reales.

Para solucionar esto en mi último proyecto de monitorización, diseñé un flujo en **n8n** con una lógica de estado persistente.

## La Arquitectura de la Solución

El objetivo es encuestar un archivo JSON alojado en GitHub que reporta el estado de bloqueos en tiempo real, pero **solo alertar a través de Telegram cuando hay un cambio de estado** (transición de `true` a `false` o viceversa).

### Manejo de Estado sin Base de Datos

En lugar de desplegar un contenedor con Redis o MySQL solo para guardar un simple valor booleano, utilicé una característica avanzada de n8n: el entorno `$getWorkflowStaticData('global')`.

Este método permite guardar variables en la memoria de la instancia de n8n que persisten entre diferentes ejecuciones del flujo.

```javascript
// Acceso a la memoria estática de n8n
const staticData = $getWorkflowStaticData('global');
const newStatus = items[0]?.json?.isBlocked;
const lastStatus = staticData.lastStatus;

// Lógica de detección de transición
if (newStatus !== lastStatus) {
  // Guardamos el nuevo estado para futuras ejecuciones
  staticData.lastStatus = newStatus;
  
  return [{
    json: {
      hasChanged: true,
      isBlocked: newStatus,
      message: newStatus ? "🚨 ALERTA: Bloqueo ACTIVADO" : "✅ AVISO: Bloqueo DESACTIVADO"
    }
  }];
} else {
  // Silencio de radio: no hay cambio
  return [{ json: { hasChanged: false, isBlocked: newStatus } }];
}
