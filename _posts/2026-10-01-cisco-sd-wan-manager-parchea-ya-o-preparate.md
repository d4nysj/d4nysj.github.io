---
title: "Cisco SD-WAN Manager: Parchea ya o prepárate"
date: 2026-10-01 14:50:05 +0200
categories: ["Ciberseguridad", "Redes"]
tags: ["cisco", "sdwan", "vulnerabilidad", "cisa", "seguridad"]
image:
  path: https://live.staticflickr.com/233/450303689_9970b01798_b.jpg
  alt: "Cisco SD-WAN Manager: Parchea ya o prepárate"
---

Si trabajas con redes y hoy se te ha cruzado Cisco en el radar, deja el café un segundo. La CISA acaba de meter en su catálogo de vulnerabilidades explotadas activamente (KEV) un fallo del Catalyst SD-WAN Manager que da bastante miedo: CVE-2026-76504, un bypass de autenticación crítico.

Lo gordo del asunto es que no hace falta usuario ni contraseña. Un atacante remoto, manipulando cómo el gestor interpreta una codificación hexadecimal en las peticiones que recibe, consigue colarse directamente con privilegios de administrador. El sistema no está validando bien lo que le llega, y con eso basta.

Y esto no es un fallo menor de los que puedes dejar aparcados hasta la próxima ventana de mantenimiento. Quien controla el SD-WAN Manager controla el cerebro de toda la red: puede interceptar tráfico, redirigir el enrutamiento a donde le dé la gana, o plantar puertas traseras en los dispositivos edge sin que nadie se entere hasta que sea demasiado tarde. Y la CISA no lo ha metido en el KEV por precaución — está en esa lista porque ya se está explotando de verdad, no es una prueba de concepto de laboratorio.

Si gestionas uno de estos, el plan es el de siempre pero con más urgencia que de costumbre: entra al panel y comprueba si tu versión está afectada, y si hay parche, instálalo hoy, no la semana que viene. Si por lo que sea no puedes parchear ahora mismo, al menos saca la interfaz de gestión de internet — que solo sea accesible desde tu VPN o tu red de administración, nunca desde cualquier IP del mundo.

La lección de fondo es la de siempre y seguimos sin aprenderla: no expongas interfaces de gestión críticas a internet. Es cómodo, lo sé, pero es dejar la puerta de casa abierta confiando en que nadie pase por ahí. Parchea, cierra el acceso externo, y esta noche duermes mejor.

---
*Fuente: [CISA - Known Exploited Vulnerabilities](https://www.cisa.gov)*
