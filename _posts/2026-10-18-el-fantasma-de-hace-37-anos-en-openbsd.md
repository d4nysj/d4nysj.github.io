---
title: "El fantasma de hace 37 años en OpenBSD"
date: 2026-10-18 08:00:30 +0200
categories: ["Ciberseguridad", "Sistemas"]
tags: ["openbsd", "kernel", "exploits", "vulnerabilidad", "seguridad"]
image:
  path: /assets/img/posts/2026-10-18-el-fantasma-de-hace-37-anos-en-openbsd.jpg
  alt: "El fantasma de hace 37 años en OpenBSD"
---

A veces me pregunto si los sistemas operativos antiguos son como esos coches clásicos que, por mucho que les cambies el motor, siguen teniendo el chasis oxidado por algún sitio que nadie quiere mirar. Acabo de leer en The Hacker News que han sacado un exploit para OpenBSD 7.9, y la historia tiene miga. Lo más cachondo es que para conseguir privilegios de root han tirado de un bug de memoria compartida que tiene la friolera de 37 años. Sí, has leído bien. Casi cuatro décadas ahí escondido, esperando a que alguien con paciencia infinita lo conectara con una vulnerabilidad moderna del kernel.

Es increíble el nivel de detalle que hace falta para encadenar esto. Normalmente pensamos que cuanto más viejo es el código, más auditado está, pero aquí queda claro que simplemente nadie se había molestado en mover esa pieza concreta del puzzle. El ataque es interesante porque se pasa por el forro las protecciones KARL, que básicamente es el mecanismo de OpenBSD para aleatorizar el layout del kernel en cada arranque. Si consigues saltarte eso, básicamente le estás diciendo al sistema que tu exploit conoce el mapa del tesoro mejor que el propio dueño de la casa.

A mí lo que más me mosquea de esto no es el bug en sí, sino lo que dice sobre la complejidad de nuestros sistemas actuales. Nos obsesionamos con meter parches sobre parches, con capas de abstracción y con medidas de seguridad que parecen salidas de una película de ciencia ficción, pero al final dependemos de librerías y funciones que se escribieron cuando todavía se escuchaba música en cassette. Es un recordatorio bastante brusco de que la "seguridad por diseño" suena muy bien en los folletos de marketing, pero en la práctica es un ejercicio de equilibrista constante sobre una cuerda que cada vez tiene más remiendos.

No es que OpenBSD sea malo, ni mucho menos. De hecho, si alguien se ha molestado en montar un exploit tan complejo es porque precisamente es un sistema que no se lo pone fácil a cualquiera. Lo que me da un poco de rabia es la falsa sensación de invulnerabilidad que a veces nos autoimponemos. Parece que por usar un sistema enfocado a la seguridad ya estamos a salvo de todo, y luego llega un fallo que estaba ahí desde antes de que yo naciera y te desmonta el chiringuito.

Al final, me queda la sensación de que cuanto más viejo es el software, más peligroso es si no se entiende profundamente lo que hay debajo del capó. No basta con actualizar el kernel cada martes; hay que saber qué estamos arrastrando. Me encantaría decir que esto servirá para limpiar el código de una vez, pero todos sabemos cómo va la historia: arreglarán este fallo, sacarán el parche, y dentro de otros diez años alguien encontrará otra pieza oxidada que nadie había tocado desde los ochenta. Al menos, mientras sigan apareciendo estas cosas, el trabajo no nos va a faltar a los que nos gusta trastear con las tripas de los sistemas.

---
*Fuente: [The Hacker News](https://thehackernews.com)*
