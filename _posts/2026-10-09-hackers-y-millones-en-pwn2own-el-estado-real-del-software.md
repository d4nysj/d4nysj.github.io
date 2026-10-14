---
title: "Hackers y millones en Pwn2Own: el estado real del software"
date: 2026-10-09 08:01:01 +0200
categories: ["Ciberseguridad", "Tecnología"]
tags: ["ciberseguridad", "zero-day", "pwn2own", "hacking"]
image:
  path: /assets/img/posts/2026-10-09-hackers-y-millones-en-pwn2own-el-estado-real-del-software.jpg
  alt: "Hackers y millones en Pwn2Own: el estado real del software"
---

Acaba de terminar la Pwn2Own de Irlanda y los números son de esos que te hacen releer la noticia dos veces: más de 1,2 millones de dólares repartidos y 98 zero-days encontrados. Noventa y ocho. En cuatro días. Si trabajas en esto, para un segundo; si no, también, porque el software que acaban de reventar probablemente lo tienes en casa sin saberlo.

Para quien no conozca el percal: Pwn2Own no es un CTF de aficionados buscando el típico fallo de un plugin de WordPress. Es gente muy buena, con meses de trabajo detrás, rompiendo routers, impresoras de oficina y software "serio" que todo el mundo da por hecho que está blindado. Y lo están rompiendo en directo, delante de los propios fabricantes, a cambio de un cheque — que visto lo visto, les sale barato.

Y aquí va lo bueno de verdad: que se pague tanto por encontrar estos fallos es, en realidad, una buena noticia. Si el investigador se lleva el dinero por avisar en vez de vendérselo a quien peor te lo quiera hacer, todos salimos ganando un poco. El problema no es que existan los zero-days — eso siempre va a pasar — el problema es el volumen. 98 en una sola edición es una barbaridad, y lo que no sale en Pwn2Own es el resto del iceberg: lo que no se presenta a concurso y se vende directamente al mejor postor.

Lo que a mí más me preocupa no es el fallo en sí, es la cadena. Una impresora comprometida no es "solo una impresora" — es la puerta de atrás para moverte por toda la red hasta llegar a algo que sí duele de verdad. Y ese salto de cacharro tonto a servidor con datos reales pasa muchas más veces de las que nos gustaría admitir.

¿Qué haces con esto mañana por la mañana? Tres cosas, sin darle más vueltas: actualiza lo que tengas pendiente de actualizar — ya, no la semana que viene; si un dispositivo no necesita estar expuesto a internet, córtale el acceso, no le pongas la vida fácil a nadie; y asume que, tarde o temprano, algo de tu red va a caer, así que segmenta para que si entran por la impresora no acaben paseándose por la base de datos.

La seguridad perfecta no existe. Pero currarte un poco la higiene básica te quita de encima el 90% de los sustos, y el otro 10% ya es mala suerte que ni Pwn2Own arregla.

---
*Fuente: [BleepingComputer](https://www.bleepingcomputer.com/)*
