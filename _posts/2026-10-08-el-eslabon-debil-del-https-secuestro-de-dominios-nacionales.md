---
title: "El eslabón débil del HTTPS: secuestro de dominios nacionales"
date: 2026-10-08 10:59:31 +0200
categories: ["Ciberseguridad", "Infraestructura"]
tags: ["dns", "https", "ciberseguridad", "dominios", "web"]
image:
  path: /assets/img/posts/2026-10-08-el-eslabon-debil-del-https-secuestro-de-dominios-nacionales.jpg
  alt: "El eslabón débil del HTTPS: secuestro de dominios nacionales"
---

Lo de esta semana es un recordatorio de que, a veces, la seguridad de Internet depende de hilos más finos de lo que nos gusta pensar. No han hackeado a Google, ni han encontrado un bug en su código. Han atacado la **infraestructura de los registros de dominio** de tres países: Ghana (.gh), Sierra Leona (.sl) y Samoa Americana (.as).

Básicamente, los atacantes tomaron el control de los registros y modificaron los servidores DNS autoritativos. Si controlas el DNS, controlas el tráfico que llega a esos dominios. Con ese acceso, hicieron algo muy listo y peligroso a la vez: **engañaron a las autoridades de certificación (CA)**.

## ¿Cómo funciona el truco?

Cuando pides un certificado HTTPS (como los de Let's Encrypt o ZeroSSL), el sistema necesita verificar que realmente eres el dueño del dominio. La validación estándar es automática: el sistema te pide que coloques un registro TXT específico o que respondas a una petición HTTP en una ruta concreta.

Si los atacantes controlan el DNS, pueden decir: "Sí, ese dominio es mío, aquí tienes la prueba". Resultado: la CA emite un certificado oficial para `google.com` (o subdominios críticos) porque técnicamente "demostraste" el control. 

Así consiguieron **12 certificados válidos** (11 de Let's Encrypt y 1 de ZeroSSL). Para el navegador, la conexión era 100% legítima y "segura".

## El problema no es la encriptación, es la confianza

El sistema de certificados actual asume que si controlas el DNS, controlas el dominio. Es una simplificación necesaria para que Internet funcione a escala, pero aquí es donde está el punto ciego. 

Google se dio cuenta rápido —tienen sistemas de monitoreo de transparencia de certificados que son una bestia— y activó el protocolo de emergencia:

1. **CRLSet**: Bloquearon los certificados directamente en Chrome antes de que nadie pudiera usarlos para un ataque de *man-in-the-middle*.
2. **Coordinación**: Avisaron a las CAs para revocar esos certificados globalmente.

Si quieres revisar qué certificados se están emitiendo para tus dominios, **Certificate Transparency (CT)** es tu mejor amigo. Aquí un ejemplo rápido de cómo consultar registros en un log de CT usando `openssl` o herramientas tipo `crt.sh`:

```bash
# Ejemplo conceptual para buscar certificados emitidos recientemente
curl -s "https://crt.sh/?q=tu-dominio.com&output=json" | jq .
```

## ¿Qué sacamos en claro?

Esto no significa que debas dejar de confiar en HTTPS. Lo que significa es que **la seguridad en Internet es un ecosistema**. Si el registrador de un TLD (.gh, .sl, .as) no tiene sus sistemas bien blindados (MFA, auditorías, controles de acceso), todo lo que dependa de ellos queda expuesto.

**Mi consejo práctico:**
* **Monitorea tus dominios:** Usa herramientas como [crt.sh](https://crt.sh/) o servicios de *Certificate Transparency monitoring* para recibir alertas cuando alguien emita un certificado a tu nombre.
* **DNSSEC:** No soluciona el problema de las CAs, pero es una capa más de integridad que deberías tener activada sí o sí.
* **No te fíes solo del candado:** HTTPS asegura el transporte, pero no garantiza que la web que visitas sea la que crees, especialmente si el DNS está comprometido.

La noticia original viene de [The Hacker News](https://thehackernews.com/2026/10/attackers-hijack-gh-sl-and-as.html), por si quieres profundizar en los detalles técnicos de cómo movieron los registros.

---
*Fuente: [The Hacker News](https://thehackernews.com/2026/10/attackers-hijack-gh-sl-and-as.html)*
