---
title: "El desastre de LiteLLM y por qué sigues exponiendo secretos"
date: 2026-10-25 08:01:19 +0200
categories: ["Ciberseguridad", "Desarrollo"]
tags: ["litellm", "seguridad", "github", "devops", "supplychain"]
image:
  path: /assets/img/posts/2026-10-25-el-desastre-de-litellm-y-por-que-sigues-exponiendo-secretos.jpg
  alt: "El desastre de LiteLLM y por qué sigues exponiendo secretos"
---

Lo de LiteLLM de esta semana me ha dejado el cuerpo bastante raro. Si no has visto la noticia, básicamente alguien dejó una llave maestra colgada en un repositorio público y, claro, los atacantes hicieron lo que cualquiera con dos dedos de frente y malas intenciones haría: entrar hasta la cocina. No es el primer caso de una llave expuesta en GitHub y, lamentablemente, no será el último, pero el nivel de desastre aquí es de manual.

Lo que me mosquea de este caso, y lo que cuento porque lo he leído en The Hacker News, es que no solo se expusieron secretos, sino que los atacantes lograron inyectar código directamente en el pipeline de despliegue. Imagínate la situación: te descargas una librería pensando que todo está controlado y resulta que el paquete que instalas lleva un regalito sorpresa que alguien metió mientras el equipo de desarrollo quizás estaba tomando café. Es el escenario de pesadilla de cualquier desarrollador. Confías en la cadena de suministro, confías en el proceso automatizado y al final resulta que una variable de entorno mal gestionada tira todo el castillo de naipes abajo.

A mí lo que más me quema es que seguimos viendo el mismo error de siempre en 2024. Gestionar secretos en repositorios parece algo que ya deberíamos tener superado con herramientas como Vault, AWS Secrets Manager o simplemente no ser tan descuidados con lo que subimos a un commit. Pero ahí estamos, una y otra vez viendo cómo una simple cadena de texto de un token de GitHub acaba comprometida porque a alguien se le olvidó borrar un archivo o no configuró bien el .gitignore.

Al final, este tipo de ataques demuestran que el eslabón más débil no es el cifrado de última generación ni el firewall más caro que puedas pagar, sino el descuido humano a las tres de la mañana cuando intentas cerrar un despliegue rápido. La automatización es genial para mover código, pero si el acceso a esa automatización está abierto de par en par, estás creando una autopista directa para que cualquiera con un script se convierta en el dueño de tu infraestructura.

Es frustrante ver cómo una herramienta que intenta facilitar el uso de LLMs termina siendo el vehículo para una brecha de este calibre. Lo que me hace pensar es cuántos otros proyectos están ahora mismo con llaves maestras expuestas sin que nadie se haya dado cuenta todavía. Si tienes alguna integración con llaves de API o tokens de despliegue en tus repositorios, hazte un favor y revisa hoy mismo qué hay en tu historial de commits. A veces el miedo es el mejor auditor de seguridad.

---
*Fuente: [The Hacker News](https://thehackernews.com/)*
