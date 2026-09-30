[README_9.md](https://github.com/user-attachments/files/32836673/README_9.md)
# 🌿 Bot de Telegram para Diagnóstico de Plantas con IA

Bot de Telegram, [**@BIODIVERSIDAdd_bot**](https://t.me/BIODIVERSIDAdd_bot), automatizado con **Make** que recibe fotos de plantas, las analiza con Inteligencia Artificial y responde con un diagnóstico amigable sobre su **salud, riego y cuidados**. Si el usuario escribe sin mandar foto, el bot le recuerda amablemente que necesita una imagen.

## 🎥 Video de demostración

[![Ver demostración en YouTube](https://img.youtube.com/vi/2DF2Kxf9Afs/0.jpg)](https://www.youtube.com/shorts/2DF2Kxf9Afs)

▶️ **Ver en YouTube:** https://www.youtube.com/shorts/2DF2Kxf9Afs

## 🎯 Objetivo

Construir un bot inteligente capaz de:

1. Recibir fotos de plantas enviadas por los usuarios.
2. Analizarlas con Inteligencia Artificial.
3. Responder con un diagnóstico sobre salud, riego y cuidados.
4. Pedir la foto cuando el usuario solo envía texto.

## 🛠️ Tecnologías

| Herramienta | Uso |
|-------------|-----|
| [Telegram Bot API](https://core.telegram.org/bots/api) | Canal de comunicación con el usuario |
| [Make](https://www.make.com) | Automatización del flujo (escenario) |
| Make AI Agents + Make's AI Provider | Análisis de la imagen y generación del diagnóstico |

## 🧩 Arquitectura del escenario

![Escenario en Make: arquitectura del flujo](imagenes/escenario_make.png)

| # | Módulo (Make) | Función |
|---|---------------|---------|
| 1 | Telegram Bot – Watch Updates | Webhook que recibe cada mensaje del bot |
| 2 | Router | Bifurca según el mensaje tenga foto o no |
| 3 | Telegram Bot – Download a File | Descarga la foto (ruta "Tiene foto") |
| 4 | Make AI Agents – Run an agent | Analiza la planta con Make's AI Provider |
| 5 | Telegram Bot – Send a Reply | Envía el diagnóstico al usuario |
| 6 | Telegram Bot – Send a Reply | Envía el recordatorio (ruta "No tiene foto") |

### Filtros del Router

| Ruta | Condición |
|------|-----------|
| Tiene foto | `{{1.message.photo}}` existe |
| No tiene foto | `{{1.message.chat.id}}` existe **y** `{{1.message.photo}}` no existe |

Los dos filtros son excluyentes: cada mensaje toma una sola ruta.

## 🤖 Agente de IA

- **Proveedor:** Make's AI Provider (modelo `small`).
- **Entrada:** la foto descargada y el comentario opcional del usuario (caption).
- **Salida:** diagnóstico en español, máximo 150 palabras, con planta identificada, estado de salud, riego, luz y recomendaciones.
- El prompt completo está en [`codigo/prompt_agente_ia.md`](codigo/prompt_agente_ia.md).

**Ejemplo del formato de respuesta esperado:**

```
🌿 Planta: ...
🩺 Estado de salud: ...
💧 Riego: ...
☀️ Luz y ubicación: ...
✅ Recomendaciones: ...
```

## 📊 Resultados

Pruebas reales registradas en el historial de ejecuciones de Make:

| # | Prueba | Ruta tomada | Operaciones | Duración | Estado |
|---|--------|-------------|-------------|----------|--------|
| 1 | Mensaje de texto sin foto | No tiene foto → Recordatorio | 2 | 0.56 s | ✅ Éxito |
| 2 | Mensaje con foto de una planta | Tiene foto → Descarga → IA → Diagnóstico | 4 | 3.94 s | ✅ Éxito |

**Conclusiones:**

- ✅ El Router separó correctamente los mensajes con y sin foto.
- ✅ Sin foto, el bot responde en menos de 1 segundo pidiendo la imagen.
- ✅ Con foto, el flujo completo (descarga, análisis con IA y respuesta) tarda menos de 4 segundos.
- ✅ Ambas ejecuciones terminaron sin errores.
- 💡 Cada análisis con foto consume 4 operaciones de Make; el recordatorio consume 2.

## 👤 Autor

Julio César García – TecNM Campus Mazatlán
