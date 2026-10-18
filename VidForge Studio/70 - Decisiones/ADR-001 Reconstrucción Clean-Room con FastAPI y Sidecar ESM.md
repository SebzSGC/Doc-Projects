---
titulo: "ADR-001 Reconstrucción Clean-Room con FastAPI y Sidecar ESM"
tipo: decision
estado: vigente
audiencia: [negocio, tecnico]
responsable: "SebzSGC"
actualizado: "2026-10-18"
fecha: "2026-10-16"
decisores: [SebzSGC]
reemplaza: "ninguno"
etiquetas: [tipo/decision, area/arquitectura]
---

# ⚖️ ADR-001 · Reconstrucción Clean-Room con FastAPI y Sidecar ESM

> [!abstract] En pocas palabras
> Decidimos **reconstruir el generador de vídeos desde cero (*[[Glosario#Clean-Room|Clean-Room]]*) usando FastAPI asíncrono en Python 3.12 junto con un [[Glosario#Sidecar|Sidecar]] ligero en Node.js 24 (`puppeteer-core`)** porque **ofrece un código 100 % mantenible, tipado y rápido sin bloqueos de interfaz**, aceptando que **debimos diseñar y validar nuestros propios esquemas JSON Schema y pruebas desde cero**.

## 🧩 Contexto

Tras auditar `Creator-IA 2.0 (Entrega a5)` con [[Glosario#REA|REA]], comprobamos que el sistema original:
1. Concentraba **193 rutas HTTP** en un servidor síncrono Flask + Waitress compilado con [[Glosario#Nuitka|Nuitka]] (`server.cp313-win_amd64.pyd` de `6,86 MB`).
2. Dependía de pesados bundles de `@hyperframes/producer` y Remotion que requerían parches en caliente (*monkey-patching*) sobre `node:child_process`.
3. Carecía de código fuente editable y bloqueaba el arranque si se añadía cualquier archivo `.py` o `.pyd` nuevo.

Necesitábamos una arquitectura propia para Windows x64 que fuera rápida, fácil de mantener, 100 % funcional sin internet y con progreso en tiempo real.

## 🔀 Opciones evaluadas

| Criterio | Opción A: FastAPI + Sidecar ESM ligero ✅ | Opción B: Parchear binarios `.pyd` originales | Opción C: Monolito Electron puro (todo en Node.js) |
|---|---|---|---|
| **Mantenibilidad** | Alta (código limpio, modular y tipado con Pydantic v2) | Muy baja (binarios cerrados y frágiles ante cambios) | Media (peor integración con librerías de audio/ASR en Python) |
| **Rendimiento y concurrencia** | Alta (`JobManager` con semáforos `gpu`/`cpu` + [[Glosario#SSE\|SSE]]) | Media (hilos WSGI en Waitress sin cola unificada) | Media |
| **Peso y simplicidad de render** | Ligero (`puppeteer-core` + `image2pipe` directo a FFmpeg) | Pesado (`41.201` archivos y `1,70 GB` instalados) | Pesado (Chromium duplicado) |
| **Riesgo legal / técnico** | Nulo (implementación 100 % *Clean-Room*) | Alto (bloqueos de integridad `WinVerifyTrust` y SHA-256) | Bajo |

## ✅ Decisión

Construir **VidForge Studio** desde cero en `d:\rea-projects\vidforge` separando claramente tres capas:
1. **Backend FastAPI (`/api/v1/*`):** reduce las 193 rutas dispersas a **28 endpoints REST + SSE** bien estructurados con modelos Pydantic v2 (`domain/models.py`) y contratos JSON Schema 2020-12 (`packages/contracts/`).
2. **Sidecar ESM (`apps/render-sidecar/src/sidecar.mjs`):** un único módulo limpio en Node.js que usa `puppeteer-core` y [[Glosario#CDP|CDP]] para capturar plantillas HTML/CSS/SVG con transparencia (`webm_vp9_alpha`) y shaders [[Glosario#WebGL|WebGL]] GLSL enviando los fotogramas por tubería en memoria (`image2pipe`) a FFmpeg.
3. **Lanzador nativo (`apps/launcher/main.py`):** usa `pywebview` sobre [[Glosario#WebView2|WebView2]] de Windows en lugar de empaquetar otro navegador completo para la ventana principal.

> [!example] Impacto concreto de la decisión
> El contrato de plantillas se simplificó a dos funciones deterministas en el navegador (`window.__vf.init(props, canvas)` y `window.__vf.seek(tMs)`), permitiendo crear las 6 plantillas animadas en archivos HTML autocontenidos de menos de 4 KB cada uno.

## 📉 Consecuencias

- 👍 **Código 100 % auditable y probado:** toda la lógica de negocio vive en módulos Python y ESM legibles, cubiertos por `pytest` e `hypothesis`.
- 👍 **Cero ventanas negras emergentes:** todos los procesos hijos corren bajo [[Glosario#Job Object|Job Objects]] de Windows con `CREATE_NO_WINDOW`.
- 👎 **Doble runtime local (Python + Node.js):** requiere disponer tanto del entorno virtual Python (`runtime/venv`) como de `node.exe` para el sidecar de plantillas.
- 🔁 **Revisar si:** en el futuro se desea distribuir la aplicación en macOS o Linux, en cuyo caso bastará con adaptar las llamadas específicas de Windows (`DPAPI` y `Job Objects` ya cuentan con *fallbacks* multiplataforma en el código).

## 🔗 Relacionado

- [[Decisiones]] · [[Arquitectura general]] · [[Servidor Backend y Gestor de Trabajos]] · [[Sidecar de Renderizado y Motor FFmpeg]]
