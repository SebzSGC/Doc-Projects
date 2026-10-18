---
titulo: "Datos"
tipo: indice
estado: vigente
audiencia: [negocio, tecnico]
responsable: "SebzSGC"
actualizado: "2026-10-18"
etiquetas: [tipo/indice, area/datos]
---

# 📊 Datos

> [!abstract] Qué encontrarás aquí
> Qué información guarda **VidForge Studio** (la base de datos [[Glosario#SQLite WAL|SQLite WAL]], las carpetas de proyecto `.vfproj` y la bóveda cifrada de credenciales) y cuáles son los indicadores técnicos y de negocio que miden su calidad.

| Nota | En una línea | Audiencia | Estado |
|---|---|---|---|
| [[Modelo de datos]] | Diagrama entidad-relación de `vidforge.db`, diccionario de tablas y estructura de carpetas `.vfproj` | 👩‍💼🛠️ | 🟢 vigente |
| [[Indicadores y métricas]] | Métricas de velocidad de producción, precisión de sincronización, cobertura de contratos e integridad | 👩‍💼🛠️ | 🟢 vigente |

> [!tip]- Vista automática (requiere el plugin Dataview)
> ```dataview
> TABLE tipo, estado, actualizado
> FROM "40 - Datos"
> WHERE file.name != this.file.name
> SORT actualizado DESC
> ```

⬅️ Volver a [[00 - Inicio]]
