---
titulo: "Decisiones"
tipo: indice
estado: vigente
audiencia: [negocio, tecnico]
responsable: "SebzSGC"
actualizado: "2026-10-18"
etiquetas: [tipo/indice, area/decisiones]
---

# ⚖️ Decisiones

> [!abstract] Qué encontrarás aquí
> El registro de decisiones de arquitectura ([[Glosario#ADR|ADR]]) que explica **por qué** se diseñó **VidForge Studio** y la integración de **REA** de esta manera, qué otras alternativas se evaluaron y qué ventajas o compromisos implican.

| Nota | En una línea | Audiencia | Estado |
|---|---|---|---|
| [[ADR-001 Reconstrucción Clean-Room con FastAPI y Sidecar ESM]] | Por qué se reemplazó el monolito Flask/Nuitka por FastAPI + Sidecar Node.js con contratos JSON Schema | 👩‍💼🛠️ | 🟢 vigente |
| [[ADR-002 Almacenamiento híbrido SQLite WAL y carpetas vfproj]] | Por qué se combinó un índice SQLite WAL con carpetas autocontenidas `.vfproj` por proyecto | 🛠️ | 🟢 vigente |
| [[ADR-003 Integración MCP de REA en Windows mediante wrappers cmd]] | Por qué se fijó `rea-agents@6.5.0` bajo `cmd.exe /d /c npx` en Windows 11 | 🛠️ | 🟢 vigente |

> [!tip]- Vista automática (requiere el plugin Dataview)
> ```dataview
> TABLE tipo, estado, actualizado
> FROM "70 - Decisiones"
> WHERE file.name != this.file.name
> SORT actualizado DESC
> ```

⬅️ Volver a [[00 - Inicio]]
