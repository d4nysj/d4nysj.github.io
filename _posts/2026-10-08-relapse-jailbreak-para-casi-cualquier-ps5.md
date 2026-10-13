---
title: "Relapse: Jailbreak para casi cualquier PS5"
date: 2026-10-08 11:19:35 +0200
categories: ["Tecnologia", "Consolas"]
tags: ["ps5", "jailbreak", "ciberseguridad", "hacking"]
image:
  path: /assets/img/posts/2026-10-08-relapse-jailbreak-para-casi-cualquier-ps5.jpg
  alt: "Relapse: Jailbreak para casi cualquier PS5"
---

Hace un par de días saltó la noticia que muchos esperaban en la escena de la scene. Un desarrollador llamado ntfargo soltó en GitHub **Relapse**, un jailbreak que básicamente deja las puertas de la PlayStation 5 abiertas de par en par. Si tienes una consola con un firmware entre el 7.00 y el 13.60, estás dentro.

## Cómo funciona el truco

La magia detrás de esto no es una sola vulnerabilidad, sino una combinación de dos fallos técnicos bastante elegantes. Primero, el exploit ataca **JavaScriptCore**, el motor que mueve el navegador de la consola. Mediante una corrupción de memoria, el atacante logra ejecutar código arbitrario desde el propio navegador.

Una vez ahí, no tienen el control total, así que utilizan una segunda vulnerabilidad: una **condición de carrera (race condition)** de tipo *use-after-free* en el kernel. Al encadenar ambos fallos, saltan del navegador a obtener lectura y escritura completa sobre la memoria del sistema. En lenguaje sencillo: consiguen los permisos necesarios para modificar lo que quieran en la consola.

## Lo que necesitas saber antes de tocar nada

Aquí es donde entra la realidad. Esto no es un proceso automático ni estable.
* **No es persistente:** Cada vez que apagas o reinicias la PS5, pierdes el acceso. Tienes que volver a lanzar la cadena de exploits desde cero.
* **La estabilidad es un chiste:** No siempre funciona a la primera. La parte del WebKit suele pedir varios intentos y, en la fase del kernel, es muy común que la consola se bloquee o se reinicie sola. Ten paciencia o te vas a frustrar rápido.
* **El muro de Sony:** Si actualizaste a la versión 14.00, lanzada a mediados de septiembre de 2026, olvídate. Sony tapó el agujero y, ahora mismo, no hay marcha atrás conocida.

Si estás en una versión vulnerable, el flujo técnico es básicamente este:

```javascript
// Esquema simplificado del exploit
if (webkit_exploit()) {
    trigger_race_condition_kernel();
    if (check_rw_access()) {
        enable_homebrew_capabilities();
    } else {
        panic_and_reboot(); // Es lo que pasa el 50% de las veces
    }
}
```

## ¿Vale la pena?

Si eres un entusiasta de la seguridad, de la preservación de software o simplemente te gusta cacharrear, es una oportunidad de oro para trastear con el sistema operativo de Sony. Si lo que quieres es jugar sin complicaciones, prepárate para perder más tiempo reiniciando la consola que jugando.

La información técnica detallada proviene originalmente de **GBHackers**. Si decides probarlo, hazlo bajo tu propia responsabilidad. Entiende que estás forzando a un sistema a hacer algo para lo que no fue diseñado y los reinicios inesperados, aunque sean parte del proceso, nunca son amigos de los componentes electrónicos a largo plazo.

Mi recomendación: si tienes una consola en un firmware vulnerable, **desconéctala de internet** ya mismo para evitar actualizaciones automáticas. Quédate donde estás y sigue el progreso de la comunidad, porque esto acaba de empezar y es probable que la estabilidad mejore en los próximos meses.

---
*Fuente: [GBHackers](https://gbhackers.com/sony-ps5-relapse-jailbreak-exploit-uses/)*
