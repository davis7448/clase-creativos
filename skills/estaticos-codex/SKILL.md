---
name: estaticos-codex
description: Crea anuncios estáticos (imágenes 4:5 y 9:16 para Meta) a partir de la investigación, con Codex como generador de imágenes y las fotos reales del producto como referencia. Lee MATRIZ.md de investigar-anuncios y produce cada anuncio con su copy. Úsala cuando el usuario diga "crea los estáticos", "haz las imágenes de los anuncios", "anuncios estáticos con Codex", "produce la matriz" o "haz el anuncio P03".
---

# Anuncios estáticos con Codex

**Entrada:** `~/mis-creativos/<producto>/MATRIZ.md`, `PRODUCTO.md` y las fotos de `media/producto/`. Todo lo deja la skill `investigar-anuncios`. Si no existen, pide primero la investigación o, como mínimo, una foto real del producto y los datos de cada anuncio.
**Salida:**
- `estaticos/<ID>_4x5.png` (1080×1350, feed) y, si se pide, `<ID>_9x16.png` (1080×1920, historias y reels).
- `ESTATICOS.md` con el copy de cada anuncio.

## 0. Verificar herramientas (una vez por sesión)
```bash
codex --version && codex login status    # debe decir "Logged in using ChatGPT"
ffmpeg -version | head -1                # para dejar cada imagen en la medida exacta
```
- Si Codex no tiene sesión: `codex login` (abre el navegador; se entra con la cuenta de ChatGPT de pago).
- Si falta algo, manda al usuario a `docs/GUIA-INSTALACION.pdf`.

## 1. Reglas de fidelidad (no negociables)
1. **Codex recibe SOLO fotos reales del producto como referencia** (`-i media/producto/foto1.jpg`). Nunca imágenes de competidores: copiaría su producto o su marca.
2. **El producto tiene que salir idéntico a la foto:** forma, color, piezas y proporciones. Si cambia, se regenera.
3. **Texto en español sin errores.** Codex se equivoca con tildes y palabras largas. Por eso los textos largos, los precios y los formatos nativos se componen en HTML (modos 2 y 3).
4. **Uso correcto:** el producto se usa como se usa en la vida real (una plantilla va dentro del zapato, no en la mano de alguien caminando).
5. **Sin claims prohibidos** (ver `PRODUCTO.md`), sin precio "antes" inventado y sin logos ajenos.

## 2. Elegir el modo por formato
| Modo | Cuándo | Cómo |
|---|---|---|
| **1. Codex completo** | Escenas con poco texto (titular de ≤ 6 palabras): lifestyle, antes/después, problema visual, producto en uso | Codex genera la imagen con el titular incluido |
| **2. Codex escena + texto HTML** | Argumento con bullets, comparativas, oferta con precios, titulares largos | Codex genera la escena **sin texto** y Claude compone el texto encima en HTML |
| **3. Nativo HTML** | Nota del iPhone, chat de WhatsApp, búsqueda de Google, ticket, historia de IG, tweet, email | Claude arma la interfaz en HTML con la foto real del producto (Codex solo si hace falta una foto casera) |

Si en el modo 1 el texto sale mal 2 veces, se pasa al modo 2.

## 3. Modo 1 y 2 — generar con Codex
Desde la carpeta del producto. **Un comando por anuncio**, hasta 3 en paralelo:
```bash
cd ~/mis-creativos/<producto>
mkdir -p estaticos/raw
codex exec --skip-git-repo-check -s workspace-write -C "$PWD/estaticos/raw" \
  -i "$PWD/media/producto/foto1.jpg" -i "$PWD/media/producto/foto2.jpg" -- \
  "Use your image generation tool to create a vertical 4:5 Meta ad image. <PROMPT DEL ANUNCIO>. \
The product must look EXACTLY like the reference photos (same shape, color, parts, proportions). \
Save/copy the generated image into the current working directory as P01_raw.png" >/dev/null 2>&1 &
# … P02, P03 …
wait; ls estaticos/raw
```
- `-i` acepta varias fotos, y el prompt va **después de `--`**.
- Las plantillas de prompt por formato están en [references/prompts.md](references/prompts.md). En el modo 2, el prompt termina en **"No text, no letters, no logos anywhere in the image. Leave clean empty space at the <top/bottom> for text."**
- Cada imagen tarda de 1 a 3 minutos. Si no aparece el archivo, se vuelve a correr ese anuncio solo, sin `>/dev/null`, para ver el error.
- **Windows:** el mismo comando funciona en Git Bash, que es la terminal que usa Claude Code si Git para Windows está instalado. En PowerShell, cambia `\` por `` ` `` y quita `&`/`wait` (genera de a uno). Si Codex da un error de sandbox en Windows, usa `-s danger-full-access` (avísale al usuario).

**Dejarla a medida** (Codex entrega 1024×1536 o similar):
```bash
ffmpeg -y -v error -i estaticos/raw/P01_raw.png -vf "scale=1080:1350:force_original_aspect_ratio=increase,crop=1080:1350" estaticos/P01_PROD_DOLOR_ESCENA_H1_v1_4x5.png
```

## 4. Modos 2 y 3 — texto exacto con HTML (Playwright)
1. Sirve la carpeta del producto para que el navegador vea las imágenes:
   `npx -y http-server ~/mis-creativos/<producto> -p 8777 -s` (en segundo plano).
2. Con `browser_run_code_unsafe`: abre una página nueva de 1080×1350, carga el HTML con `setContent` (las imágenes como `http://localhost:8777/estaticos/raw/P02_raw.png` o `.../media/producto/foto1.jpg`), espera las fuentes y toma la captura:
```js
async (page) => {
  const p = await page.context().newPage();
  await p.setViewportSize({ width: 1080, height: 1350 });
  await p.setContent(`<html><head><link href="https://fonts.googleapis.com/css2?family=Inter:wght@500;800&display=swap" rel="stylesheet">
  <style>body{margin:0;width:1080px;height:1350px;font-family:Inter,sans-serif;position:relative;overflow:hidden}
  .bg{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
  .t{position:absolute;left:60px;right:60px;top:70px;font-size:76px;font-weight:800;line-height:1.05;color:#fff;text-shadow:0 4px 18px rgba(0,0,0,.45)}
  </style></head><body>
  <img class="bg" src="http://localhost:8777/estaticos/raw/P02_raw.png"><div class="t">Titular exacto aquí</div></body></html>`);
  await p.evaluate(() => document.fonts.ready); await p.waitForTimeout(400);
  await p.screenshot({ path: '<RUTA ABSOLUTA>/estaticos/P02_..._4x5.png' });
  await p.close(); return 'ok';
}
```
3. Las plantillas HTML de los formatos nativos (nota del iPhone, WhatsApp, Google, ticket, historia de IG) y del bloque de oferta están en [references/plantillas-html.md](references/plantillas-html.md).

## 5. Control de calidad (cada imagen, con Read)
Abre cada PNG final y revisa:
- [ ] El producto es idéntico a la foto real (sin piezas inventadas ni deformes).
- [ ] La ortografía y las tildes están bien, y el texto coincide con `MATRIZ.md`.
- [ ] El titular se lee en un celular (más de 60 px en 1080 de ancho) y no queda tapado en los bordes: 9:16 deja libres 250 px arriba y abajo.
- [ ] El producto se usa correctamente en la escena.
- [ ] El precio y la oferta son reales; no hay claims ❌ ni logos ajenos.
Si algo falla, se corrige y se regenera solo esa pieza (máximo 3 intentos; después, se cambia de modo o se avisa).

## 6. Entrega
1. `ESTATICOS.md` en la carpeta del producto. Por anuncio: ID · archivo · ángulo · formato · etapa · copy para Meta (texto principal, título y descripción, de `MATRIZ.md`).
2. Abre la carpeta para que el usuario las vea: `open estaticos` (Mac) o `explorer estaticos` (Windows).
3. Muestra en el chat la tabla y las rutas. Pregunta cuáles se aprueban.
4. **No se suben a Meta sin el visto bueno del usuario.** Si lo da y está el conector de Meta: `ads_creative_upload_media` → `ads_create_creative` → anuncio en borrador. Nunca se activa sin otro ok explícito.

## Iterar
- Para cambiar un anuncio, se cambia **una sola cosa por versión** (titular, escena o formato) y se sube el número: `_v2`.
- Cuando el anuncio ganador esté identificado (después de 5–7 días con gasto parejo), se le hacen **6 titulares nuevos con la misma imagen** y **el mismo ángulo en 3–6 formatos más**. Ver `investigar-anuncios/references/formatos.md`, sección "Cómo iterar".
