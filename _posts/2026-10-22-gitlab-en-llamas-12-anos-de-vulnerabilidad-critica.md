---
title: "GitLab en llamas: 12 años de vulnerabilidad crítica"
date: 2026-10-22 08:01:31 +0200
categories: ["Ciberseguridad", "DevOps"]
tags: ["gitlab", "rce", "seguridad", "infra", "vulnerabilidad"]
image:
  path: /assets/img/posts/2026-10-22-gitlab-en-llamas-12-anos-de-vulnerabilidad-critica.jpg
  alt: "GitLab en llamas: 12 años de vulnerabilidad crítica"
---

Me he despertado esta mañana con la noticia de GitLab y, sinceramente, es de esas cosas que te hacen cuestionarte por qué seguimos levantando servidores propios. The Hacker News ha publicado que hay una vulnerabilidad crítica con un CVSS de 10.0, que es básicamente el fin del mundo técnico. No es el típico fallo que requiere que el atacante tenga acceso a la red interna o que un usuario haga clic en un enlace sospechoso. Aquí hablamos de ejecución remota de código sin autenticación. Cualquiera con acceso a tu instancia puede hacer lo que le dé la gana.

Lo que más me mosquea de todo esto no es solo la gravedad, que ya es suficiente para no dormir, sino el alcance. Hablamos de versiones desde la 7.0, lanzada hace más de una década. Doce años de historial de software, un legado eterno de código que, de repente, se convierte en una puerta abierta de par en par. Es un recordatorio brutal de que mantener infraestructura autogestionada es un deporte de riesgo. A veces nos creemos muy listos por no depender de la nube o por tener control total sobre nuestros repositorios, pero cuando sale un agujero de este calibre, el "control total" se transforma en una pesadilla logística de parches de emergencia a las tres de la mañana.

Si gestionas tu propio GitLab, espero que estés leyendo esto mientras actualizas, porque no hay margen de maniobra. No estamos ante un fallo que afecte a una librería obscure que nadie usa; es el corazón del pipeline de desarrollo de media industria. Si alguien compromete tu instancia, tiene acceso a tu código fuente, a tus secretos, a tus tokens de API y a todo el flujo de despliegue. Básicamente, les estás dando las llaves de la ciudad. 

A mí lo que me hace gracia, por llamarlo de alguna forma, es cómo normalizamos el tener sistemas tan críticos expuestos. Nos relajamos porque "llevamos años sin incidentes" y de repente llega un reporte de seguridad que tira por tierra una década de supuesta robustez. Si algo he aprendido en estos años es que el código viejo es como una bomba de relojería que a veces ni siquiera sabes que tienes instalada. La gente suele decir que si no está roto no lo toques, pero con esta clase de bugs, el refrán se queda corto. Aquí, si no lo tocas, el que te va a tocar el código es un tercero que no tiene precisamente buenas intenciones.

Al final del día, esto es un toque de atención para revisar qué tenemos rodando en producción y qué nivel de deuda técnica estamos arrastrando. Yo voy a dedicar la tarde a repasar el inventario de servicios y asegurar que no tengo nada con diez años de antigüedad acumulando polvo y vulnerabilidades, porque el susto que se van a llevar algunos equipos de sysadmin hoy no se lo deseo ni a mi peor enemigo. Actualiza ya y reza para que nadie haya estado rastreando tu instancia antes de que saliera el parche.

---
*Fuente: [The Hacker News](https://thehackernews.com)*
