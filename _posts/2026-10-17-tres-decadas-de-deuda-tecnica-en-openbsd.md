---
title: "Tres décadas de deuda técnica en OpenBSD"
date: 2026-10-17 08:02:36 +0200
categories: ["Ciberseguridad", "Sistemas"]
tags: ["openbsd", "kernel", "exploits", "vulnerabilidades"]
image:
  path: /assets/img/posts/2026-10-17-tres-decadas-de-deuda-tecnica-en-openbsd.jpg
  alt: "Tres décadas de deuda técnica en OpenBSD"
---

Hace un par de días me topé con una noticia en The Hacker News que me dejó pensando un buen rato. Resulta que alguien ha encontrado una vía para escalar privilegios a root en OpenBSD 7.9, y lo gracioso es que la base del exploit es un bug de desbordamiento de enteros que lleva ahí treinta y siete años. Casi nada. Es el tipo de cosas que te recuerdan que el software perfecto no existe, solo código que simplemente no hemos tenido suficiente tiempo o interés en romper todavía.

Lo que mola de esta historia, si es que se puede llamar así, es cómo han combinado una reliquia arqueológica con un fallo en el kernel para saltarse la protección KARL. Básicamente, han hecho un puzzle con una pieza que ya existía antes de que muchos de nosotros naciéramos y otra pieza actual. Es una lección de humildad para el ecosistema de BSD, que siempre ha presumido de esa limpieza de código casi obsesiva. Al final, un entero que desborda es un entero que desborda, da igual si lo escribiste en 1988 o la semana pasada.

A mí lo que más me mosquea de esto no es el bug en sí, sino la persistencia. Vivimos en una burbuja donde parece que si un sistema se audita constantemente, es impenetrable. Luego llega alguien, tira de hemeroteca y te demuestra que el código puede esconder bombas de relojería que han pasado desapercibidas por tres décadas de ojos expertos. Me imagino al desarrollador original viendo esto y no sé si morirse de vergüenza o tomárselo como un trofeo por haber creado algo que ha aguantado tanto sin que nadie se diese cuenta de la grieta.

Es curioso cómo tratamos la ciberseguridad como si fuera una carrera de armamentos donde siempre estamos un paso por delante, cuando la realidad es que el código viejo es un campo de minas. Muchas veces, la seguridad por oscuridad o simplemente el hecho de que nadie se haya molestado en mirar en un rincón concreto es lo que nos mantiene a flote. OpenBSD ha respondido rápido, como era de esperar, pero el susto ya está ahí.

Al final, este caso sirve para recordarnos que ninguna capa de protección, por muy moderna que sea, es mágica. Los investigadores han tenido que trabajar fino para combinar ese error histórico con el fallo actual y sortear KARL, lo que demuestra que la complejidad en el kernel sigue siendo nuestro mayor enemigo. Si el código es tan antiguo que ya ni sus padres lo reconocen, el riesgo de que alguien encuentre una nueva forma de usarlo en tu contra crece exponencialmente. Yo de momento voy a seguir actualizando los parches al minuto, pero con la mosca detrás de la oreja, porque si algo nos ha enseñado esta noticia es que la longevidad del código no es siempre sinónimo de fiabilidad.

---
*Fuente: [The Hacker News](https://thehackernews.com/2025/02/researchers-demonstrate-openbsd-root.html)*
