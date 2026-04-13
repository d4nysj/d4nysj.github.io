---
title: "La Cruda Realidad del eCPPTv3: Guía Definitiva y 'A Tiro Hecho' para Aprobar"
date: 2026-04-16 09:00:00 +0200
categories: [Certificaciones, Ciberseguridad]
tags: [ecpptv3, pentesting, active-directory, hacking, redteam]
image:
  path: /assets/img/ecppt.jpg
  alt: Certificado oficial eCPPTv3 de eLearnSecurity
---

Si estás pensando en sacarte el **eCPPTv3**, borra de tu mente todo lo que te hayan contado sobre la versión 2. El examen ha mutado por completo: ya no hay que redactar un reporte de pentesting de una semana, ya no hay Buffer Overflow y despídete del dolor de cabeza del Pivoting extremo.

Ahora te enfrentas a un examen de **24 horas** y un cuestionario tipo test de **45 preguntas**. Suena más fácil, ¿verdad? Pues prepárate, porque la "faena" de este examen viene por otro lado. Aquí te cuento la verdad sin filtros.

---

## 🚩 La Faena: El Entorno y las Herramientas Rotas

El mayor enemigo del eCPPTv3 no es el Active Directory, es la propia plataforma de INE.

* **Aislamiento total:** Te dan acceso a una máquina Kali a través del navegador (Guacamole) sin conexión a internet. No puedes descargar herramientas externas. **Tip:** Usa Google Chrome; Firefox da problemas con el portapapeles.
* **Herramientas rotas:** Vas a sudar sangre porque herramientas críticas como `evil-winrm`, `Hashcat` local o ciertos módulos de `CrackMapExec` fallan por errores de configuración del entorno.
* **El botón de reinicio:** Si llevas horas buscando un usuario que el test dice que existe y no lo ves, **reinicio del laboratorio**. Tarda 10 minutos, pero a veces es la única forma de que carguen las *flags*.

---

## 🤫 El Secreto para Aprobar: Fuerza Bruta y Metasploit

INE dice que "si un ataque tarda más de 20 minutos, vas mal". **Mentira.** Este examen es un festival de fuerza bruta. Usa estos diccionarios en este orden exacto:

1.  `seasons.txt`
2.  `months.txt`
3.  `xato-net-10-million-passwords-10000.txt`
4.  `common_corporate_passwords.lst`
5.  `rockyou.txt` (Último recurso).

**Usa Metasploit sin remordimientos:** Al estar aislado y con herramientas que fallan, Metasploit es tu salvavidas. Para escalada en Windows, no pierdas el tiempo: tira un `getsystem` o usa el módulo `local_exploit_suggester`.

---

## 💻 El Enfoque Técnico: Qué estudiar

El curso oficial se queda corto en **Active Directory** (que es el 70% del examen). Debes dominar:

* **Acceso inicial:** Vulnerar máquinas Linux (Web/CMS) para ganar el *foothold*.
* **Ataques AD:** AS-REP Roasting, Kerberoasting, enumeración con BloodHound, Password Spraying y Pass the Hash.

---

## 🗺️ Máquinas a Tiro Hecho (Máximo Valor)

Esta es la ruta exacta recomendada por los que ya han aprobado:

### 1. Acceso Inicial (DockerLabs - Gratis)
* **Move:** Ideal para explotación básica.
* **Working CMS / Find Your Style:** Fundamentales para agilidad en WordPress/CMS.

### 2. Active Directory (Hack The Box)
Paga un mes de HTB Academy y haz el módulo **"Active Directory Enumeration & Attacks"**. Es obligatorio.
* **Máquinas HTB (Fácil/Medio):** Active, Sauna, Forest, Cascade, Authority, Hospital.
* **HackMyVM:** Máquinas DC01, DC02 y DC03.

---

## 📝 Recomendación Final para el Día D

Usa las **45 preguntas del test a tu favor**. Léelas antes de empezar. A veces te preguntan *"¿Cuál es la contraseña del usuario X?"*, dándote el mapa exacto de a quién debes atacar.

Organiza todo en **Obsidian** o **CherryTree**: anota cada IP, usuario y hash. Si te atascas, respira, lanza `seasons.txt` por fuerza bruta, tómate un café y vuelve. ¡A por ellos! 🚀

---

## 🏆 Mi Certificado: Objetivo Cumplido

Comparto mi certificación eCPPTv3, un examen que pone a prueba tu paciencia y tu capacidad de adaptación en entornos restringidos. 

*(Haz clic en la imagen para ver mi publicación en LinkedIn)*

[![Certificado oficial eCPPTv3](/assets/img/ecppt.jpg)](https://www.linkedin.com/feed/update/urn:li:activity:7418666260957396992/)
