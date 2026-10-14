---
title: "Pwn2Own 2026: El día que el S26 y tu casa inteligente se rindieron"
date: 2026-10-08 10:59:06 +0200
categories: ["Ciberseguridad", "Noticias"]
tags: ["pwn2own", "zero-day", "samsung", "ciberseguridad", "hacking"]
image:
  path: https://live.staticflickr.com/7517/15327725543_9e22232f14_b.jpg
  alt: "Pwn2Own 2026: El día que el S26 y tu casa inteligente se rindieron"
---

Ponte en situación: te acabas de comprar el Galaxy S26, vas presumiendo de que es un tanque, y antes de que te dé tiempo a terminarte el café un grupo de investigadores en Cork lo deja abierto en canal. Así ha arrancado el día uno de Pwn2Own Ireland 2026, y la cifra del primer día ya marea: 32 zero-days encontrados en 24 horas. No hablo de fallos de manual, hablo de puertas traseras que nadie sabía que existían hasta que alguien decidió llamar a la puerta.

El protagonista negativo ha sido justamente el S26, que ha caído tres veces. La que más duele es la del equipo japonés Ikotas, que encadenó cuatro vulnerabilidades distintas para hacerse con el control remoto total del teléfono — y de esas cuatro, Samsung solo tenía constancia de una. El resto era terreno completamente virgen. Se llevaron 11.000 dólares por el trabajo, y un recordatorio de que por mucha IA y mucho marketing que le metan a un móvil, debajo sigue habiendo código, y el código falla.

Y si pensabas que tu casa domotizada te libraba de esto, malas noticias: el Philips Hue Bridge Pro — el puente que controla las lucecitas del salón — cayó tras encadenar siete zero-days. Siete. Lo interesante, y lo que de verdad debería preocuparte, es que desde ahí se puede saltar de la bombilla a toda tu red doméstica. También cayeron cosas de más peso, como Oracle Autonomous Database, y OpenAI Codex con una inyección de argumentos de manual.

Lo que saco en claro de todo esto es que la seguridad por oscuridad simplemente no existe a estos niveles — si quieres proteger algo de verdad, tienes que asumir que quien va a por ello tiene más tiempo y más recursos que tú para encontrar justo el bug que a ti se te pasó. Así que lo de siempre, pero dicho una vez más por si no ha quedado claro: actualiza en cuanto puedas (muchas de estas cadenas se rompen con cerrar aunque sea una sola puerta), mete tu domótica en una VLAN separada para que si te hackean la bombilla no lleguen a tu ordenador de trabajo, y no te fíes a ciegas de la IA — la inyección de prompts y argumentos se está convirtiendo en el nuevo puerto 80 abierto de par en par.

Si te va el detalle técnico de cada hazaña, la Zero Day Initiative tiene el resumen completo. Y si no, quédate con esto: la tecnología avanza rápido, pero últimamente los exploits van todavía más deprisa.

---
*Fuente: [Zero Day Initiative](https://www.thezdi.com/blog/2026/10/6/pwn2own-ireland-2026-day-one-results)*
