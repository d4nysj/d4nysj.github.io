---
title: "Relapse: Jailbreak para casi cualquier PS5"
date: 2026-10-08 11:19:35 +0200
categories: ["Tecnologia", "Consolas"]
tags: ["ps5", "jailbreak", "ciberseguridad", "hacking"]
image:
  path: /assets/img/posts/2026-10-08-relapse-jailbreak-para-casi-cualquier-ps5.jpg
  alt: "Relapse: Jailbreak para casi cualquier PS5"
---

Hace un par de días saltó la noticia que media escena llevaba esperando desde hace meses: ntfargo subió a GitHub **Relapse**, un jailbreak que deja la PS5 con las puertas abiertas de par en par. Si tu consola está en un firmware entre el 7.00 y el 13.60, estás dentro.

La parte técnica tiene su gracia. No es un fallo, son dos encadenados. Primero ataca JavaScriptCore — el motor que mueve el navegador de la consola — con una corrupción de memoria que le deja ejecutar código desde ahí dentro. Hasta aquí, control limitado: estás en el navegador, no en el sistema. El segundo paso es el bueno: una condición de carrera tipo use-after-free en el kernel que, bien encadenada con lo anterior, te da lectura y escritura sobre la memoria del sistema entero. Traducido: pasas de "estoy viendo una web" a "puedo tocar lo que me dé la gana".

Ahora la parte menos glamurosa, porque esto no es tan sencillo como pulsar un botón. No es persistente — apagas la consola y vuelves a empezar desde cero, cadena de exploits incluida. La fase de WebKit falla más de lo que debería, y la del kernel tiene la mala costumbre de colgar la consola o reiniciarla sola a mitad del proceso. Si actualizaste a la 14.00 (salió a mediados de septiembre), olvídate: Sony ya tapó el agujero y de momento nadie ha encontrado la vuelta.

¿Merece la pena? Si te gusta trastear, sí, es un juguete nuevo precioso. Si solo quieres jugar sin dramas, vas a pasar más tiempo reiniciando la consola que jugando, y no te lo recomiendo.

Un consejo si estás en firmware vulnerable y no piensas tocarlo todavía: desconéctala de internet ya, para que no se actualice sola y pierdas la ventana. Y si decides meterle mano, hazlo sabiendo que forzar reinicios constantes a una consola nunca le sienta bien al hardware a largo plazo — nada que vaya a explotar, pero tampoco le estás haciendo ningún favor.

---
*Fuente: [GBHackers](https://gbhackers.com/sony-ps5-relapse-jailbreak-exploit-uses/)*
