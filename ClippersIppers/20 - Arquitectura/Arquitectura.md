---
titulo: "Arquitectura — Diseño del sistema, integraciones y componentes de OpusClip"
tipo: moc
estado: aprobado
audiencia: [todos]
responsable: "Ingeniería Inversa (REA)"
actualizado: 2026-10-18
etiquetas:
  - opusclip
  - arquitectura
  - moc
---

# 🏗️ Arquitectura

> [!abstract] En pocas palabras
> Esta sección documenta la arquitectura técnica completa de **OpusClip Dashboard (`clip.opus.pro/dashboard`)**: cómo está empaquetada la aplicación web, con qué servicios externos se comunica y cómo funcionan por dentro cada uno de sus diez subsistemas principales.

```mermaid
flowchart TD
    Arq["🏗️ Arquitectura (MOC)"] --> AG["📐 Arquitectura general<br/>(Next.js + Turbopack + Jotai + React Query)"]
    Arq --> INT["🔌 Integraciones<br/>(GCS, Drive, YouTube, Statsig, Turnstile, Brevo)"]
    Arq --> COMP["🧩 Componentes (MOC)<br/>(10 Subsistemas Internos)"]
    COMP --> C1["1. Módulo Dashboard y Workflows"]
    COMP --> C2["2. Motor de Ingesta y Subida"]
    COMP --> C3["3. Motor de Curación IA"]
    COMP --> C4["4. Auto-Importación YouTube"]
    COMP --> C5["5. Plantillas de Marca y Subtítulos"]
    COMP --> C6["6. Gestor de Proyectos y Búsqueda"]
    COMP --> C7["7. Editor Multipista y Renderizado"]
    COMP --> C8["8. Créditos, Planes y Facturación"]
    COMP --> C9["9. Publicación Social y Calendario"]
    COMP --> C10["10. Campañas y Marketplace"]
```

El diagrama muestra la estructura jerárquica de la sección de Arquitectura: desde el diseño global y las integraciones externas hasta el catálogo de los diez componentes funcionales que conforman la plataforma.

> [!example] Ejemplo real: Desacoplamiento entre Interfaz, Estado y Red
> Ningún componente visual del dashboard llama a `fetch` directamente. Toda petición pasa por el creador de cabeceras `useClipApiHeaderWrapperCreator` (módulo `693320`), que lee los prefijos de API desde `apiStore` (`986231`), inyecta automáticamente el token de organización desde `authStore` (`847027`), aplica los encabezados de enrutamiento (`x-opus-clip-project-toggle: clip-api`, `x-associated-org-id`) y deposita los resultados normalizados en tiendas [[Glosario#Átomo Jotai|Jotai]] (`clipProjectStore`, `clipStore`, `assetStore`).

---

## 📑 Notas de esta sección

| Nota | Propósito |
|---|---|
| [[Arquitectura general]] | Visión global de las 4 capas del cliente web: Presentación (Next.js/Radix/Tailwind), Estado Reactivo (161 tiendas Jotai + TanStack Query), Servicios/Transporte (`apiStore` + Web Workers) y Backend/Cloud (`api.opus.pro` + GCS). |
| [[Integraciones]] | Detalle técnico de los 15 proveedores externos conectados al dashboard (autenticación, almacenamiento en nube, feature flags, antifraude, pagos, CRM y analítica). |
| [[Componentes]] | Índice detallado de los **10 subsistemas internos** de OpusClip analizados a nivel de módulo y función. |

---

> [!info]- Detalle técnico (para desarrolladores)
> - **Prefijos de API configurados en `apiStore` (módulo `986231` en `0bk_j0ng_-uy_.js` y `1_shcqomv7f9c.js`)**:
>   - `apiPrefix` (`global.api.apiPrefixV2`): `"https://api.opus.pro/api"`
>   - `nestApiPrefix` (`global.api.nestApiPrefixV2`): `"https://api.opus.pro/api"`
>   - `planSocialApiPrefix` (`global.api.planSocialApiPrefix`): `"https://api.opus.pro/api"`
>   - Entorno de staging detectado por `isWorkflowStagingApi` (`404898`): `"https://stg-api.opus.pro"`.

## 🔗 Relacionado
- [[00 - Inicio]] — Portada principal del vault.
- [[Flujos y procesos]] — Cómo colaboran estos componentes en escenarios de ejecución reales.
- [[Modelo de datos y Estado Global Jotai]] — Esquema de las tiendas de estado que conectan la arquitectura.
