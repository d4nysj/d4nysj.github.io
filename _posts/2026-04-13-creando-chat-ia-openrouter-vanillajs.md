---
title: "Construyendo mi propio cliente de IA 'Privacy-First' con OpenRouter y Vanilla JS"
date: 2026-04-13 12:00:00 +0200
categories: [Proyectos, Desarrollo Web]
tags: [javascript, openrouter, llm, frontend, api]
image:
  path: https://github.com/d4nysj/D4NyosChat/raw/main/img/2.png
  alt: Interfaz de chat de Inteligencia Artificial abstracta
---

## El Problema: Dependencia y Privacidad en el Ecosistema LLM

En el día a día como ingeniero, interactuar con Modelos de Lenguaje Grande (LLMs) es fundamental. Sin embargo, usar las interfaces oficiales de OpenAI (ChatGPT) o Anthropic (Claude) presenta dos inconvenientes graves:

1. **Fragmentación:** Tienes que saltar entre diferentes pestañas si quieres comparar las respuestas de GPT-4o con las de Claude 3.5 Sonnet o Llama 3.
2. **Privacidad (Data Scraping):** Por defecto, muchas de las conversaciones que introduces en las interfaces gratuitas pueden ser utilizadas para entrenar futuros modelos. En entornos de ciberseguridad o desarrollo, pegar código fuente o logs de infraestructura en estas interfaces es un riesgo de OPSEC.

## La Solución: Un Cliente Local 'Zero-Dependency'

Para solucionar esto, decidí construir **D4Nyos Chat**, una interfaz web ligera diseñada para conectar directamente con la API de [OpenRouter](https://openrouter.ai/), un agregador que permite acceso a decenas de modelos desde un solo *endpoint*.

### Principios de Diseño y Arquitectura

Quería que este proyecto fuera un ejercicio de purismo en el desarrollo frontend. Las premisas fueron:

* **Vanilla JS/HTML/CSS:** Nada de React, Vue o pesados `node_modules`. Todo debía caber en un único archivo ejecutable en cualquier navegador moderno.
* **Privacy First (Local Storage):** La aplicación no tiene un backend propio. Las claves de la API de OpenRouter (API Keys) y las preferencias del usuario se guardan **exclusivamente en el `localStorage` del navegador**. Ningún dato viaja a un servidor intermedio controlado por mí.
* **Soporte Multimodal:** Integración de carga de archivos (imágenes y documentos) mediante conversión a Base64 nativa en el navegador, condicionando el envío solo si el modelo seleccionado soporta capacidades de visión.

## Lógica Core: La Llamada a la API

El corazón de la aplicación es la función que orquesta la petición HTTP POST hacia OpenRouter. En lugar de usar SDKs pesados, implementé la llamada usando la API `fetch` nativa:

```javascript
async function sendMessage() {
  // 1. Recolección de inputs (Texto + Adjuntos)
  const content = buildContent(text);
  history.push({ role: 'user', content });

  try {
    // 2. Inyección del System Prompt si está configurado
    const msgs = config.systemPrompt ? [{ role: 'system', content: config.systemPrompt }, ...history] : history;
    
    // 3. Petición POST directa a OpenRouter
    const res = await fetch('[https://openrouter.ai/api/v1/chat/completions](https://openrouter.ai/api/v1/chat/completions)', {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${config.apiKey}`,
        'Content-Type': 'application/json',
        'HTTP-Referer': location.href,
        'X-Title': 'D4Nyos Chat'
      },
      body: JSON.stringify({
        model: config.model,
        messages: msgs,
        stream: false 
      })
    });

    const data = await res.json();
    if (data.error) throw new Error(data.error.message);
    
    // 4. Renderizado de la respuesta
    const reply = data.choices?.[0]?.message?.content || '';
    history.push({ role: 'assistant', content: reply });
    appendBubble('ai', reply);

  } catch (err) {
    showToast('Error al conectar: ' + err.message);
  }
}
```
