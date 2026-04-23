---
layout: post
title: "Cómo construí un proxy inteligente para no volver a ver un error 429 de Gemini"
date: 2026-04-23 10:00:00 +0100
categories: [python, infraestructura, gemini, api]
tags: [python, flask, google-gemini, api-proxy, sqlite, raspberry-pi, automation]
image: /assets/img/posts/geminikeys-banner.png
description: "Un middleware en Flask que rota API Keys y cambia de modelo automáticamente cuando la cuota se agota. Cero cambios en tu código existente."
---

Llevaba semanas trabajando en un proyecto que hacía llamadas intensivas a la API de Google Gemini. El flujo era el mismo una y otra vez: lanzar el script, esperar, ver el temido `429 Too Many Requests`, esperar el cooldown y volver a empezar. Un ciclo frustrante que me robaba horas.

La solución obvia era gestionar múltiples cuentas con múltiples API Keys y rotar entre ellas. La ejecución de esa idea, sin embargo, requería un refactor feo en cada script que tocaba la API. No me gustaba la idea.

Entonces pensé: ¿y si construyo un proxy local que lo gestione por mí?

## La idea central

En lugar de que mi código hable directamente con Google, habla con un servidor local en el puerto `5005`. Ese servidor tiene un pool de API Keys y una lista de modelos de respaldo. Cuando Google devuelve un error, el proxy lo absorbe, cambia de key o modelo, y reintenta. Mi código no se entera de nada.

El cambio en el código es de una línea:

```python
# Antes
url = "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=MI_KEY"

# Después
url = "http://localhost:5005/proxy/v1beta/models/gemini-1.5-flash:generateContent"
```

Eso es todo. Sin cambiar headers, sin cambiar el payload, sin tocar la lógica de negocio.

## La arquitectura

El proyecto tiene tres componentes principales:

**1. La base de datos (SQLite)**

Tres tablas simples: `api_keys` para el pool de llaves, `models` para la lista de fallback, y `logs` para el historial de peticiones. SQLite fue la elección obvia: sin servidor, sin configuración, persistencia local. Para este caso de uso es más que suficiente.

**2. El proxy inteligente (Flask)**

**2. El proxy inteligente (Flask)**

{% raw %}
```python
for modelo_actual in modelos_a_probar:
    endpoint_modificado = endpoint.replace(
        f"models/{modelo_original}:", 
        f"models/{modelo_actual}:"
    )
    for _ in range(2):
        llave = llaves_disponibles[intento % len(llaves_disponibles)]
        google_url = f"[https://generativelanguage.googleapis.com/](https://generativelanguage.googleapis.com/){endpoint_modificado}?key={llave}"
        
        resp = requests.request(method=request.method, url=google_url, ...)
        
        if resp.status_code == 200:
            return Response(resp.content, 200)  # ✅ Éxito
        if resp.status_code == 400:
            return Response(resp.content, 400)  # ❌ Error cliente
        if resp.status_code == 503:
            break  # Pasar al siguiente modelo
{% endraw %}

**3. El panel de control (HTML/CSS)**

Una interfaz web sencilla para gestionar el pool sin tocar la base de datos directamente. Desde aquí puedes añadir y eliminar keys y modelos, ver el historial de las últimas 50 peticiones, e importar/exportar keys en bulk desde un archivo `.txt`.

No usé ningún framework frontend. HTML y CSS puro. Para algo así, añadir React o Vue hubiera sido matar moscas a cañonazos.

## El despliegue

El caso de uso principal es una Raspberry Pi o un servidor con DietPi haciendo de proxy permanente en la red local. Para eso, un servicio Systemd es lo más limpio:

```ini
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
```

Con `systemctl enable --now geminikeys` el proxy arranca solo con el sistema y se reinicia si peta. Sin más.

## Lo que aprendí

**El orden de modelos importa.** Los modelos en la lista de fallback se intentan en orden. Pon los más rápidos y baratos primero (`gemini-1.5-flash` antes que `gemini-1.5-pro`). Si el objetivo es que una tarea salga adelante sin importar el coste, pon los más capaces al final como último recurso.

**La aleatoriedad en el pool de keys es deliberada.** Si siempre empezaras por la primera key, esa key se agotaría antes que las demás. El `random.shuffle()` distribuye la carga de forma uniforme a lo largo del tiempo.

**Separar el `503` del `400` es crítico.** Un `503` significa que Google está ocupado, tiene sentido reintentar con otro modelo. Un `400` significa que tu petición está mal formada, reintentar es inútil y consume cuota. El proxy devuelve los `400` inmediatamente.

**SQLite para logging fue una buena decisión.** La alternativa era logging en fichero, que es más rápido pero hace las consultas de los últimos N registros más engorrosas. Con SQLite, el `ORDER BY id DESC LIMIT 50` funciona perfecto.

## Lo que añadiría en una v2

Hay cosas que dejé fuera conscientemente para mantener el scope manejable, pero que sería interesante explorar:

- **Health check automático de keys**: un endpoint `/healthcheck` que prueba todas las keys registradas y marca como inactivas las que devuelven errores de autenticación.
- **Estadísticas por key**: saber qué key tiene el mayor número de errores para detectar cuáles están agotadas o revocadas.
- **Soporte streaming**: actualmente el proxy no gestiona respuestas streaming (`stream: true`). Para generación de texto larga, esto sería un añadido valioso.
- **Rate limiting configurable**: pausas programadas entre peticiones para evitar que el propio proxy dispare demasiado rápido y provoque los mismos errores que intenta resolver.

## El código

El proyecto completo está en GitHub. Tiene dos ficheros principales (`app.py` y `templates/index.html`), menos de 200 líneas de código en total, y las únicas dependencias son Flask y requests.

```bash
git clone https://github.com/d4nysj/Gemini-API-Key-Rotator-Intelligent-Proxy
cd GeminiKeys
pip install flask requests
python app.py
```

Si trabajas con Gemini de forma intensiva y estás harto de gestionar manualmente las cuotas, espero que te sea útil.

---

*¿Tienes alguna mejora en mente o encontraste un bug? Abre un issue en el repositorio, las contribuciones son bienvenidas.*
