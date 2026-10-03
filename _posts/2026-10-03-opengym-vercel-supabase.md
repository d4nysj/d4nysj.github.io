---
title: "Monté mi propio OpenGym gratis en Vercel (y por qué casi nadie lo necesita menos que tú)"
date: 2026-10-03 09:00:00 +0200
categories: ["Proyectos", "DevSecOps"]
tags: ["opengym", "vercel", "supabase", "selfhosted", "react"]
image:
  path: https://picsum.photos/seed/opengym-vercel-supabase/1200/630
  alt: "Monté mi propio OpenGym gratis en Vercel"
---

Llevo un tiempo entrenando a gente y necesitaba algo para llevarles las rutinas. Las apps que hay o cobran suscripción, o tienen anuncios, o directamente son un Excel con pretensiones. Así que hice lo que hace cualquier friki con ganas de perder un fin de semana: cogí [openGym](https://github.com/DuarteSantos8/openGym) (un tracker de gimnasio open source que ya estaba muy bien hecho) y lo retoqué a mi manera.

Repo: **[github.com/d4nysj/opengym](https://github.com/d4nysj/opengym)** (rama `vercel-deploy`, que es donde está el lío de verdad).

## ¿Por qué no usar el proyecto original tal cual?

Porque el original se autoaloja con `docker compose up` y punto — genial si tienes un servidor en casa encendido 24/7 (yo lo tengo, para mis cosas), pero si quiero invitar a clientes de verdad a usarlo, prefiero algo que no dependa de que mi Raspberry Pi no se caiga un domingo. Quería:

- **Cero coste** — ni VPS, ni suscripción.
- **Invitación cerrada** — nadie se registra si yo no le doy un código.
- **Rutinas preasignadas** — cuando invito a alguien, ya le dejo su plan de entreno puesto, no tiene que montárselo él.

Nada de esto rompe el espíritu del proyecto original, solo lo adapta a "quiero dar esto como servicio a mis clientes" en vez de "quiero esto para mí en mi red local".

## Qué cambié de verdad (la parte técnica)

El proyecto original guarda todo en ficheros JSON en disco, con un proceso Node que vive siempre encendido. Eso es perfecto en Docker, pero **no funciona en serverless** — Vercel no te garantiza que el mismo proceso siga vivo entre petición y petición, así que cualquier cosa en memoria o en disco local desaparece.

La solución: sustituir esa capa entera por **Supabase (Postgres)**. Reescribí las rutas de la API como funciones serverless (`api/[...all].js`) que leen y escriben en Supabase en vez de en `db.json`. La lógica de negocio, el frontend en React y el login con passkeys (WebAuthn) son exactamente los mismos que en el proyecto original — no tenía sentido tocar lo que ya funcionaba bien.

Otras cosas que tuve que resolver:

| Problema | Cómo lo resolví |
|---|---|
| Los avisos de "se acabó el descanso" dependían de un timer en memoria | Movido a un cron externo — **GitHub Actions**, cada 5 minutos, gratis y sin los límites del plan Hobby de Vercel |
| El Coach de IA necesita spawnear procesos CLI persistentes | Deshabilitado en esta rama — no existe eso en serverless, y para mi caso de uso no lo necesitaba |
| Registro abierto a cualquiera | Modo `INVITE_ONLY` + panel de admin para generar códigos de un solo uso |
| Dar de alta a un cliente y que ya tenga su rutina puesta | Un campo `preset_state` en la invitación que se copia al perfil en cuanto canjean el código — cero pasos extra para el cliente |

¿La pega? El Coach de IA no funciona en esta versión, y los avisos llegan con un margen de minutos (no al segundo) porque dependen del cron externo. Para mi caso, ningún drama.

## Cómo lo corres en local

Aviso honesto antes de nada: **si solo quieres openGym para ti**, no necesitas nada de lo que viene ahora. Clona el [original](https://github.com/DuarteSantos8/openGym), haz `docker compose up` y listo, en 5 minutos lo tienes andando sin tocar una sola variable de entorno. Esta rama mía tiene sentido si quieres dar acceso a terceros sin montar infraestructura propia.

Dicho esto, si quieres levantar **mi versión** en local para toquetearla:

```bash
git clone https://github.com/d4nysj/opengym
cd opengym
git checkout vercel-deploy
npm i -g vercel
```

Necesitas un proyecto de [Supabase](https://supabase.com) (el plan gratis sobra) con las tablas que usa la app. Luego, variables de entorno en un `.env.local`:

```bash
SUPABASE_URL=https://tu-proyecto.supabase.co
SUPABASE_SERVICE_ROLE_KEY=tu_service_role_key   # el secreto, no el "anon"
SESSION_SECRET=algo_random_largo
VAPID_PUBLIC_KEY=...
VAPID_PRIVATE_KEY=...
RP_ID=localhost
ORIGIN=http://localhost:3000
```

Y arrancas con:

```bash
vercel dev
```

Esto levanta tanto el frontend como las funciones serverless en local, simulando lo que hace Vercel en producción — es la forma más fiel de probarlo sin desplegar nada todavía. Entras en `http://localhost:3000`, creas tu perfil y ya estás dentro.

Dos cosas que en local NO van a funcionar igual que en producción:
- El cron de GitHub Actions no corre en tu máquina, así que los avisos de descanso no van a llegarte solo (tendrías que forzar la llamada a mano).
- Las passkeys están atadas al dominio (`RP_ID`) — en local funcionan con `localhost`, pero si luego despliegas a un dominio real, tendrás que crear tu perfil de nuevo ahí.

## La conclusión

Esto no es "mejor" que el openGym original — es el mismo proyecto con un cambio de tripas para que encaje en un caso de uso muy concreto: dar acceso a terceros sin mantener un servidor. Si tu caso es "quiero trackear mis entrenos yo", el original con Docker te va a ir mejor y con menos piezas moviéndose. Si tu caso es el mío, aquí tienes el camino ya andado.

Todo el detalle de despliegue (variables de entorno, orden exacto para activar el modo invitación sin quedarte fuera tú mismo) está en [`docs/VERCEL_DEPLOY.md`](https://github.com/d4nysj/opengym/blob/vercel-deploy/docs/VERCEL_DEPLOY.md) del repo.
