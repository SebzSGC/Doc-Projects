---
titulo: "Cómo auditar binarios con REA en Windows"
tipo: guia
estado: vigente
audiencia: [tecnico]
dificultad: "🟡 media"
tiempo: "15 min"
responsable: "SebzSGC"
actualizado: "2026-10-18"
etiquetas: [tipo/guia, area/reversing]
---

# 🧭 Cómo auditar binarios con REA en Windows

> [!abstract] Al terminar tendrás…
> Un diagnóstico verificado de un archivo ejecutable, ensamblado `.NET` o aplicación JavaScript/Electron en Windows 11 usando **[[Glosario#REA|REA]] (`rea-agents@6.5.0`)**, con evidencias criptográficas reproducibles y sin riesgo de ejecutar código desconocido.

| 🎚️ Dificultad | ⏱️ Tiempo | 👤 Para |
|---|---|---|
| 🟡 Media | 15 min | Desarrolladores y analistas de ingeniería inversa |

## ✅ Antes de empezar necesitas

- [ ] Tener abierto este workspace (`d:\rea-projects`) en **Google Antigravity**.
- [ ] Node.js `v24.14.1+` instalado en el sistema.
- [ ] Colocar el archivo o carpeta que vas a analizar dentro de `d:\rea-projects\projects\<nombre-objetivo>\`.

## 🪜 Pasos

### 1. Auditar la salud del servidor REA antes de empezar

Siguiendo la regla #1 del especialista (**Diagnóstico antes de Reparación**), comprueba desde PowerShell que el servidor [[Glosario#MCP|MCP]] y la skill v42 estén alineados usando siempre el shim `.cmd`:

```powershell
# Verifica que REA 6.5.0 y el cliente Antigravity estén saludables
npx.cmd -y rea-agents@6.5.0 doctor --client antigravity --json
```

> [!success] Deberías ver
> Un JSON con `"healthy": true`, `"registration:antigravity": "ok"` y `"tools": 207`.

### 2. Calcular la huella SHA-256 del artefacto en modo de solo lectura

Nunca ejecutes el binario que estás investigando. Primero registra su tamaño exacto y su hash SHA-256:

```powershell
# Calcula el hash SHA-256 del binario objetivo sin ejecutarlo
Get-FileHash "d:\rea-projects\projects\01-generadorVideos\analisis\extracted_app\Creator-IA.exe" -Algorithm SHA256
```

> [!success] Deberías ver
> El algoritmo `SHA256` y el hash de 64 caracteres hexadecimales (ej. `D9BA08BB5DB461045A65F92A4529D4C1E9B4A63DE2D1D0F275766A44D0B9E31B`).

### 3. Ejecutar la herramienta de REA adecuada según el tipo de archivo

Puedes pedirle directamente al agente en el chat de Antigravity que invoque las herramientas MCP de `rea` o ejecutar el comando por CLI:

#### Caso A · Aplicación JavaScript / Electron / Bundle Vite
```powershell
# Analiza el árbol sintáctico (AST), módulos, fronteras IPC y llamadas de una app JS
npx.cmd -y rea-agents@6.5.0 analyze-javascript-application "d:\rea-projects\projects\01-generadorVideos\analisis\extracted_app\frontend\scripts" --json
```

#### Caso B · Ejecutable Windows PE (`.exe`, `.dll`, `.pyd`) o Ensamblado `.NET`
Desde el chat de Antigravity, solicita usar las herramientas MCP de `rea`:
1. `inspect_pe_resources` → extrae iconos, manifiesto `RT_MANIFEST`, versión `RT_VERSION` y tablas `RT_RCDATA` (como los offsets de [[Glosario#Inno Setup|Inno Setup]]).
2. `inspect_managed_artifact` y `inspect_managed_members` → confirma si tiene cabecera CLR (`.NET`) y lista clases/métodos [[Glosario#CIL|CIL]] sin cargar el runtime.
3. `inspect_go_binary` e `inspect_rust_binary` → confirma o descarta empíricamente si fue compilado con Go o Rust.

> [!success] Deberías ver
> Cada herramienta devuelve un identificador de evidencia único (`ev_...`) que puedes paginar de forma determinista con `inspect_analysis_view`.

## 🛠️ Si algo falla

| Síntoma | Causa probable | Solución |
|---|---|---|
| `npm : No se puede cargar el archivo ... npm.ps1` | PowerShell bloquea scripts `.ps1` por `ExecutionPolicy` | Usa siempre `npx.cmd` y `npm.cmd` en lugar de `npx` y `npm` |
| `hopper` aparece como `unsupported_host` en `rea doctor` | Hopper Disassembler solo existe para macOS | Ignora ese aviso en Windows; para descompilación C/C++ en Windows se usa [[Glosario#Ghidra\|Ghidra]] |
| `inspect_sqlite_database` reporta `unsupported_version` | Requiere Node.js `>= 24.15` (`DatabaseSync.limits`) y el host tiene `24.14.1` | Usa `sqlite3` de Python (`d:\rea-projects\vidforge\runtime\venv\Scripts\python.exe`) o actualiza Node a `24.15+` |
| Las herramientas `rea` no responden en Antigravity | Sesión de Antigravity sin recargar tras cambio MCP | Sigue [[Runbook · Herramientas MCP de REA no disponibles]] |

## 🔗 Relacionado

- [[Especialista REA y Auditoría Forense]] · [[Flujo de ingeniería inversa con REA]] · [[Runbook · Herramientas MCP de REA no disponibles]] · [[FAQ]]
