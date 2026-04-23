---
layout: post
title: "Cómo construí un proxy inteligente para no volver a ver un error 429 de Gemini"
date: 2026-04-23 10:00:00 +0100
categories: [python, infraestructura, gemini, api]
tags: [python, flask, google-gemini, api-proxy, sqlite, raspberry-pi, automation]
image: /assets/img/posts/geminikeys-banner.png
description: "Un middleware en Flask que rota API keys y cambia de modelo automáticamente cuando la cuota se agota. Cero cambios en tu código existente."
---

Llevaba semanas trabajando en un proyecto que hacía llamadas intensivas a la API de Google Gemini. El flujo era siempre el mismo: lanzar el script, esperar, ver el temido 429 Too Many Requests, esperar el cooldown y volver a empezar. Un ciclo frustrante que me robaba horas.

La solución obvia era gestionar múltiples cuentas con múltiples API keys y rotar entre ellas. La ejecución de esa idea, sin embargo, requería un refactor feo en cada script que tocaba la API. No me convencía.

Entonces pensé: ¿y si construyo un proxy local que lo gestione por mí?

## La idea central

En lugar de que mi código hable directamente con Google, habla con un servidor local en el puerto 5005. Ese servidor tiene un pool de API keys y una lista de modelos de respaldo. Cuando Google devuelve un error, el proxy lo absorbe, cambia de key o de modelo, y reintenta. Mi código no se entera de nada.

El cambio en el código es de una línea:

# Antes
url = "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=MI_KEY"

# Después
url = "http://localhost:5005/proxy/v1beta/models/gemini-1.5-flash:generateContent"

Eso es todo. Sin cambiar headers, sin cambiar el payload, sin tocar la lógica de negocio.

## La arquitectura

El proyecto tiene tres componentes principales:

1. Base de datos (SQLite)

Tres tablas simples: api_keys para el pool de claves, models para la lista de fallback y logs para el historial de peticiones. SQLite fue la elección obvia: sin servidor, sin configuración y con persistencia local. Para este caso de uso es más que suficiente.

2. Proxy inteligente (Flask)

El corazón del proyecto. Cuando llega una petición a /proxy/<endpoint>, la lógica es:

1. Lee el endpoint para extraer el nombre del modelo mediante regex.
2. Construye una lista de modelos a intentar: primero el original, luego los de fallback.
3. Para cada modelo, intenta con dos keys diferentes del pool (orden aleatorio).
4. Si recibe 200, devuelve la respuesta al cliente.
5. Si recibe 503, pasa al siguiente modelo.
6. Si recibe 400, lo devuelve inmediatamente (error del cliente, no tiene sentido reintentar).
7. Registra cada intento en la tabla de logs.

for modelo_actual in modelos_a_probar:
    endpoint_modificado = endpoint.replace(
        f"models/{modelo_original}:",
        f"models/{modelo_actual}:"
    )
    for _ in range(2):
        llave = llaves_disponibles[intento % len(llaves_disponibles)]
        google_url = f"https://generativelanguage.googleapis.com/{endpoint_modificado}?key={llave}"

        resp = requests.request(method=request.method, url=google_url, ...)

        if resp.status_code == 200:
            return Response(resp.content, 200)  # Éxito
        if resp.status_code == 400:
            return Response(resp.content, 400)  # Error del cliente
        if resp.status_code == 503:
            break  # Pasar al siguiente modelo

3. Panel de control (HTML/CSS)

Una interfaz web sencilla para gestionar el pool sin tocar la base de datos directamente. Desde aquí puedes añadir y eliminar keys y modelos, ver el historial de las últimas 50 peticiones e importar/exportar keys en bloque desde un archivo .txt.

No usé ningún framework frontend. HTML y CSS puro. Para algo así, añadir React o Vue sería matar moscas a cañonazos.

## El despliegue

El caso de uso principal es una Raspberry Pi o un servidor con DietPi haciendo de proxy permanente en la red local. Para eso, un servicio systemd es lo más limpio:

[Unit]
Description=Gemini API Key Rotator Proxy
After=network.target

[Service]
User=root
WorkingDirectory=/root/GeminiKeys
ExecStart=/usr/bin/python3 app.py
Restart=always

[Install]
WantedBy=multi-user.target

Con systemctl enable --now geminikeys, el proxy arranca automáticamente con el sistema y se reinicia si falla.

## Lo que aprendí

El orden de modelos importa.
Los modelos en la lista de fallback se intentan en orden. Pon los más rápidos y baratos primero (gemini-1.5-flash antes que gemini-1.5-pro). Si el objetivo es que una tarea salga adelante sin importar el coste, deja los más potentes al final.

La aleatoriedad en el pool de keys es deliberada.
Si siempre empezaras por la misma key, se agotaría antes que las demás. random.shuffle() distribuye la carga de forma más uniforme.

Separar el 503 del 400 es crítico.
Un 503 significa que el servicio está saturado → reintentar tiene sentido.
Un 400 significa que la petición está mal → reintentar solo desperdicia cuota.

SQLite para logging fue una buena decisión.
Frente a logs en fichero, permite consultas simples como ORDER BY id DESC LIMIT 50 sin complicaciones.

## Lo que añadiría en una v2

- Health check automático de keys
  Endpoint /healthcheck para validar keys y desactivar las inválidas.

- Estadísticas por key
  Detectar cuáles fallan más o están agotadas.

- Soporte streaming
  Actualmente no gestiona stream: true.

- Rate limiting configurable
  Para evitar que el propio proxy dispare demasiadas peticiones.

## El código

El proyecto completo está en GitHub. Tiene dos ficheros principales (app.py y templates/index.html), menos de 200 líneas de código en total, y las únicas dependencias son Flask y requests.

git clone https://github.com/d4nysj/Gemini-API-Key-Rotator-Intelligent-Proxy
cd GeminiKeys
pip install flask requests
python app.py

Si trabajas con Gemini de forma intensiva y estás harto de gestionar manualmente las cuotas, esto puede ahorrarte bastante tiempo.

---

¿Tienes alguna mejora en mente o encontraste un bug? Abre un issue en el repositorio. Las contribuciones son bienvenidas.
