---
title: "El eslabón débil del HTTPS: secuestro de dominios nacionales"
date: 2026-10-08 10:59:31 +0200
categories: ["Ciberseguridad", "Infraestructura"]
tags: ["dns", "https", "ciberseguridad", "dominios", "web"]
image:
  path: /assets/img/posts/2026-10-08-el-eslabon-debil-del-https-secuestro-de-dominios-nacionales.jpg
  alt: "El eslabón débil del HTTPS: secuestro de dominios nacionales"
---

Esta semana tocó recordar que la seguridad de medio internet depende de hilos bastante más finos de lo que nos gusta pensar. Nadie ha hackeado a Google ni ha encontrado un bug en su código — lo que han hackeado es la infraestructura de los registros de tres países: Ghana (.gh), Sierra Leona (.sl) y Samoa Americana (.as).

El truco es feo y elegante a la vez. Si controlas el DNS autoritativo de un dominio, controlas lo que ese dominio "demuestra" ser. Y resulta que así es exactamente como se valida un certificado HTTPS: el sistema te pide que pongas un registro TXT concreto o que respondas en una ruta específica, y si lo haces bien, la autoridad certificadora asume que eres el dueño legítimo. Los atacantes, con el DNS en su poder, pudieron decir "sí, este dominio es mío, aquí la prueba" — y consiguieron doce certificados válidos (once de Let's Encrypt, uno de ZeroSSL) para dominios que no eran suyos.

Para cualquier navegador, esa conexión era legítima y segura al cien por cien. Ese es justo el punto flaco: el sistema de certificados entero da por hecho que quien controla el DNS controla el dominio, y es una asunción razonable el 99,9% del tiempo — hasta que el registro de un TLD entero queda comprometido y deja de serlo.

Por suerte Google tiene sus propios sistemas de Certificate Transparency vigilando esto como un halcón, y en cuanto detectaron los certificados raros los bloquearon directamente en Chrome vía CRLSet antes de que nadie pudiera usarlos para hacer un hombre-en-el-medio de verdad, y avisaron a las CAs para revocarlos globalmente. Rápido y limpio, como debe ser.

Lo que me deja esto es una cosa clara: HTTPS no es el problema, la confianza delegada sí lo es. Si el registrador de un TLD no tiene MFA, auditorías y controles de acceso decentes, todo lo que cuelgue de él — y ahí entran millones de sitios — queda a merced de lo débil que sea ese eslabón. Si quieres dormir un poco más tranquilo con tus propios dominios, mira si tu CA soporta monitorización de Certificate Transparency (crt.sh es gratis y va bien) para que te avisen si alguien emite un certificado a tu nombre sin que tú lo hayas pedido. Y activa DNSSEC ya que estás — no arregla esto, pero es una capa de integridad que no cuesta nada tener puesta.

---
*Fuente: [The Hacker News](https://thehackernews.com/2026/10/attackers-hijack-gh-sl-and-as.html)*
