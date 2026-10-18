---
titulo: "Glosario"
tipo: glosario
estado: vigente
audiencia: [negocio, tecnico]
responsable: "SebzSGC"
actualizado: "2026-10-18"
etiquetas: [tipo/glosario, area/vision]
---

# 📖 Glosario

> [!abstract] En pocas palabras
> Todas las palabras técnicas y siglas de **VidForge Studio** y del entorno **REA** explicadas en lenguaje cotidiano, con una analogía sencilla y cómo se usan dentro de este proyecto.

## A

### AES-256-GCM
**Qué es:** Un método de cifrado bancario y militar que no solo oculta el contenido de un archivo con una llave de 256 bits, sino que además detecta si alguien intentó alterar un solo byte.
**Analogía:** Como guardar tus llaves en una caja fuerte de acero con un sello de lacre que se rompe si alguien intenta forzarla.
**En este proyecto:** El módulo `infra/secrets.py` cifra todas las claves de API de proveedores externos en `storage/userdata/secrets.v1.bin` usando `AESGCM` con el contexto autenticado `vidforge.secrets.v1`.
**Ver:** [[Servidor Backend y Gestor de Trabajos]], [[Modelo de datos]]

### Alineación forzada
**Qué es:** Técnica que compara el texto exacto de un guion con un archivo de voz grabada para descubrir en qué microsegundo empieza y termina cada palabra.
**Analogía:** Como seguir con el dedo la letra impresa de una canción mientras escuchas al cantante para anotar el segundo exacto de cada sílaba.
**En este proyecto:** El módulo `services/asr.py` combina programación dinámica (*Needleman-Wunsch*), similitud difusa (`RapidFuzz`) y detección de silencios para asignar `start_us` y `end_us` a cada palabra del guion.
**Ver:** [[Alineador ASR y Planificador de Timeline]], [[Flujo de Autopilot en un clic]]

### ASS
**Qué es:** Formato avanzado de subtítulos (*Advanced SubStation Alpha*) que permite controlar tipografía, colores, bordes, posición y efecto karaoke palabra por palabra.
**Analogía:** Mientras que un subtítulo básico (`.srt`) es como una nota en bloc de texto plano, un archivo `.ass` es como un cartel luminoso de karaoke que ilumina cada palabra al ritmo de la voz.
**En este proyecto:** VidForge genera subtítulos `.ass` con etiquetas `\kf` de karaoke (`highlight_color: #38BDF8`) y los incrusta en el vídeo final o los exporta como archivo independiente.
**Ver:** [[Alineador ASR y Planificador de Timeline]], [[Flujo de renderizado y escenas animadas]]

### AudioLab
**Qué es:** El taller de procesamiento de sonido integrado en VidForge Studio para unir pistas, recortar silencios muertos y nivelar el volumen.
**Analogía:** Como un ingeniero de sonido que limpia las pausas incómodas de una grabación y ajusta el volumen para que nunca suene ni muy bajo ni saturado.
**En este proyecto:** Expuesto en `POST /api/v1/projects/{id}/audio/process`, ejecuta las operaciones `join` (unión a 48 kHz estéreo), `trim_silence` (`silenceremove` a `-38 dB`) y `normalize` (`loudnorm` a `-14 LUFS`).
**Ver:** [[Servidor Backend y Gestor de Trabajos]], [[Cómo crear y exportar un vídeo con IA]]

## B

### Beat
**Qué es:** Cada oración o unidad narrativa en la que se divide un guion para asignarle su propia imagen, clip de vídeo o gráfica animada.
**Analogía:** Como cada viñeta de un cómic o *storyboard*: una frase de la historia acompañada de un dibujo que la representa.
**En este proyecto:** El servicio `director.py` divide el guion en objetos `BeatModel` (`beat_001_...`), extrae los personajes `@Tag` presentes en esa frase y redacta su prompt visual.
**Ver:** [[Modelo de datos]], [[Flujo de Autopilot en un clic]]

## C

### CapCut Desktop
**Qué es:** Editor de vídeo de escritorio muy popular entre creadores de contenido que guarda sus proyectos en carpetas locales con archivos JSON (`draft_content.json` y `draft_meta_info.json`).
**Analogía:** Es la mesa de montaje final donde el editor puede dar retoques manuales al vídeo si no quiere quedarse solo con la versión automática.
**En este proyecto:** El módulo `services/export.py` genera un borrador completo de CapCut con su carpeta `Resources/`, validado contra `capcut-draft-subset.v1.json` y con verificación previa de que `CapCut.exe` no esté abierto.
**Ver:** [[Flujo de exportación a CapCut y OTIO]]

### Clean-Room
**Qué es:** Metodología de ingeniería de software donde primero se documenta qué hace un sistema existente (sus entradas, salidas y reglas) y luego se construye una implementación 100 % nueva desde cero, sin copiar ni reutilizar código original.
**Analogía:** Como probar un plato en un restaurante, anotar qué ingredientes lleva y luego cocinar tu propia receta desde cero en tu cocina con utensilios nuevos.
**En este proyecto:** VidForge Studio (`d:\rea-projects\vidforge`) es la reconstrucción *Clean-Room* en FastAPI, Node.js y React 18 realizada tras auditar `Creator-IA 2.0_a5`.
**Ver:** [[Resumen ejecutivo]], [[ADR-001 Reconstrucción Clean-Room con FastAPI y Sidecar ESM]]

## D

### DPAPI
**Qué es:** Interfaz de protección de datos nativa de Windows (*Data Protection API*) que cifra secretos vinculándolos a la cuenta del usuario de Windows actual.
**Analogía:** Una cerradura biométrica del propio sistema operativo que solo se abre cuando tú has iniciado sesión en tu computador.
**En este proyecto:** `SecretStore` utiliza `win32crypt.CryptProtectData` para proteger la llave maestra (`secrets.v1.key`) con la que se abre la bóveda de credenciales.
**Ver:** [[Servidor Backend y Gestor de Trabajos]], [[Modelo de datos]]

## G

### Ghidra
**Qué es:** Suite gratuita de ingeniería inversa creada por la NSA que permite descompilar ejecutables nativos (C/C++, PE x86/x64) para entender su lógica interna.
**Analogía:** Un aparato de rayos X industrial que permite ver los planos mecánicos de un motor sellado.
**En este proyecto:** Es el motor opcional de REA para inspeccionar binarios nativos en Windows (`GHIDRA_INSTALL_DIR`); en Windows opera en modo seguro de solo lectura.
**Ver:** [[Especialista REA y Auditoría Forense]], [[Cómo auditar binarios con REA en Windows]]

## I

### Inno Setup
**Qué es:** Sistema de empaquetado para Windows que comprime todos los archivos de un programa en un instalador ejecutable (`Setup.exe`).
**Analogía:** La caja de mudanza etiquetada que descomprime y coloca cada mueble en su habitación correcta cuando llegas a una casa nueva.
**En este proyecto:** Se utiliza en dos frentes: REA desempaquetó estáticamente los `41.201` flujos LZMA2 del instalador de `Creator-IA 2.0_a5`, y VidForge incluye su propio script `installer/vidforge.iss` para generar `VidForge-Studio-1.0.0-Setup.exe`.
**Ver:** [[Despliegue e instalador]], [[Flujo de ingeniería inversa con REA]]

## J

### Job Object
**Qué es:** Mecanismo del núcleo de Windows que agrupa varios procesos hijos bajo un mismo "contrato" para que, si la aplicación principal se cierra, ningún proceso hijo quede colgado consumiendo memoria o tarjeta gráfica.
**Analogía:** Como el interruptor general de un taller: al bajarlo al final del día, se apagan automáticamente todas las máquinas conectadas.
**En este proyecto:** `infra/procs.py` crea un Job Object de Windows con la bandera `JOB_OBJECT_LIMIT_KILL_ON_JOB_CLOSE` y ejecuta FFmpeg y Node con `CREATE_NO_WINDOW` (`0x08000000`) para que nunca parpadeen ventanas negras de consola.
**Ver:** [[Sidecar de Renderizado y Motor FFmpeg]]

## K

### Ken Burns
**Qué es:** Efecto cinematográfico que aplica un paneo y acercamiento (*zoom*) suave y continuo sobre una fotografía fija para darle sensación de movimiento.
**Analogía:** Mover lentamente una lupa sobre una fotografía impresa para que parezca una toma de cámara en movimiento.
**En este proyecto:** `services/render.py` sobreescala cada imagen `1.5x` antes de aplicar el filtro `zoompan` de FFmpeg (`1.03x` a `1.10x`), eliminando el temblor de redondeo (*jitter*) a 60 FPS.
**Ver:** [[Sidecar de Renderizado y Motor FFmpeg]], [[Flujo de renderizado y escenas animadas]]

## L

### LUFS
**Qué es:** Unidad estándar internacional (*Loudness Units relative to Full Scale*) que mide el volumen real tal como lo percibe el oído humano.
**Analogía:** En vez de medir solo los picos de una onda eléctrica, mide qué tan fuerte siente una persona la voz al escucharla en YouTube, TikTok o Spotify.
**En este proyecto:** [[Glosario#AudioLab|AudioLab]] normaliza las locuciones a un objetivo predeterminado de **-14.0 LUFS** con pico verdadero máximo de `-1.0 dBTP` mediante el filtro `loudnorm` de FFmpeg.
**Ver:** [[Servidor Backend y Gestor de Trabajos]]

## M

### MCP
**Qué es:** *Model Context Protocol*, un estándar abierto que conecta asistentes de inteligencia artificial (como Google Antigravity) con herramientas especializadas locales mediante mensajes estructurados JSON-RPC.
**Analogía:** Un puerto USB universal que permite enchufar cualquier instrumento de laboratorio al asistente de IA sin tener que reprogramarlo.
**En este proyecto:** El servidor `rea` se conecta con Antigravity vía `stdio` usando `cmd.exe /d /c npx -y rea-agents@6.5.0 mcp`, exponiendo 207 herramientas de ingeniería inversa.
**Ver:** [[Especialista REA y Auditoría Forense]], [[ADR-003 Integración MCP de REA en Windows mediante wrappers cmd]]

## N

### Nuitka
**Qué es:** Compilador que traduce código escrito en Python a código C nativo y genera librerías dinámicas de Windows (`.pyd`), ocultando el código fuente original.
**Analogía:** Traducir un libro de recetas manuscrito a un molde metálico industrial: puedes fabricar el producto, pero ya no ves el texto original a simple vista.
**En este proyecto:** `Creator-IA 2.0_a5` tenía su backend dividido en 15 módulos `.cp313-win_amd64.pyd` compilados con Nuitka; durante la auditoría forense se extrajeron sus tablas de constantes para reconstruir sus 193 rutas y contratos en VidForge Studio.
**Ver:** [[Especialista REA y Auditoría Forense]], [[Flujo de ingeniería inversa con REA]]

### NVENC
**Qué es:** Chip físico dedicado dentro de las tarjetas gráficas NVIDIA que comprime vídeo H.264/HEVC a gran velocidad sin sobrecargar el procesador principal (CPU).
**Analogía:** Tener una autopista exclusiva de alta velocidad solo para camiones de vídeo, dejando libres las calles normales de la ciudad (la CPU).
**En este proyecto:** `infra/hw.py` prueba al arrancar si el codificador `h264_nvenc` funciona en el equipo; si está disponible, lo usa con preset `p3`/`p4` y calidad constante `cq=20`, cayendo limpiamente a `libx264` si no hay GPU NVIDIA.
**Ver:** [[Sidecar de Renderizado y Motor FFmpeg]], [[Runbook · Fallo en Sidecar o FFmpeg NVENC]]

## O

### OTIO
**Qué es:** *OpenTimelineIO*, formato abierto creado por Pixar para intercambiar líneas de tiempo de edición de vídeo (pistas, cortes, duraciones y transiciones) entre programas profesionales como DaVinci Resolve, Premiere o Nuke.
**Analogía:** El formato PDF o Excel universal pero para líneas de tiempo de cine y televisión.
**En este proyecto:** `services/export.py` exporta cualquier proyecto de VidForge a un archivo `.otio` (`Timeline.1`) mediante `POST /api/v1/projects/{id}/exports` con `target: "otio"`.
**Ver:** [[Flujo de exportación a CapCut y OTIO]]

## R

### REA
**Qué es:** *Reverse Engineer Anything* (`rea-agents@6.5.0`), plataforma de ingeniería inversa asistida por IA que inspecciona ejecutables Windows PE, ensamblados .NET, aplicaciones Electron/JavaScript y contenedores.
**Analogía:** Un laboratorio forense automatizado que examina programas compilados con lupa y entrega informes con pruebas verificables.
**En este proyecto:** Está instalado y registrado en Google Antigravity sobre Windows 11 con su skill oficial v42 (207 herramientas) y la skill local `rea-windows-specialist`.
**Ver:** [[Especialista REA y Auditoría Forense]], [[Cómo auditar binarios con REA en Windows]]

## S

### Sidecar
**Qué es:** Proceso auxiliar especializado que corre al lado del servidor principal para encargarse de una tarea específica (en este caso, renderizar gráficos web y shaders 3D en un navegador oculto).
**Analogía:** El sidecar de una motocicleta: viaja acoplado a la moto principal y lleva equipamiento especializado sin estorbar al conductor.
**En este proyecto:** `apps/render-sidecar/src/sidecar.mjs` es un motor en Node.js 24 que controla Chrome Headless mediante `puppeteer-core`, captura cada fotograma de las plantillas HTML/SVG o transiciones WebGL y los envía por tubería directa a FFmpeg.
**Ver:** [[Sidecar de Renderizado y Motor FFmpeg]], [[Flujo de renderizado y escenas animadas]]

### SQLite WAL
**Qué es:** Base de datos local en un solo archivo (`vidforge.db`) configurada en modo *Write-Ahead Logging* (`WAL`), lo que permite leer datos al mismo tiempo que otro hilo está escribiendo sin bloqueos.
**Analogía:** Un cuaderno de contabilidad donde las nuevas anotaciones se apuntan primero en una hoja rápida adjunta para que nadie tenga que esperar mientras consulta el libro principal.
**En este proyecto:** `infra/db.py` almacena el índice de proyectos, activos, transcripciones, cola de trabajos y ajustes en `storage/userdata/vidforge.db` con `PRAGMA journal_mode=WAL;`.
**Ver:** [[Modelo de datos]], [[ADR-002 Almacenamiento híbrido SQLite WAL y carpetas vfproj]]

### SSE
**Qué es:** *Server-Sent Events*, tecnología web que mantiene una línea abierta del servidor hacia el navegador para enviar avisos en tiempo real (como el porcentaje de avance de un renderizado).
**Analogía:** Una pantalla de aeropuerto que se actualiza sola apenas cambia el estado de un vuelo, sin que tengas que preguntar en recepción cada 5 segundos.
**En este proyecto:** El endpoint `GET /api/v1/events` emite eventos en vivo (`job.queued`, `job.running`, `job.progress`, `job.succeeded`, `job.failed`) desde el `EventBroker` hacia la interfaz React.
**Ver:** [[Servidor Backend y Gestor de Trabajos]], [[Referencia API]]

## T

### TTS
**Qué es:** *Text-to-Speech* (síntesis de voz a partir de texto), tecnología que convierte un guion escrito en una locución hablada en formato de audio.
**Analogía:** Un locutor virtual que lee en voz alta el guion que escribiste y te entrega la grabación lista.
**En este proyecto:** `services/audio.py` utiliza el sintetizador nativo `System.Speech.Synthesis.SpeechSynthesizer` de Windows (100 % offline) y cuenta con un generador acústico determinista de respaldo a 24 kHz PCM-16.
**Ver:** [[Servidor Backend y Gestor de Trabajos]], [[Flujo de Autopilot en un clic]]

## V

### vfproj
**Qué es:** Carpeta autocontenida de proyecto de VidForge Studio (`<nombre>_<id>.vfproj`) que guarda el archivo `project.json`, el guion `script.json`, la línea de tiempo `timeline.json` y todas sus subcarpetas de medios (`assets/`, `renders/`, `exports/`).
**Analogía:** Un maletín de producción donde van juntos el guion, las cintas de audio, las fotos y la película terminada de un mismo proyecto.
**En este proyecto:** Se crea automáticamente dentro de `storage/projects/` al invocar `POST /api/v1/projects`.
**Ver:** [[Modelo de datos]], [[ADR-002 Almacenamiento híbrido SQLite WAL y carpetas vfproj]]

## W

### WebGL
**Qué es:** Tecnología estándar que permite dibujar gráficos y efectos visuales acelerados por tarjeta gráfica directamente dentro de un lienzo HTML5 (`<canvas>`) usando pequeños programas matemáticos llamados *shaders* GLSL.
**Analogía:** Un estudio de efectos especiales en miniatura dentro del navegador web que deforma, gira o mezcla dos imágenes a gran velocidad.
**En este proyecto:** `sidecar.mjs` incluye 4 shaders GLSL limpios (`crosswarp`, `directionalwarp`, `swirl` y `ripple`) para generar transiciones fluidas entre escenas.
**Ver:** [[Sidecar de Renderizado y Motor FFmpeg]], [[Catálogo de plantillas y contratos JSON]]

### WebView2
**Qué es:** Componente nativo de Microsoft Windows (basado en Edge Chromium) que permite mostrar una aplicación web moderna como una ventana de escritorio propia, sin barra de direcciones ni pestañas de navegador.
**Analogía:** Ponerle un marco de cuadro elegante a una pantalla interactiva para que se sienta como un programa nativo de Windows.
**En este proyecto:** `apps/launcher/main.py` utiliza `pywebview` con el motor `edgechromium` y expone el puente nativo `window.pywebview.api` para abrir selectores de carpetas y mostrar archivos en el Explorador de Windows.
**Ver:** [[Interfaz Web Studio y Lanzador Nativo]]

## X

### xfade
**Qué es:** Filtro nativo de FFmpeg que encadena dos clips de vídeo aplicando una transición suave entre el final del primero y el principio del segundo.
**Analogía:** Cuando un DJ mezcla dos canciones superponiendo los últimos compases de una con los primeros de la siguiente para que la música nunca se detenga.
**En este proyecto:** `compute_xfade_schedule` en `services/render.py` extiende matemáticamente cada clip según la duración de su transición (`d_i = s_i + t_{i+1}`) para que la duración total del vídeo nunca se acorte ni pierda sincronía con la voz.
**Ver:** [[Sidecar de Renderizado y Motor FFmpeg]], [[Flujo de renderizado y escenas animadas]]

## 🔤 Siglas rápidas

| Sigla | Significa | En una frase |
|---|---|---|
| **ADR** | *Architecture Decision Record* | Documento breve que explica por qué se tomó una decisión técnica importante |
| **ASR** | *Automatic Speech Recognition* | Reconocimiento automático del habla y alineación temporal de palabras |
| **AST** | *Abstract Syntax Tree* | Árbol sintáctico que representa la estructura del código fuente para analizarlo |
| **CDP** | *Chrome DevTools Protocol* | Protocolo interno para controlar Chromium y capturar fotogramas con fondo transparente |
| **CIL** | *Common Intermediate Language* | Código intermedio de los programas .NET que REA analiza sin ejecutarlos |
| **DPAPI** | *Data Protection API* | Sistema de cifrado de credenciales integrado en Windows |
| **FPS** | *Frames Per Second* | Fotogramas por segundo (VidForge trabaja por defecto a 60 FPS) |
| **LUFS** | *Loudness Units Full Scale* | Medida estándar de volumen percibido por el oído humano |
| **MCP** | *Model Context Protocol* | Protocolo que conecta Google Antigravity con las 207 herramientas de REA |
| **NVENC** | *NVIDIA Encoder* | Codificador de vídeo por hardware en tarjetas gráficas NVIDIA |
| **OTIO** | *OpenTimelineIO* | Estándar abierto de Pixar para intercambiar líneas de tiempo entre editores |
| **PE / PE32+** | *Portable Executable* | Formato estándar de los archivos `.exe`, `.dll` y `.pyd` en Windows |
| **REA** | *Reverse Engineer Anything* | Suite de ingeniería inversa asistida por IA (`rea-agents@6.5.0`) |
| **SSE** | *Server-Sent Events* | Canal de eventos en tiempo real desde el servidor FastAPI hacia la interfaz web |
| **TTS** | *Text-to-Speech* | Conversión automática de un texto escrito en locución de voz |
| **WAL** | *Write-Ahead Logging* | Modo de alta concurrencia de SQLite que permite leer y escribir sin bloqueos |
