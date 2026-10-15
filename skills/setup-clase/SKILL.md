---
name: setup-clase
description: Revisa que el computador del estudiante tenga todo lo de la clase (Node, Git, ffmpeg, Codex con sesión, navegador Playwright y los conectores de Meta y Shopify), le dice qué falta y cómo instalarlo según sea Mac o Windows, y crea la carpeta de trabajo ~/mis-creativos. Úsala al empezar la clase, cuando el usuario diga "setup", "revisa mi instalación", "configura la clase" o cuando otra skill no encuentre una herramienta.
---

# Setup de la clase

Objetivo: dejar el equipo listo para `investigar-anuncios` y `estaticos-codex` en menos de 10 minutos, sin instalar nada sin avisar.

## 1. Detectar el sistema
Corre `uname -s 2>/dev/null || ver`.
- `Darwin` = Mac.
- `MINGW`/`MSYS` o un error con versión de Windows = Windows. Claude Code en Windows usa Git Bash si Git está instalado; si no, PowerShell.

## 2. Revisar las herramientas (todo en una sola llamada)
```bash
echo "node:   $(node -v 2>&1)"
echo "git:    $(git --version 2>&1)"
echo "ffmpeg: $(ffmpeg -version 2>&1 | head -1)"
echo "codex:  $(codex --version 2>&1)"
echo "login:  $(codex login status 2>&1)"
```
Muestra una tabla ✅/❌. Para cada ❌, da el comando de **su** sistema:

| Falta | Mac (Terminal) | Windows (PowerShell) |
|---|---|---|
| Node.js | `brew install node` | `winget install -e --id OpenJS.NodeJS.LTS` |
| Git | `brew install git` | `winget install -e --id Git.Git` |
| ffmpeg | `brew install ffmpeg` | `winget install -e --id Gyan.FFmpeg` |
| Codex | `npm install -g @openai/codex` | `npm install -g @openai/codex` |
| Sesión de Codex | `codex login` | `codex login` |

- En Mac, si no hay `brew`, primero hay que instalar Homebrew (ver `docs/GUIA-INSTALACION.pdf`).
- En Windows, después de instalar con `winget` hay que **cerrar y volver a abrir** la terminal y Claude Code. Si `npm` da un error de "scripts deshabilitados": `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`.
- `codex login` abre el navegador para entrar con la cuenta de **ChatGPT de pago** (Plus o superior). Es la que genera las imágenes.
- No instales nada sin decirle al usuario qué vas a instalar y por qué. Si el usuario prefiere hacerlo él, dale el comando con el prefijo `!` para que lo corra dentro de Claude Code.

## 3. Navegador (Playwright MCP)
Revisa si tienes disponibles las herramientas `browser_navigate` / `browser_run_code_unsafe`. Si no:
- Mac: `claude mcp add playwright -s user -- npx @playwright/mcp@latest`
- Windows: `claude mcp add playwright -s user -- cmd /c npx -y @playwright/mcp@latest`
  - Si en `/mcp` sale ✘ (algunas versiones guardaban `/c` como `C:/`): `claude mcp remove playwright -s user` y luego `claude mcp add-json playwright "{\"command\":\"cmd\",\"args\":[\"/c\",\"npx\",\"-y\",\"@playwright/mcp@latest\"]}" -s user`. Córrelo tú desde Git Bash, no desde PowerShell, para evitar los problemas de comillas.

Luego hay que reiniciar Claude Code (salir con `/exit` y volver a entrar con `claude`). Prueba que funcione abriendo `https://www.facebook.com/ads/library/` y tomando una captura.

## 4. Conectores (opcionales, recomendados)
Revisa si tienes las herramientas de **Facebook** (`ads_library_search`, `ads_get_ad_accounts`) y de **Shopify** (`search_products`, `get-shop-info`).
- **Si están:** prueba `ads_get_ad_accounts` y `get-shop-info`, y muestra la cuenta y la tienda conectadas.
- **Si no están:** la investigación funciona igual con el navegador. Para conectarlos, ver la guía:
  1. claude.ai → Configuración → Conectores.
  2. Facebook: `https://mcp.facebook.com/ads`. Shopify: buscar "Shopify" en el directorio, o `https://setup.shopify.com/mcp`.
  3. Reiniciar Claude Code y verificar con `/mcp`.
- Si Meta responde `is_ads_mcp_enabled: false`, la cuenta todavía no tiene el beta de Meta. No tiene arreglo desde aquí: se sigue con el navegador.

## 5. Carpeta de trabajo y prueba de Codex
1. Crea `~/mis-creativos/` (en Windows, `%USERPROFILE%\mis-creativos`).
2. **Prueba real de imagen** (1–2 min). Confirma que la cuenta de ChatGPT genera imágenes desde Codex:
```bash
mkdir -p ~/mis-creativos/_prueba && codex exec --skip-git-repo-check -s workspace-write -C ~/mis-creativos/_prueba -- "Use your image generation tool to create a simple square image of a red apple on a white table. Save/copy the generated image into the current working directory as prueba.png" >/dev/null 2>&1; ls ~/mis-creativos/_prueba
```
   Si aparece `prueba.png`, ábrela con Read y confirma. Si no aparece, corre el mismo comando sin `>/dev/null 2>&1` para ver el error. Lo más común:
   - no hay sesión o el plan no es de pago → `codex login`;
   - se llegó al límite de uso → esperar o cambiar de cuenta;
   - **Windows:** error de sandbox → repetir con `-s danger-full-access` en vez de `-s workspace-write`. El `-C` sigue limitando la carpeta de trabajo. Avisa al usuario de que ese modo quita el aislamiento.

## 6. Cierre
Muestra la tabla final (✅/❌) y el siguiente paso de la clase:
> **"Investiga este producto: `<link de AliExpress o de tu tienda>`"** → luego **"Crea los estáticos de la matriz"**.
