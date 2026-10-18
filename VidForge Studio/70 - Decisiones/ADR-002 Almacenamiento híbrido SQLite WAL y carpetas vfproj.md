---
titulo: "ADR-002 Almacenamiento híbrido SQLite WAL y carpetas vfproj"
tipo: decision
estado: vigente
audiencia: [tecnico]
responsable: "SebzSGC"
actualizado: "2026-10-18"
fecha: "2026-10-16"
decisores: [SebzSGC]
reemplaza: "ninguno"
etiquetas: [tipo/decision, area/datos]
---

# ⚖️ ADR-002 · Almacenamiento híbrido SQLite WAL y carpetas vfproj

> [!abstract] En pocas palabras
> Decidimos **combinar una base de datos [[Glosario#SQLite WAL|SQLite en modo WAL]] (`vidforge.db`) con carpetas independientes [[Glosario#vfproj|`.vfproj`]] en disco** porque **permite listar y filtrar proyectos, activos y trabajos en milisegundos sin perder la comodidad de abrir o copiar cada proyecto como una carpeta normal de Windows**, aceptando que **debemos mantener sincronizados `project.json` y la tabla `projects` al guardar cambios**.

## 🧩 Contexto

Una aplicación de producción de vídeo maneja dos tipos de datos muy distintos:
1. **Metadatos que se consultan constantemente:** lista de proyectos recientes, cola de trabajos en curso (`jobs`), catálogo de activos por tipo (`image`, `audio`, `motion`, `render`) y ajustes globales.
2. **Archivos pesados y documentos de proyecto:** pistas de audio `.wav`, imágenes `.png`, vídeos `.mp4`, el guion `script.json` y la línea de tiempo `timeline.json`.

Si se guarda todo únicamente en archivos sueltos, listar activos y estados de trabajos se vuelve lento y propenso a condiciones de carrera cuando varios hilos escriben a la vez. Si se encierra todo dentro de una base de datos binaria, el usuario no puede inspeccionar ni respaldar fácilmente la carpeta de un proyecto.

## 🔀 Opciones evaluadas

| Criterio | Opción A: Híbrido SQLite WAL + carpetas `.vfproj` ✅ | Opción B: Solo archivos JSON sueltos en disco | Opción C: Servidor PostgreSQL / MySQL externo |
|---|---|---|---|
| **Instalación y portabilidad** | Inmediata (incluido en Python estándar, cero servidores externos) | Inmediata | Compleja (requiere instalar y administrar un motor pesado) |
| **Concurrencia lectura/escritura** | Alta (`PRAGMA journal_mode=WAL` + `threading.RLock`) | Baja (riesgo de archivos truncados si dos hilos escriben) | Alta |
| **Facilidad para respaldar un proyecto** | Alta (cada `.vfproj` tiene su `project.json`, `script.json`, `timeline.json` y `assets/`) | Alta | Baja (requiere `pg_dump` + exportar binarios) |

## ✅ Decisión

Implementar en `vidforge/apps/backend/src/vidforge/infra/db.py` e `infra/paths.py` un modelo dual:
1. **Índice relacional en `storage/userdata/vidforge.db`:** configurado con `PRAGMA journal_mode=WAL;` y `PRAGMA synchronous=NORMAL;`, con las tablas `projects`, `assets`, `transcripts`, `jobs` y `settings`.
2. **Paquete de proyecto en `storage/projects/<slug>_<id>.vfproj/`:** cada proyecto conserva en texto claro sus documentos `project.json`, `script.json`, `timeline.json`, `transcripts/*.json` y sus subcarpetas `assets/`, `renders/` y `exports/`.

> [!example] Ejemplo de escritura sincronizada en `api/app.py`
> ```python
> def _save_project_files(proj_data: dict[str, Any], folder: Path) -> None:
>     folder.mkdir(parents=True, exist_ok=True)
>     (folder / "project.json").write_text(json.dumps(proj_data, indent=2, ensure_ascii=False), encoding="utf-8")
>     db.upsert_project(proj_data, folder)
> ```

## 📉 Consecuencias

- 👍 **Consultas instantáneas y seguras entre hilos:** el `JobManager` puede actualizar el progreso de los trabajos muchas veces por segundo en la tabla `jobs` mientras la interfaz lee proyectos y activos sin bloqueos.
- 👍 **Transparencia total en disco:** cualquier usuario o herramienta externa puede abrir la carpeta `.vfproj` en el Explorador de Windows y encontrar todos sus archivos organizados.
- 👎 **Doble escritura en metadatos de proyecto:** siempre debe usarse el helper `_save_project_files` al actualizar un proyecto para que `project.json` y `vidforge.db` nunca diverjan.
- 🔁 **Revisar si:** se añade una función de "Importar carpeta `.vfproj` copiada desde otro computador", en cuyo caso bastará con un endpoint que lea `project.json` de la carpeta y ejecute `db.upsert_project`.

## 🔗 Relacionado

- [[Decisiones]] · [[Modelo de datos]] · [[Servidor Backend y Gestor de Trabajos]]
