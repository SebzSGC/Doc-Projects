---
titulo: "Operación"
tipo: indice
estado: vigente
audiencia: [negocio, tecnico]
responsable: "SebzSGC"
actualizado: "2026-10-18"
etiquetas: [tipo/indice, area/operacion]
---

# 🚑 Operación

> [!abstract] Qué encontrarás aquí
> Todo lo necesario para empaquetar, desplegar, monitorear y solucionar fallos en **VidForge Studio** y en el servidor **MCP de REA** sobre Windows 11.

| Nota | En una línea | Audiencia | Estado |
|---|---|---|---|
| [[Despliegue e instalador]] | Cómo empaquetar VidForge Studio con Inno Setup 6 y verificar el manifiesto SHA-256 | 🛠️ | 🟢 vigente |
| [[Monitoreo y diagnóstico]] | Chequeos de salud (`/api/v1/health`, `rea doctor`), limpieza de temporales y ubicación de logs | 👩‍💼🛠️ | 🟢 vigente |
| [[Runbook · Fallo en Sidecar o FFmpeg NVENC]] | Qué hacer si falla el renderizado de vídeo MP4, las plantillas HTML/SVG o el codificador GPU | 🛠️ | 🟢 vigente |
| [[Runbook · Herramientas MCP de REA no disponibles]] | Qué hacer si el servidor MCP de REA no carga en Google Antigravity o falla PowerShell | 🛠️ | 🟢 vigente |

> [!tip]- Vista automática (requiere el plugin Dataview)
> ```dataview
> TABLE tipo, estado, actualizado
> FROM "60 - Operación"
> WHERE file.name != this.file.name
> SORT actualizado DESC
> ```

⬅️ Volver a [[00 - Inicio]]
