---
titulo: "ADR-003 Integración MCP de REA en Windows mediante wrappers cmd"
tipo: decision
estado: vigente
audiencia: [tecnico]
responsable: "SebzSGC"
actualizado: "2026-10-18"
fecha: "2026-10-15"
decisores: [SebzSGC]
reemplaza: "ninguno"
etiquetas: [tipo/decision, area/reversing]
---

# ⚖️ ADR-003 · Integración MCP de REA en Windows mediante wrappers cmd

> [!abstract] En pocas palabras
> Decidimos **registrar el servidor [[Glosario#MCP|MCP]] de [[Glosario#REA|REA]] fijando la versión exacta `rea-agents@6.5.0` e invocándolo a través de `cmd.exe /d /c npx`** porque **evita los bloqueos de políticas de ejecución de PowerShell en Windows 11 y garantiza resultados idénticos en todas las auditorías**, aceptando que **las actualizaciones de REA deben aplicarse de forma manual y controlada**.

## 🧩 Contexto

En Windows 11, cuando un cliente como Google Antigravity o una terminal PowerShell intenta ejecutar `npx` o `npm` directamente, el sistema operativo intenta resolver el script `npx.ps1`. Si la directiva `ExecutionPolicy` del equipo está en modo restringido (`Restricted`), la ejecución falla inmediatamente con un error de seguridad y el servidor MCP nunca llega a levantar su transporte `stdio`.

Además, usar `@latest` en el arranque automático de un servidor MCP introduce el riesgo de descargar versiones nuevas no auditadas en mitad de una investigación forense.

## 🔀 Opciones evaluadas

| Criterio | Opción A: Wrapper `cmd.exe /d /c npx -y rea-agents@6.5.0 mcp` ✅ | Opción B: Invocar `npx` directo con `@latest` | Opción C: Cambiar `ExecutionPolicy` global del sistema a `Unrestricted` |
|---|---|---|---|
| **Fiabilidad en Windows 11** | 100 % (usa el shim nativo `.cmd` sin pasar por PowerShell) | Falla si `ExecutionPolicy` bloquea `.ps1` | Funciona, pero altera la postura de seguridad del equipo |
| **Reproducibilidad forense** | Alta (versión `6.5.0` y catálogo de 207 herramientas congelados) | Baja (puede cambiar de versión sin previo aviso) | Baja |
| **Respeto a configuraciones ajenas** | Total (no modifica políticas globales de Windows) | Total | Violenta la regla de no alterar configuraciones globales del host |

## ✅ Decisión

1. Configurar en `C:\Users\sebas\.gemini\config\mcp_config.json` el servidor `rea` usando `cmd.exe /d /c npx -y rea-agents@6.5.0 mcp` y conservar el respaldo automático en `mcp_config.json.rea.backup`.
2. Establecer en `d:\rea-projects\AGENTS.md` la regla obligatoria para Windows de invocar siempre `npx.cmd` y `npm.cmd` cuando se ejecuten diagnósticos desde PowerShell.

> [!example] Configuración aplicada y verificada con `rea doctor`
> ```json
> {
>   "mcpServers": {
>     "rea": {
>       "command": "cmd.exe",
>       "args": ["/d", "/c", "npx", "-y", "rea-agents@6.5.0", "mcp"]
>     }
  }
> }
> ```

## 📉 Consecuencias

- 👍 **Arranque infalible del transporte `stdio` JSON-RPC 2.0** en Google Antigravity sobre Windows 11 sin tocar las políticas de seguridad de PowerShell.
- 👍 **Estabilidad del catálogo MCP:** las 207 herramientas y la skill v42 permanecen 100 % alineadas (`skill: aligned`).
- 👎 **Actualización manual:** cuando salga una nueva versión de `rea-agents`, deberá probarse primero con `setup --dry-run` antes de actualizar el número de versión en `mcp_config.json`.
- 🔁 **Revisar si:** se aprueba actualizar REA a una versión superior tras auditar su compatibilidad con Windows.

## 🔗 Relacionado

- [[Decisiones]] · [[Especialista REA y Auditoría Forense]] · [[Cómo auditar binarios con REA en Windows]] · [[Runbook · Herramientas MCP de REA no disponibles]]
