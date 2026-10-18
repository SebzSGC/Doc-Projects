---
titulo: "Primeros pasos"
tipo: guia
estado: vigente
audiencia: [negocio, tecnico]
dificultad: "🟢 fácil"
tiempo: "5 min"
responsable: "SebzSGC"
actualizado: "2026-10-18"
etiquetas: [tipo/guia, area/guias]
---

# 🧭 Primeros pasos

> [!abstract] Al terminar tendrás…
> **VidForge Studio** verificado y abierto en tu pantalla (ya sea como aplicación nativa de escritorio en Windows o en tu navegador web) y habrás comprobado que todos sus motores locales responden sin errores.

| 🎚️ Dificultad | ⏱️ Tiempo | 👤 Para |
|---|---|---|
| 🟢 Fácil | 5 min | Cualquier persona que empiece en el proyecto |

## ✅ Antes de empezar necesitas

- [ ] Estar en un equipo con **Windows 10/11 x64**.
- [ ] Tener acceso a la carpeta del proyecto en `d:\rea-projects\vidforge` (que ya incluye su propio entorno aislado en `runtime/`).

## 🪜 Pasos

### 1. Verificar la integridad de la instalación

Antes de abrir la aplicación por primera vez, comprueba desde PowerShell que los 78 archivos del sistema estén completos y que el backend importe correctamente:

```powershell
# Verifica los hashes SHA-256 de MANIFEST.json y las importaciones de FastAPI
d:\rea-projects\vidforge\runtime\venv\Scripts\python.exe d:\rea-projects\vidforge\tools\verify-install.py d:\rea-projects\vidforge
```

> [!success] Deberías ver
> `OK: Verified 78 files and backend imports.`

### 2. Iniciar VidForge Studio (elige tu modo preferido)

Tienes dos lanzadores directos en la raíz de `d:\rea-projects\vidforge`:

#### Opción A · Modo Escritorio Nativo (recomendado)
Abre una ventana dedicada de Windows con [[Glosario#WebView2|WebView2]] (`1440x900`) sin mostrar ninguna consola negra detrás:

```powershell
# Inicia la ventana nativa de escritorio de VidForge Studio
d:\rea-projects\vidforge\Iniciar-VidForge.bat
```

*(También puedes hacer **doble clic** sobre `Iniciar-VidForge.bat` desde el Explorador de Windows).*

#### Opción B · Modo Navegador Web Estándar
Si prefieres usar Google Chrome o Microsoft Edge con las herramientas de desarrollador (`F12`):

```powershell
# Levanta el servidor en el puerto 8765 y abre el navegador predeterminado
d:\rea-projects\vidforge\Iniciar-Servidor-Web.bat
```

> [!success] Deberías ver
> La interfaz oscura de **VidForge Studio — Generador Modular de Vídeos IA** con la barra lateral de proyectos a la izquierda y los 5 espacios de trabajo en la parte superior (**Director & Prompts**, **Multimedia & AudioLab**, **Sync & Plantillas**, **Editor Studio** y **Hardware & Claves**).

### 3. Comprobar el hardware detectado y ejecutar la suite de pruebas (opcional para técnicos)

Para confirmar que el codificador [[Glosario#NVENC|NVENC]], [[Glosario#AudioLab|AudioLab]], la [[Glosario#Alineación forzada|alineación forzada]] y el exportador de [[Glosario#CapCut Desktop|CapCut Desktop]] pasan todas las pruebas automatizadas:

```powershell
# Ejecuta la suite completa de pruebas unitarias y de propiedades (Hypothesis)
d:\rea-projects\vidforge\runtime\venv\Scripts\pytest.exe d:\rea-projects\vidforge\apps\backend\tests\test_vidforge_suite.py -v
```

> [!success] Deberías ver
> Todos los tests en estado `PASSED` en menos de 15 segundos.

## 🛠️ Si algo falla

| Síntoma | Causa probable | Solución |
|---|---|---|
| Al hacer doble clic en `Iniciar-VidForge.bat` no se abre la ventana | Falta el componente **Microsoft Edge WebView2 Runtime** en Windows | Usa `Iniciar-Servidor-Web.bat` (que abre tu navegador normal) o instala WebView2 Runtime de Microsoft |
| El puerto `8765` ya está ocupado por otra sesión previa | Quedó un servidor web anterior abierto | `Iniciar-VidForge.bat` busca automáticamente otro puerto libre; para `Iniciar-Servidor-Web.bat`, cierra la ventana anterior del servidor |
| `verify-install.py` reporta `Hash mismatch` tras editar código | Modificaste un archivo de `apps/` o `packages/` sin actualizar el manifiesto | Ejecuta `d:\rea-projects\vidforge\runtime\venv\Scripts\python.exe d:\rea-projects\vidforge\tools\verify-install.py --generate d:\rea-projects\vidforge` |

## 🔗 Relacionado

- [[Cómo crear y exportar un vídeo con IA]] · [[Arquitectura general]] · [[Interfaz Web Studio y Lanzador Nativo]] · [[FAQ]]
