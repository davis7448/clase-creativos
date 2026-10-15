# Guía de instalación — Clase de creativos con IA

**Qué vas a poder hacer al final:** investigar los anuncios que ya venden un producto y convertir esa investigación en anuncios estáticos listos para Meta, con Claude Code y Codex.

**Tiempo:** 20–30 minutos. **Hazlo antes de la clase.** El último paso (sección 7) te dice si quedó todo bien.

---

## 1. Requisitos

### Cuentas
| Cuenta | Para qué | ¿Obligatoria? |
|---|---|---|
| **Claude Pro o Max** (claude.ai) | Claude Code, el asistente que hace todo el trabajo | ✅ Sí |
| **ChatGPT Plus o superior** (chatgpt.com) | Codex, que genera las imágenes de los anuncios | ✅ Sí |
| **Meta Business** como administrador de la cuenta publicitaria | Conector de Meta: leer la Ad Library y tus campañas | Recomendada |
| **Tienda Shopify** (dueño o colaborador con permisos) | Conector de Shopify: leer tus productos | Opcional |

### Equipo
| | Mac | Windows |
|---|---|---|
| Sistema | macOS 13 o superior | Windows 10 (1809+) u 11, de 64 bits |
| Memoria | 8 GB o más | 8 GB o más |
| Navegador | Google Chrome | Google Chrome |
| Espacio libre | 5 GB | 5 GB |

> **Cómo abrir la terminal**
> - **Mac:** `Cmd + Espacio` → escribe **Terminal** → Enter.
> - **Windows:** tecla Windows → escribe **PowerShell** → Enter. No hace falta abrirla como administrador, salvo donde se indique.
>
> Copia cada comando **completo**, pégalo en la terminal y presiona Enter. Espera a que termine antes de pegar el siguiente.

---

## 2. Instalación en Mac

**Paso 1 — Homebrew** (el instalador de programas de Mac). Si ya lo tienes, salta al paso 2.
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Te pide la contraseña del Mac (no se ve mientras escribes: es normal). Al final, **copia y pega los 2 comandos que aparecen en "Next steps"** (agregan `brew` al PATH).

**Paso 2 — Node.js, Git, ffmpeg y Chrome**
```bash
brew install node git ffmpeg
brew install --cask google-chrome
```
Si ya tienes Chrome, el segundo comando avisa y sigue.

**Paso 3 — Claude Code**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```
Cierra la Terminal, ábrela de nuevo y escribe `claude`. Elige **"Claude account with subscription"** e inicia sesión en el navegador con tu cuenta Pro o Max. Sal con `/exit`.

**Paso 4 — Codex**
```bash
npm install -g @openai/codex
codex login
```
Se abre el navegador: entra con tu cuenta de **ChatGPT de pago**. Comprueba con `codex login status`: debe decir *Logged in using ChatGPT*.

**Paso 5 — Navegador para Claude (Playwright)**
```bash
claude mcp add playwright -s user -- npx @playwright/mcp@latest
```

Sigue en la **sección 4**.

---

## 3. Instalación en Windows

**Paso 1 — Node.js, Git, ffmpeg y Chrome** (PowerShell)
```powershell
winget install -e --id OpenJS.NodeJS.LTS
winget install -e --id Git.Git
winget install -e --id Gyan.FFmpeg
winget install -e --id Google.Chrome
```
Acepta los términos si te los pide (`Y`). **Cierra PowerShell y ábrelo de nuevo** para que reconozca los programas.
> ¿`winget` no existe? Instala "App Installer" desde la Microsoft Store, o descarga Node.js LTS de nodejs.org, Git de git-scm.com y ffmpeg de gyan.dev.

**Paso 2 — Permitir scripts de npm** (una sola vez)
```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```
Responde `S` o `Y`.

**Paso 3 — Claude Code**
```powershell
irm https://claude.ai/install.ps1 | iex
```
Cierra y abre PowerShell. Escribe `claude`, elige **"Claude account with subscription"** e inicia sesión con tu cuenta Pro o Max. Sal con `/exit`.

**Paso 4 — Codex**
```powershell
npm install -g @openai/codex
codex login
```
Entra con tu cuenta de **ChatGPT de pago**. Comprueba con `codex login status`.

**Paso 5 — Navegador para Claude (Playwright)**
```powershell
claude mcp add playwright -s user -- cmd /c npx -y @playwright/mcp@latest
```
> En Windows, `npx` necesita `cmd /c` adelante. Comprueba en Claude Code con `/mcp`: debe decir **playwright ✔**. Si sale ✘, escríbele a Claude: *"arregla el MCP de Playwright para Windows con add-json y cmd /c"* y él lo configura.

Sigue en la **sección 4**.

---

## 4. Instalar las skills de la clase (Mac y Windows)
Abre la terminal en tu carpeta de usuario, escribe `claude` y, **dentro de Claude Code**, escribe:
```
/plugin marketplace add davis7448/clase-creativos
/plugin install clase-creativos@clase-creativos
```
Sal con `/exit` y vuelve a entrar con `claude`. Ahora tienes 3 skills:
| Skill | Qué hace |
|---|---|
| `setup-clase` | Revisa tu instalación y te dice qué falta |
| `investigar-anuncios` | Investiga un producto y entrega la matriz de anuncios |
| `estaticos-codex` | Convierte la matriz en anuncios estáticos con Codex |

> **Sin internet a GitHub o con el repositorio privado:** descomprime el `.zip` que te compartieron y usa `/plugin marketplace add <ruta de la carpeta>` (por ejemplo `/plugin marketplace add C:/Users/tu-usuario/Downloads/clase-creativos`).

---

## 5. Conectar Shopify (opcional)
Los conectores se agregan **una sola vez en claude.ai** y después aparecen solos en Claude Code (con la misma cuenta).
1. Entra a **claude.ai** → tu nombre (abajo a la izquierda) → **Configuración** → **Conectores**.
2. Busca **Shopify** en el directorio y haz clic en **Conectar**.
   - Si no aparece: **Agregar conector personalizado** → nombre `Shopify` → URL `https://setup.shopify.com/mcp`.
3. Inicia sesión en Shopify, elige tu tienda y aprueba los permisos.
4. En Claude Code escribe `/mcp`: debe aparecer **claude.ai Shopify ✔**. Si dice que necesita autenticación, selecciónalo y sigue los pasos.
5. Prueba: *"¿qué tienda tengo conectada en Shopify?"*.
> ¿Tienes varias tiendas? Dile a Claude *"cambia a la tienda X"* y él hace el cambio de tienda.

## 6. Conectar Meta / Facebook (recomendado)
1. **claude.ai** → **Configuración** → **Conectores** → **Agregar conector personalizado**.
2. Nombre: `Facebook` · URL: `https://mcp.facebook.com/ads` → **Agregar** → **Conectar**.
   - Si en el directorio aparece **Meta Ads** o **Facebook**, puedes usar ese directamente.
3. Inicia sesión con el **perfil de Facebook que es administrador** de tu cuenta publicitaria. Elige qué **cuentas publicitarias y páginas** compartir y el tipo de permiso. Para la clase basta con **solo lectura**.
4. En Claude Code escribe `/mcp`: debe aparecer **claude.ai Facebook ✔**.
5. Prueba: *"lista mis cuentas publicitarias de Meta"*.

> ⚠️ **El conector de Meta está en beta** y se activa poco a poco. Si te sale `is_ads_mcp_enabled: false`, tu cuenta todavía no lo tiene. **No pasa nada:** la investigación funciona igual con el navegador.
> ⚠️ Con permiso de escritura, Claude puede crear y modificar campañas. Las skills de la clase **nunca publican nada sin que digas que sí**, pero revisa siempre lo que apruebas.

---

## 7. Verificar todo (el último paso)
En la terminal, en tu carpeta de usuario:
```
claude
```
y escríbele:
```
/clase-creativos:setup-clase
```
Revisa todo, crea la carpeta `mis-creativos` y **genera una imagen de prueba con Codex**. Tienes que ver una tabla con ✅. Si algo sale ❌, Claude te da el comando exacto para arreglarlo.

**Lista final antes de la clase:**
- [ ] `claude` abre e inicié sesión con Pro o Max
- [ ] `codex login status` → *Logged in using ChatGPT*
- [ ] `/mcp` muestra **playwright ✔**
- [ ] (Recomendado) `/mcp` muestra **claude.ai Facebook ✔**
- [ ] (Opcional) `/mcp` muestra **claude.ai Shopify ✔**
- [ ] `setup-clase` generó `prueba.png`
- [ ] Tengo el link de AliExpress o de mi tienda del producto que voy a trabajar, y 2–3 fotos reales del producto

---

## 8. Problemas comunes
| Problema | Solución |
|---|---|
| `command not found: claude` / `claude no se reconoce` | Cierra y abre la terminal. En Mac, revisa que hiciste los "Next steps" de Homebrew. |
| `npm` no se reconoce (Windows) | Cierra y abre PowerShell después de instalar Node. Si sigue, reinicia el PC. |
| "La ejecución de scripts está deshabilitada" (Windows) | `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` |
| `codex login` no abre el navegador | Copia el link que aparece en la terminal y pégalo en Chrome. |
| Codex no genera la imagen | Tu cuenta de ChatGPT debe ser de pago. Revisa `codex login status`. Si llegaste al límite de uso, espera un rato. |
| Error de *sandbox* de Codex en Windows | Pídele a Claude: *"usa Codex con danger-full-access"* (quita el aislamiento solo para esa carpeta). |
| `/mcp` no muestra playwright | Repite el paso 5 de tu sistema y reinicia Claude Code (`/exit` → `claude`). |
| Playwright dice que falta el navegador | `npx playwright install chrome` |
| Shopify o Facebook no aparecen en `/mcp` | Revisa que en claude.ai iniciaste sesión con la **misma cuenta** que en Claude Code (`/status` muestra la cuenta). |
| Meta: `is_ads_mcp_enabled: false` | Tu cuenta aún no tiene el beta. Sigue con el navegador. |
| Facebook pide iniciar sesión en la Ad Library | La Ad Library funciona sin sesión. Si Facebook insiste, inicia sesión una vez en la ventana de Chrome que abre Claude. |
