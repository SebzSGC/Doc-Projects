---
titulo: "Referencia API"
tipo: api
estado: vigente
audiencia: [tecnico]
responsable: "SebzSGC"
version_api: "v1"
url_base: "http://127.0.0.1:8765/api/v1"
actualizado: "2026-10-18"
etiquetas: [tipo/api, area/referencia]
---

# 🔌 Referencia API

> [!abstract] En pocas palabras
> Catálogo técnico de las **28 rutas HTTP REST y [[Glosario#SSE|SSE]]** que expone el servidor FastAPI de **VidForge Studio** (`vidforge/apps/backend/src/vidforge/api/app.py`) para crear proyectos, segmentar guiones, procesar audio, alinear subtítulos, renderizar escenas y exportar a [[Glosario#CapCut Desktop|CapCut Desktop]].

**URL base:** `http://127.0.0.1:8765/api/v1` · **Autenticación:** Local en `127.0.0.1` (sin token externo; las claves de terceros se gestionan internamente en la bóveda cifrada).

## 📋 Resumen completo de endpoints (`/api/v1`)

| Dominio | Método | Ruta | Qué hace |
|---|---|---|---|
| **Sistema** | `GET` | `/api/v1/health` | Verifica estado del servicio (`status: "ok"`, `version: "1.0.0"`) |
| **Sistema** | `GET` | `/api/v1/system/hardware` | Devuelve núcleos CPU, RAM, GPU NVIDIA y codificador preferido (`h264_nvenc` / `libx264`) |
| **Sistema** | `POST` | `/api/v1/system/cleanup-temp` | Limpia subcarpetas efímeras dentro de `storage/temp/` |
| **Proyectos** | `GET` | `/api/v1/projects` | Lista todos los proyectos ordenados por fecha de actualización |
| **Proyectos** | `POST` | `/api/v1/projects` | Crea un proyecto nuevo, su carpeta `.vfproj`, `script.json` y `timeline.json` iniciales |
| **Proyectos** | `GET` | `/api/v1/projects/{project_id}` | Obtiene el detalle de un proyecto, su ruta `.vfproj` y total de activos |
| **Proyectos** | `DELETE` | `/api/v1/projects/{project_id}` | Elimina el proyecto de `vidforge.db` y borra su carpeta `.vfproj` |
| **Guion / Director** | `GET` | `/api/v1/projects/{project_id}/script` | Devuelve el `ScriptModel` actual (`text`, `characters`, `beats`) |
| **Guion / Director** | `PUT` | `/api/v1/projects/{project_id}/script` | Guarda cambios manuales sobre el guion y sus [[Glosario#Beat\|beats]] |
| **Guion / Director** | `POST` | `/api/v1/projects/{project_id}/script/segment` | Extrae `@Personajes`, divide el texto en beats y opcionalmente enriquece prompts con LLM |
| **Activos** | `GET` | `/api/v1/projects/{project_id}/assets` | Lista los activos del proyecto (filtrable con `?kind=image\|video\|audio\|motion\|render`) |
| **Activos** | `POST` | `/api/v1/projects/{project_id}/assets/upload` | Sube un archivo multimedia, lo inspecciona con `ffprobe` y genera su miniatura |
| **Activos** | `GET` | `/api/v1/assets/{asset_id}/file` | Sirve el archivo binario original del activo |
| **Activos** | `GET` | `/api/v1/assets/{asset_id}/thumbnail` | Sirve la miniatura `.jpg` generada en `.cache/thumbs/` |
| **Activos** | `DELETE` | `/api/v1/assets/{asset_id}` | Elimina un activo de disco y de la base de datos |
| **Activos** | `POST` | `/api/v1/assets/batch-delete` | Elimina múltiples activos en lote recibiendo `{"asset_ids": [...]}` |
| **Generación IA** | `POST` | `/api/v1/projects/{project_id}/generations` | Encola un trabajo asíncrono de tipo `tts`, `image`, `video` o `batch_beats` |
| **AudioLab** | `POST` | `/api/v1/projects/{project_id}/audio/process` | Encola proceso de [[Glosario#AudioLab\|AudioLab]]: `join`, `trim_silence` o `normalize` (`-14 LUFS`) |
| **Transcripción** | `POST` | `/api/v1/projects/{project_id}/transcripts` | Encola la [[Glosario#Alineación forzada\|alineación forzada]] palabra por palabra sobre el audio maestro |
| **Transcripción** | `GET` | `/api/v1/transcripts/{transcript_id}` | Obtiene el `TranscriptModel` completo con tiempos en microsegundos |
| **Transcripción** | `GET` | `/api/v1/transcripts/{transcript_id}/export` | Descarga los subtítulos en texto plano (`?format=srt\|vtt\|ass`) |
| **Timeline** | `GET` | `/api/v1/projects/{project_id}/timeline` | Obtiene el `TimelineModel` actual del proyecto |
| **Timeline** | `PUT` | `/api/v1/projects/{project_id}/timeline` | Actualiza el `TimelineModel` e incrementa su número de `revision` |
| **Timeline** | `POST` | `/api/v1/projects/{project_id}/timeline/autoplan` | Construye automáticamente la línea de tiempo multipista sincronizada |
| **Timeline** | `POST` | `/api/v1/projects/{project_id}/timeline/validate` | Valida invariantes del timeline (duraciones positivas y cero solapes) |
| **Plantillas y Render** | `GET` | `/api/v1/templates` | Lista las 6 plantillas HTML/SVG disponibles con sus esquemas y reglas semánticas |
| **Plantillas y Render** | `POST` | `/api/v1/projects/{project_id}/motion-scenes/render` | Valida semántica y renderiza una plantilla HTML/SVG en `.webm` transparente vía Sidecar |
| **Plantillas y Render** | `POST` | `/api/v1/projects/{project_id}/renders` | Encola en el pool `gpu` el renderizado completo del timeline a MP4 60 FPS |
| **Exportación** | `POST` | `/api/v1/projects/{project_id}/exports` | Exporta el proyecto a `capcut`, `otio`, `srt` o `ass` |
| **Autopilot** | `POST` | `/api/v1/projects/{project_id}/autopilot` | Ejecuta el pipeline completo en un clic (Segmentar → TTS → Alinear → Imágenes → Timeline) |
| **Trabajos y Eventos** | `GET` | `/api/v1/jobs` | Lista los últimos 100 trabajos (`?project_id=...`), y `GET /POST /api/v1/jobs/{id}[/cancel]` |
| **Trabajos y Eventos** | `GET` | `/api/v1/events` | Stream **[[Glosario#SSE\|SSE]]** en tiempo real de eventos `job.queued`, `job.running`, `job.progress`, `job.succeeded` |
| **Ajustes y Claves** | `GET/PATCH` | `/api/v1/settings` y `/api/v1/providers` | Consulta/actualiza preferencias y gestiona secretos cifrados (`PUT/DELETE /providers/{p}/secret`) |

---

## 💡 Ejemplos de peticiones y respuestas clave

### 1. `POST /api/v1/projects/{project_id}/autopilot` · Pipeline en 1 clic
Dispara de principio a fin la generación de guion, voz, alineación, imágenes y línea de tiempo.

```bash
# Lanza el flujo Autopilot 1-Click para un proyecto existente
curl -X POST http://127.0.0.1:8765/api/v1/projects/8f4e2b10-9a3c-4d7e-b123-7c8e9d0a1b2c/autopilot
```

**Respuesta `200 OK` (trabajo encolado):**
```json
{
  "id": "d4e5f6a7-1122-3344-5566-778899aabbcc",
  "project_id": "8f4e2b10-9a3c-4d7e-b123-7c8e9d0a1b2c",
  "type": "autoplan",
  "state": "queued",
  "progress": 0.0,
  "stage": "En cola"
}
```

### 2. `POST /api/v1/projects/{project_id}/motion-scenes/render` · Escena animada HTML/SVG
Valida las reglas semánticas contra el texto hablado y renderiza un overlay `.webm` con canal alfa.

| Campo | Tipo | Obligatorio | Descripción | Ejemplo |
|---|---|---|---|---|
| `template_id` | texto | ✅ | Identificador de la plantilla (`packages/templates/`) | `"stat-highlight"` |
| `props` | objeto | ✅ | Propiedades según el `manifest.json` de la plantilla | `{"value": "87%", "label": "Crecimiento"}` |
| `duration_us` | entero | ❌ | Duración en microsegundos (por defecto `4000000`) | `4000000` |
| `spoken_text` | texto | ❌ | Frase hablada para validar reglas semánticas | `"El rendimiento creció un 87% este año"` |
| `alpha` | booleano | ❌ | Si es `true`, exporta WebM VP9 con transparencia | `true` |

### 3. `PUT /api/v1/providers/{provider}/secret` · Guardar credencial en bóveda
Guarda una clave en `secrets.v1.bin` cifrada con [[Glosario#DPAPI|DPAPI]] + [[Glosario#AES-256-GCM|AES-256-GCM]].

```bash
# Registra de forma segura una credencial sin exponerla en texto plano
curl -X PUT http://127.0.0.1:8765/api/v1/providers/openai/secret \
  -H "Content-Type: application/json" \
  -d '{"secret": "<SECRETO: ver gestor de claves>"}'
```

## 🛠️ Códigos de error habituales

| Código HTTP | Cuándo ocurre | Qué hacer |
|---|---|---|
| `400 Bad Request` | Se intenta alinear o exportar subtítulos sin tener audio o transcripción activa | Generar o subir locución primero, o usar **Autopilot 1-Click** |
| `404 Not Found` | El `project_id`, `asset_id`, `transcript_id` o `template_id` no existe | Verificar el ID consultando `GET /api/v1/projects` o `GET /api/v1/templates` |
| `422 Unprocessable Entity` | Falló la validación semántica de una escena animada (`validate_motion_semantics`) o un campo Pydantic | Ajustar las propiedades (`props`) para que las cifras coincidan con el guion hablado |

## 🔗 Relacionado

- [[Servidor Backend y Gestor de Trabajos]] · [[Catálogo de plantillas y contratos JSON]] · [[Flujo de Autopilot en un clic]]
