---
titulo: "Flujos y procesos"
tipo: indice
estado: vigente
audiencia: [negocio, tecnico]
responsable: "SebzSGC"
actualizado: "2026-10-18"
etiquetas: [tipo/indice, area/flujos]
---

# 🔄 Flujos y procesos

> [!abstract] Qué encontrarás aquí
> El recorrido paso a paso de lo que ocurre dentro del sistema cuando produces un vídeo automático en un clic, cuando se renderizan escenas animadas y efectos 3D, cuando exportas a CapCut Desktop o cuando auditas un binario con REA.

| Nota | En una línea | Audiencia | Estado |
|---|---|---|---|
| [[Flujo de Autopilot en un clic]] | Del guion escrito a la línea de tiempo multipista sincronizada en 4 etapas automáticas | 👩‍💼🛠️ | 🟢 vigente |
| [[Flujo de renderizado y escenas animadas]] | Cómo el Sidecar Node.js y FFmpeg NVENC convierten la línea de tiempo en un vídeo MP4 a 60 FPS | 🛠️ | 🟢 vigente |
| [[Flujo de exportación a CapCut y OTIO]] | Cómo se genera y valida un proyecto editable para CapCut Desktop, OpenTimelineIO o SRT/ASS | 👩‍💼🛠️ | 🟢 vigente |
| [[Flujo de ingeniería inversa con REA]] | Metodología forense de 4 etapas en Windows para auditar artefactos y extraer contratos Clean-Room | 🛠️ | 🟢 vigente |

> [!tip]- Vista automática (requiere el plugin Dataview)
> ```dataview
> TABLE tipo, estado, actualizado
> FROM "30 - Flujos y procesos"
> WHERE file.name != this.file.name
> SORT actualizado DESC
> ```

⬅️ Volver a [[00 - Inicio]]
