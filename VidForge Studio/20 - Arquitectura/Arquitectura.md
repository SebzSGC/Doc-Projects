---
titulo: "Arquitectura"
tipo: indice
estado: vigente
audiencia: [negocio, tecnico]
responsable: "SebzSGC"
actualizado: "2026-10-18"
etiquetas: [tipo/indice, area/arquitectura]
---

# 🏗️ Arquitectura

> [!abstract] Qué encontrarás aquí
> Cómo están construidos y conectados todos los pisos del ecosistema: la integración de ingeniería inversa **REA** en Windows y los componentes internos de **VidForge Studio** (interfaz, servidor FastAPI, sidecar Node.js y motor de vídeo).

| Nota | En una línea | Audiencia | Estado |
|---|---|---|
| [[Arquitectura general]] | Vista de contexto y contenedores del ecosistema REA + VidForge Studio | 👩‍💼🛠️ | 🟢 vigente |
| [[Integraciones]] | Con qué motores locales, tarjetas gráficas y proveedores opcionales en la nube se conecta | 👩‍💼🛠️ | 🟢 vigente |
| [[Especialista REA y Auditoría Forense]] | Servidor MCP de 207 herramientas y laboratorio forense que dio origen al sistema | 🛠️ | 🟢 vigente |
| [[Servidor Backend y Gestor de Trabajos]] | API REST/SSE en FastAPI, colas por recurso, AudioLab y bóveda de secretos | 🛠️ | 🟢 vigente |
| [[Sidecar de Renderizado y Motor FFmpeg]] | Motor Node.js + Chrome Headless para escenas HTML/WebGL y pipeline FFmpeg NVENC | 🛠️ | 🟢 vigente |
| [[Alineador ASR y Planificador de Timeline]] | Alineación forzada palabra por palabra, subtítulos ASS y armado automático de pistas | 🛠️ | 🟢 vigente |
| [[Interfaz Web Studio y Lanzador Nativo]] | Aplicación React 18 con 5 espacios de trabajo y ventana nativa WebView2 | 👩‍💼🛠️ | 🟢 vigente |

> [!tip]- Vista automática (requiere el plugin Dataview)
> ```dataview
> TABLE tipo, estado, actualizado
> FROM "20 - Arquitectura"
> WHERE file.name != this.file.name
> SORT actualizado DESC
> ```

⬅️ Volver a [[00 - Inicio]]
