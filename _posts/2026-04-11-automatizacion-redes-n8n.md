---
title: "De 3 semanas a 2 horas: Automatizando O&M con Python y n8n"
date: 2026-04-11 10:00:00 +0200
categories: [Proyectos, DevSecOps]
tags: [python, automatización, n8n, infraestructura]
image:
  path: https://images.unsplash.com/photo-1550751827-4bd374c3f58b?q=80&w=1000
  alt: Centro de datos y servidores
---

## El Desafío Operativo (O&M)

En infraestructuras críticas, la Operación y Mantenimiento (O&M) suele ser el cuello de botella. Recientemente, me enfrenté a un reto corporativo: **las auditorías de red y la extracción masiva de copias de seguridad (backups) tomaban hasta 3 semanas de trabajo manual.**

El error humano, los *timeouts* y la gestión de credenciales hacían que el proceso fuera ineficiente e inescalable. La solución no era contratar más personal, sino inyectar ingeniería de automatización.

## La Solución Arquitectónica

Diseñé un flujo híbrido combinando la potencia de *scripting* de **Python** con la orquestación visual y gestión de alertas de **n8n**. 

### 1. El Core en Python (Tolerancia a fallos)
El mayor problema de los scripts de red tradicionales es que se detienen si un equipo falla. Para evitar esto, estructuré el código con un manejo de excepciones granular (`try-except`). Si un router tiene las credenciales rotadas o no responde, el script aísla el error, notifica y salta al siguiente nodo sin romper el bucle.

```python
    try:
        # Intento de conexión y backup (Netmiko/SecureCRT)
        logging.info(f"[INICIO O&M] Conectando a {device_ip}...")
        ejecutar_backup(device)

    except (TimeoutException, AuthException) as e:
        logging.error(f"[ERROR CRÍTICO] Fallo en {device_ip}: {e}")
        trigger_n8n_alert(device_ip, "Timeout SSH o Auth Failed") # Llamada al Webhook
