---
title: "La trampa de las dependencias externas"
date: 2026-11-02 08:00:31 +0200
categories: ["Ciberseguridad", "Desarrollo"]
tags: ["supply chain", "javascript", "seguridad", "web"]
image:
  path: /assets/img/posts/2026-11-02-la-trampa-de-las-dependencias-externas.jpg
  alt: "La trampa de las dependencias externas"
---

Lo de Polymarket esta semana es para hacérselo mirar, aunque siendo sincero, es el recordatorio anual de que vivimos en un castillo de naipes digital. Si no te has enterado, unos atacantes se han llevado 2,7 millones de dólares de la plataforma porque alguien logró inyectar código malicioso en un script de DebugBear que Polymarket tenía cargado en su web. Sí, has leído bien. No hackearon el núcleo del exchange ni encontraron una vulnerabilidad loca en sus contratos inteligentes; simplemente comprometieron una dependencia externa que sirve para medir el rendimiento de la carga.

A mí lo que más me mosquea de esto no es que el ataque sea sofisticado, sino lo rutinario que es el vector. Básicamente, tú confías en un tercero, ese tercero tiene una brecha, y de repente, tu página web, que debería ser un entorno seguro, se convierte en un colador. Los atacantes reemplazaron un script legítimo de optimización por uno que, imagino, hacía de puente para robar las credenciales o los fondos de los usuarios que se conectaban con sus carteras. Es la definición perfecta de cómo un eslabón invisible puede tumbar toda la seguridad de una aplicación que mueve millones.

Esto me recuerda a cuando metemos paquetes de npm sin mirar qué hay debajo del capó o cuando añadimos SDKs de analíticas que cargan cien megas de scripts de terceros. Todos queremos que la web vuele, que cargue en milisegundos y que las métricas de rendimiento sean perfectas, pero al final del día, cada línea de código que inyectas de fuera es un cheque en blanco que firmas a favor de un extraño. Si ese extraño tiene la seguridad de un colador o si, como en este caso, su servidor es el punto de entrada, el marrón te lo comes tú y, sobre todo, tus usuarios.

Es curioso leer el análisis en The Hacker News porque te das cuenta de que el problema no es la tecnología blockchain en sí, que es lo que siempre critican los agoreros, sino el ecosistema web tradicional que rodea a estas plataformas. Polymarket es puntera en su sector, pero su web, a ojos del navegador, es igual de vulnerable que el blog de un adolescente si no se auditan las dependencias externas con lupa. El concepto de cadena de suministro en el software empieza a darme más miedo que las brechas de día cero de las que tanto se habla en las películas.

Al final, esto deja una pregunta incómoda: ¿estamos optimizando tanto el rendimiento y la comodidad del desarrollador que estamos sacrificando lo mínimo necesario para que el usuario no pierda hasta la camisa? Me da a mí que mientras sigamos cargando scripts de terceros como si fueran caramelos, vamos a seguir viendo este tipo de historias. Personalmente, creo que tarde o temprano vamos a tener que empezar a aislar mucho más el frontend de los servicios críticos, porque confiar en que el script de analíticas de turno no se convierta en un troyano es, sencillamente, una apuesta que no puedes ganar siempre.

---
*Fuente: [The Hacker News](https://thehackernews.com/)*
