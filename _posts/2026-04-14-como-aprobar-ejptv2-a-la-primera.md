---
title: "Cómo Aprobar el eJPTv2 a la Primera: La Guía Definitiva y Sin Rodeos"
date: 2026-04-14 00:00:00 +0200
categories: [Certificaciones, Ciberseguridad]
tags: [ejptv2, pentesting, elearnsecurity, hacking, redteam]
image:
  path: https://media.licdn.com/dms/image/v2/D4E22AQGcZ5rbwhKLfg/feedshare-shrink_800/B4EZvRlpxIKEAg-/0/1768747867496?e=1777507200&v=beta&t=aF1WrjtjO_V6yqXaZtmVSD21x9lJ8qTq6tqCzNvOJGI
  alt: ejptv2Danielsanjuan
---

Vamos al grano. Quieres sacarte la **eJPTv2** (eLearnSecurity Junior Penetration Tester) y necesitas saber exactamente a qué te enfrentas y cómo prepararte sin perder el tiempo. Esta certificación es la puerta de entrada perfecta al mundo del *pentesting* porque es 100% práctica. Nada de memorizar teoría inútil; aquí te dan una máquina atacante Kali Linux en tu navegador (sin conexión a internet público) y te sueltan en una red empresarial para que la audites.

Aquí tienes la hoja de ruta directa, sincera y "a tiro hecho" para que vayas a por el aprobado.

---

## 🕒 La Faena: ¿A qué te enfrentas?

* **Formato:** Es un entorno de laboratorio con unas 5-6 máquinas en una zona desmilitarizada (DMZ) y una red interna oculta a la que tendrás que acceder haciendo *pivoting*.
* **Tiempo y Preguntas:** Tienes 48 horas para responder 35 preguntas tipo test. ¡Ojo! El tiempo sobra muchísimo, no te agobies.
* **Para aprobar:** Necesitas un 70% de aciertos en total, pero cuidado: el examen se divide en 4 dominios y debes sacar un mínimo del 70% en **CADA UNO** de ellos. Si sacas un 100% en tres y un 60% en otro, suspendes.

---

## 🛠️ Las Herramientas CORE (Lo que necesitas dominar sí o sí)

No necesitas saber de todo, necesitas dominar lo básico a la perfección:

* **Nmap:** Tu radar. Tienes que saber escanear puertos, servicios y versiones de manera ágil.
* **Metasploit (`msfconsole`):** Es tu mejor amigo y es obligatorio. Úsalo para buscar *exploits* y ganar *shells* de Meterpreter.
* **Pivoting con Metasploit:** La parte "difícil". Tienes que saber usar los módulos `autoroute` y `portfwd` (o `portproxy` en Windows) para enrutar tu tráfico hacia la red interna oculta.
* **Fuerza Bruta (Hydra y CrackMapExec):** El 60% del examen lo vas a resolver haciendo fuerza bruta. Acostúmbrate a usar Hydra para logins web, SSH y FTP.
* **Reconocimiento Web y de Directorios:** Fuzzea siempre con `gobuster` o `dirb`. Si ves un CMS (WordPress o Drupal), saca `wpscan` para enumerar usuarios y *plugins*.

---

## 💻 Las Mejores Máquinas para Practicar (Máximo Valor)

No te vayas a hacer máquinas *Insane*. Este es un examen *Junior* y de vulnerabilidades clásicas. Aquí tienes tu laboratorio de entrenamiento:

### En DockerLabs (Gratis y al grano)
Haz únicamente las máquinas de nivel "Muy Fácil" y "Fácil":
* **Trust:** Ideal para aprender a usar Hydra y hacer *fuzzing* de directorios.
* **WalkingCMS:** Máquina vital. Hecha a medida para el examen, pura enumeración y fuerza bruta sobre WordPress.
* **Domain:** Perfecta para entender cómo enumerar recursos compartidos en SMB (Samba).

### En HackTheBox
*(Nota: Algunas requieren suscripción VIP porque están retiradas, pero son las más representativas)*:
* **Blue:** Para dominar Metasploit contra sistemas Windows (EternalBlue).
* **Armageddon:** Clásica para entender cómo vulnerar un Drupal (muy común en el examen).
* **Jerry:** Tomcat con credenciales por defecto. Te enseñará a probar lo obvio antes de volverte loco.
* **Devel y Lame:** Excelentes para explotación básica (SMB y FTP).
* **Blocky y Nibbles:** Metodología muy similar a lo que pide el examen.

---

## 🧠 Mis Consejos de Oro (Cheat Sheet para el aprobado)

1. **Lee las preguntas ANTES de hackear:** Las preguntas del test te dan pistas vitales. A veces te preguntan *"¿Cuál es la contraseña del usuario 'Jason' en la máquina X?"*. Automáticamente ya tienes un usuario válido para meterlo en Hydra y reventar el login por fuerza bruta.
2. **Documenta y Dibuja:** Abre Obsidian o CherryTree y un lienzo de draw.io (o Excalidraw). Dibuja tu red, apunta cada IP, cada puerto, cada usuario y contraseña que encuentres. Las preguntas del examen te van a pedir datos muy específicos y si no eres ordenado, estarás perdido.
3. **K.I.S.S (Keep It Simple, Stupid):** Esto no es un CTF. No intentes encadenar tres vulnerabilidades web rarísimas. Piensa en simple: contraseñas por defecto (`admin/admin`), fuerza bruta directa con el diccionario `rockyou.txt` o *exploits* públicos muy conocidos en Metasploit.
4. **Tírale Fuerza Bruta a TODO:** Panel de login que veas, FTP, SSH o SMB... lánzale Hydra o CrackMapExec mientras miras otras cosas.
5. **Respira y Descansa:** Tienes 48 horas. Yo he visto a gente atascarse, irse a pasear al perro, dormir y sacar la máquina al día siguiente en 5 minutos. Tu cerebro necesita reiniciar.

> Ve con confianza, monta tu "Cheat Sheet" personal (tu chuleta de comandos de Nmap, Metasploit y Pivoting), ten tus diccionarios a mano y revienta esa red. **¡A por la chapita!** 🚀

---

## 🏆 Mi Certificado: Super Orgulloso

¡Y aquí está el resultado de todo el estudio y la práctica! Comparto mi certificado eJPTv2 súper orgulloso de este logro. Ha sido un camino exigente pero increíblemente gratificante. Espero que esta guía te sirva de ayuda para conseguir el tuyo. ¡Mucho ánimo y a hackear con ética! 🎓💻

![Certificado oficial eJPTv2](https://media.licdn.com/dms/image/v2/D4E22AQGcZ5rbwhKLfg/feedshare-shrink_800/B4EZvRlpxIKEAg-/0/1768747867496?e=1777507200&v=beta&t=aF1WrjtjO_V6yqXaZtmVSD21x9lJ8qTq6tqCzNvOJGI)
