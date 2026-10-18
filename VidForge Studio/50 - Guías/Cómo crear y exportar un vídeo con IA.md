---
titulo: "Cómo crear y exportar un vídeo con IA"
tipo: guia
estado: vigente
audiencia: [negocio, tecnico]
dificultad: "🟢 fácil"
tiempo: "10 min"
responsable: "SebzSGC"
actualizado: "2026-10-18"
etiquetas: [tipo/guia, area/produccion]
---

# 🧭 Cómo crear y exportar un vídeo con IA

> [!abstract] Al terminar tendrás…
> Un vídeo **MP4 a 60 FPS** con locución, subtítulos tipo karaoke, efecto de cámara [[Glosario#Ken Burns|Ken Burns]] y tarjetas animadas, además de una carpeta de proyecto lista para abrir en **[[Glosario#CapCut Desktop|CapCut Desktop]]** y los subtítulos `.srt`/`.ass`.

| 🎚️ Dificultad | ⏱️ Tiempo | 👤 Para |
|---|---|---|
| 🟢 Fácil | 10 min | Creadores de contenido, editores y analistas |

## ✅ Antes de empezar necesitas

- [ ] Tener abierto **VidForge Studio** (ver [[Primeros pasos]]).
- [ ] Un párrafo de guion en español (o usar el guion de demostración que viene precargado).

## 🪜 Pasos

### 1. Crear el proyecto y redactar el guion con `@Personajes`
1. En la barra lateral izquierda, escribe el nombre de tu proyecto (por ejemplo, `Resumen Trimestral`) y haz clic en **+ Nuevo Proyecto**.
2. Ve a la pestaña **1. Director & Prompts**.
3. Escribe tu guion incluyendo cifras y menciones con `@` para los personajes recurrentes, por ejemplo:
   > *"En el último trimestre las entregas puntuales subieron un 94% en toda la región. @Elena presenta la comparativa entre el proceso manual versus la automatización local."*
4. Haz clic en **Segmentar en Beats**.

> [!success] Deberías ver
> El sistema detecta automáticamente a `@Elena` en la lista de personajes y divide tu texto en escenas numeradas (*[[Glosario#Beat|beats]]*), marcando como `motion` aquellas que contienen porcentajes o comparaciones.

### 2. Ejecutar el modo rápido (**Autopilot 1-Click**) o generar activos paso a paso
- **Camino rápido (1 clic):** Pulsa el botón superior **⚡ Autopilot 1-Click**. El sistema ejecutará automáticamente la locución [[Glosario#TTS|TTS]], la [[Glosario#Alineación forzada|alineación forzada]], la creación de imágenes por escena y el armado de la línea de tiempo.
- **Camino manual (si quieres personalizar el audio):**
  1. Ve a **2. Multimedia & AudioLab** y sube tu propia grabación de voz o pulsa **Generar TTS Local**.
  2. En la sección **AudioLab**, aplica **Recortar Silencios (`trim_silence`)** y luego **Normalizar Volumen (`-14 LUFS`)** para dejar la voz con calidad de estudio.
  3. Ve a **3. Sync & Plantillas** y pulsa **Alinear Palabras con Audio**.

> [!success] Deberías ver
> En la pestaña **Sync & Plantillas** aparecerá cada palabra del guion con su tiempo exacto en segundos y podrás descargar los subtítulos en `.srt`, `.vtt` o `.ass`.

### 3. Añadir o personalizar una escena animada HTML/SVG (opcional)
1. En **3. Sync & Plantillas**, elige una plantilla del catálogo (por ejemplo, `stat-highlight` para destacar un número o `comparison-vs` para una comparativa).
2. Completa los campos asegurándote de que la cifra coincida con lo que dice tu locución (ej. `value: "94%"`).
3. Haz clic en **Renderizar Escena (WebM Alfa)**.

> [!success] Deberías ver
> Un nuevo activo de tipo `motion` (`motion_stat-highlight_....webm`) con canal transparente listo en la galería del proyecto.

### 4. Renderizar el vídeo MP4 a 60 FPS o exportar a CapCut Desktop
1. Entra a **4. Editor Studio**. Si hiciste cambios manuales, pulsa **Auto-Planificar Timeline** y luego **Validar Invariantes** (`0 problemas`).
2. Para obtener el vídeo terminado: pulsa **🎬 Renderizar Vídeo MP4 (60 FPS)**.
3. Para llevar el proyecto a edición manual:
   - Pulsa **✂️ Exportar a CapCut Desktop** (asegúrate de tener `CapCut.exe` cerrado).
   - O pulsa **Exportar OpenTimelineIO (`.otio`)** para DaVinci Resolve / Premiere.

> [!success] Deberías ver
> El vídeo `.mp4` aparece en la lista de renders finales y la carpeta `<Nombre>_CapCut/` queda creada dentro de `storage/projects/<proyecto>.vfproj/exports/` con su carpeta `Resources/` y `draft_content.json` validado.

## 🛠️ Si algo falla

| Síntoma | Causa probable | Solución |
|---|---|---|
| Error `422` al renderizar una plantilla animada | Pusiste en la tarjeta un número que no se menciona en el texto hablado | Ajusta el campo `value` para que coincida con la cifra del guion (regla `numbers_must_appear_in_speech`) |
| Error `400: Project has no active transcript` al auto-planificar | Aún no se ha ejecutado la alineación de audio | Pulsa primero **Alinear Palabras** en la pestaña 3 o usa **Autopilot 1-Click** |
| El vídeo MP4 tarda en generarse o falla FFmpeg | Problema con el driver de GPU o con el proceso sidecar | Consulta [[Runbook · Fallo en Sidecar o FFmpeg NVENC]] |

## 🔗 Relacionado

- [[Primeros pasos]] · [[Flujo de Autopilot en un clic]] · [[Flujo de exportación a CapCut y OTIO]] · [[Catálogo de plantillas y contratos JSON]]
