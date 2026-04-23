Me parece la decisión más inteligente. Es el punto medio perfecto entre la eficiencia del programador y el control del editor. Si no tienes imágenes en el repositorio, un post de Jekyll puede verse un poco "desnudo", así que la parte manual te servirá para darle estilo, mientras que la automática te ahorra el trabajo pesado de copiar el código.

Aquí tienes cómo estructurar ese flujo semiautomático para que el post no quede como un bloque de texto plano:

1. El "Truco" del Raw para el contenido
Para no estar seleccionando texto y pegando, usa la URL raw de tu archivo.

Ve a tu repositorio en GitHub.

Abre el README.md.

Haz clic en el botón "Raw".

Copia esa URL.

En tu terminal (dentro de la carpeta de tu web Jekyll), puedes ejecutar esto para crear el post con el contenido ya volcado:

Bash
echo -e "---\nlayout: post\ntitle: 'Nombre del Proyecto'\n---\n" > _posts/2024-05-20-proyecto.md
curl https://raw.githubusercontent.com/usuario/repo/main/README.md >> _posts/2024-05-20-proyecto.md
2. Solución para la falta de imágenes (Visuales automáticos)
Como no tienes imágenes, el post puede quedar muy denso. Puedes usar Placeholders o Generadores dinámicos que se vean "techie" sin tener que diseñar nada:

GitHub Cards: Como te mencioné antes, una tarjeta dinámica que muestra tus estrellas, lenguaje principal y descripción.

Social Preview de GitHub: GitHub genera automáticamente una imagen para cada repo. Puedes usarla en tu post así:
https://opengraph.githubassets.com/1/USUARIO/REPO
(Esto insertará una imagen profesional con el nombre del repo y tu avatar automáticamente).

3. Estructura recomendada para el post
Para que el proceso sea rápido pero el resultado parezca muy trabajado, usa esta plantilla de Front Matter en tu archivo .md:

Markdown
---
layout: post
title: "Analizando mi proyecto: [Nombre]"
date: 2024-05-20
description: "Una breve descripción de lo que hace el código."
image: https://opengraph.githubassets.com/1/TU_USUARIO/TU_REPO
tags: [github, proyecto, ruby]
---

![Vista previa del repo](https://opengraph.githubassets.com/1/TU_USUARIO/TU_REPO)

# Gemini API Key Rotator & Intelligent Proxy

> **Middleware de infraestructura** para maximizar la disponibilidad de la API de Google Gemini mediante rotación automática de keys y fallback inteligente de modelos.

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat-square&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-2.x-000000?style=flat-square&logo=flask&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-3-003B57?style=flat-square&logo=sqlite&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

---

## ¿Qué problema resuelve?

Cuando trabajas con la API de Google Gemini en proyectos de producción o experimentación intensiva, te topas inevitablemente con errores `429 (Rate Limit)` y `503 (Service Unavailable)`. La solución habitual es esperar. Este proyecto elimina esa espera.

**GeminiKey Rotator** actúa como un proxy local transparente que gestiona un pool de API Keys y una lista de modelos de respaldo. Tu código no cambia. El proxy se encarga del resto.

---

## Características

| Característica | Descripción |
|---|---|
| **Rotación de Keys** | Distribuye peticiones entre múltiples API Keys para evitar el agotamiento de cuota |
| **Smart Model Fallback** | Si `gemini-1.5-pro` falla, reintenta automáticamente con `gemini-1.5-flash` u otro modelo configurado |
| **Panel Web** | Interfaz para añadir/eliminar keys y modelos en tiempo real, sin reiniciar el servicio |
| **Activity Log** | Registro de cada petición: key usada, modelo respondió y código HTTP resultante |
| **SQLite** | Persistencia local de configuración y logs, sin dependencias externas |
| **Drop-in replacement** | Solo cambia la URL base en tu código. Sin modificar headers, auth ni estructura de peticiones |

---

## Stack Tecnológico

- **Backend:** Python 3 + Flask
- **Base de Datos:** SQLite3
- **Frontend:** HTML5 / CSS3
- **Despliegue:** Systemd daemon (Linux / DietPi / Raspberry Pi / Ubuntu)

---

## Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/d4nysj/Gemini-API-Key-Rotator-Intelligent-Proxy
cd Gemini-API-Key-Rotator-Intelligent-Proxy
```

### 2. Instalar dependencias

```bash
pip install flask requests
```

### 3. Ejecutar en modo desarrollo

```bash
python app.py
```

El panel web estará disponible en `http://localhost:5005`.

### 4. Desplegar como servicio (Systemd)

Para que el proxy arranque automáticamente con el sistema:

```bash
sudo nano /etc/systemd/system/geminikeys.service
```

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

```bash
sudo systemctl enable --now geminikeys
```

---

## Uso

Sustituye la URL base de Google por la del proxy. **No necesitas pasar `?key=`**, el proxy la inyecta automáticamente.

**Antes (directo a Google):**
```
https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=TU_KEY
```

**Después (a través del proxy):**
```
http://localhost:5005/proxy/v1beta/models/gemini-1.5-flash:generateContent
```

### Ejemplo en Python

```python
import requests

PROXY_URL = "http://localhost:5005/proxy/v1beta/models/gemini-1.5-flash:generateContent"

payload = {
    "contents": [{"parts": [{"text": "Explica la fusión nuclear en 2 frases."}]}]
}

response = requests.post(PROXY_URL, json=payload)
print(response.json())
```

---

## Flujo de Resiliencia

Cuando una petición llega al proxy, la lógica de decisión es la siguiente:

```
Petición entrante
       |
       v
+-----------------+
| Selecciona Key  |<--- Pool de Keys (aleatoria)
| del pool        |
+--------+--------+
         |
         v
+-----------------+
| Intenta modelo  |<--- Modelo solicitado por el cliente
| original        |
+--------+--------+
         |
    +----+----+
    | 200 OK? |---- SI ---> Devuelve respuesta al cliente [OK]
    +----+----+
         | NO (503/429)
         v
+-----------------+
| Siguiente key   |
| + modelo        |<--- Lista de modelos fallback configurados
| de fallback     |
+--------+--------+
         |
    +----+----+
    | 200 OK? |---- SI ---> Devuelve respuesta al cliente [OK]
    +----+----+
         | NO
         v
  Error crítico 500
```

El cliente **nunca se entera** de los reintentos internos.

---

## Estructura del Proyecto

```
GeminiKeys/
├── app.py              # Backend Flask + lógica del proxy
├── templates/
│   └── index.html      # Panel de control web
├── keys.db             # Base de datos SQLite (auto-generada)
├── demos/
│   └── example.py      # Ejemplo de integración
└── README.md
```

---

## Endpoints de la API interna

| Método | Ruta | Descripción |
|---|---|---|
| `GET` | `/` | Panel de control web |
| `POST` | `/add` | Añade una API Key |
| `POST` | `/delete` | Elimina una API Key |
| `POST` | `/add_model` | Añade un modelo de fallback |
| `POST` | `/delete_model` | Elimina un modelo de fallback |
| `POST` | `/clear_logs` | Limpia el historial de actividad |
| `GET` | `/export` | Exporta todas las keys en `.txt` |
| `POST` | `/import` | Importa keys desde un archivo `.txt` |
| `ANY` | `/proxy/<endpoint>` | **Proxy principal** — redirige a Google |

---

## Tips

- **Importación masiva:** Puedes importar múltiples keys de una vez desde un archivo `.txt` con formato `API_KEY|descripcion` (una por línea).
- **Orden de modelos:** El proxy prueba los modelos en el orden en que aparecen en tu lista de fallback. Pon los más rápidos/baratos primero.
- **Logs:** Los últimos 50 registros se muestran en el panel. Usa "Limpiar Historial" para resetear.

---

## Licencia

Este proyecto está bajo la [Licencia MIT](LICENSE). Úsalo, fórkéalo y mejóralo libremente.

---

## Contribuciones

Las PRs son bienvenidas. Para cambios mayores, abre un issue primero para discutir qué te gustaría cambiar.
