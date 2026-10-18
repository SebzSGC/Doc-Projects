---
titulo: "Preguntas frecuentes"
tipo: faq
estado: vigente
audiencia: [negocio, tecnico]
responsable: "SebzSGC"
actualizado: "2026-10-18"
etiquetas: [tipo/faq, area/recursos]
---

# ❓ Preguntas frecuentes

> [!abstract] En pocas palabras
> Las dudas más habituales sobre cómo usar **VidForge Studio** y el especialista **[[Glosario#REA|REA]]** en Windows, respondidas de forma directa. Si surge una nueva pregunta recurrente, agrégala aquí.

## 👩‍💼 Para creadores y usuarios de negocio

> [!question]- ¿Necesito conexión a internet o pagar suscripciones para crear un vídeo?
> **No.** VidForge Studio es 100 % operativo sin conexión a internet (*Offline-First*): genera la voz con el sintetizador nativo de Windows, alinea cada palabra localmente, dibuja las escenas animadas en tu equipo y codifica el vídeo con tu tarjeta gráfica. Las integraciones con OpenAI u Ollama son totalmente opcionales. Ver [[Integraciones]].

> [!question]- ¿Qué diferencia hay entre el botón "Autopilot 1-Click" y ir pestaña por pestaña?
> **Autopilot 1-Click** ejecuta de corrido las 4 etapas (dividir guion en escenas, crear locución [[Glosario#TTS|TTS]], alinear palabras y armar la línea de tiempo) en menos de 20 segundos. Ir pestaña por pestaña te permite subir tu propia voz grabada, limpiarla en [[Glosario#AudioLab|AudioLab]] o elegir manualmente qué imagen va en cada escena. Ver [[Flujo de Autopilot en un clic]].

> [!question]- ¿Por qué debo cerrar CapCut Desktop antes de exportar un borrador hacia su carpeta?
> Porque mientras `CapCut.exe` está abierto mantiene en memoria su propio índice de proyectos y podría sobrescribir el borrador recién exportado. Por seguridad, VidForge comprueba si el proceso está activo antes de exportar. Ver [[Flujo de exportación a CapCut y OTIO]].

> [!question]- ¿Por qué el sistema me impide renderizar una tarjeta animada si cambio un número?
> Es una protección de calidad llamada **validación semántica *Fail-Closed***: si el locutor dice *"creció un 87%"* pero en la tarjeta escribes `"95%"`, el sistema bloquea el render (`422`) para evitar que publiques un vídeo donde el gráfico contradice a la voz. Ver [[Catálogo de plantillas y contratos JSON]].

## 🛠️ Para el equipo técnico y de ingeniería inversa

> [!question]- ¿Cómo inicio el proyecto localmente y corro las pruebas automatizadas?
> Haz doble clic en `d:\rea-projects\vidforge\Iniciar-VidForge.bat` (o `Iniciar-Servidor-Web.bat`) y ejecuta `pytest` con el entorno virtual incluido en `runtime/venv`. Consulta los comandos exactos en [[Primeros pasos]].

> [!question]- ¿Dónde se guardan mis proyectos y cómo hago una copia de seguridad?
> El índice rápido vive en `d:\rea-projects\vidforge\storage\userdata\vidforge.db` y cada proyecto tiene su propia carpeta autocontenida en `d:\rea-projects\vidforge\storage\projects\<Nombre>_<id>.vfproj\`. Para respaldar un proyecto basta con copiar su carpeta `.vfproj`. Ver [[Modelo de datos]].

> [!question]- ¿Por qué en PowerShell debo escribir `npx.cmd` en lugar de `npx` al usar REA?
> Porque la directiva de ejecución predeterminada de PowerShell en Windows bloquea los scripts `.ps1` (`npx.ps1`). Invocar `npx.cmd` o `npm.cmd` utiliza el ejecutable por lotes de Windows y evita el bloqueo sin alterar la seguridad del sistema. Ver [[ADR-003 Integración MCP de REA en Windows mediante wrappers cmd]].

> [!question]- ¿Por qué `rea doctor` dice que Hopper es `unsupported_host` y SQLite Inspector `unsupported_version`?
> - **Hopper Disassembler** es exclusivo de macOS; en Windows el motor nativo soportado por REA es **[[Glosario#Ghidra|Ghidra]]**.
> - **SQLite Inspector** de REA requiere Node.js `>= 24.15` (por la API `DatabaseSync.limits`) y el equipo actual tiene Node `24.14.1`. Ninguno de los dos afecta la salud general (`healthy: true`). Ver [[Especialista REA y Auditoría Forense]].

## 🔗 Relacionado

- [[00 - Inicio]] · [[Primeros pasos]] · [[Glosario]] · [[Monitoreo y diagnóstico]]
