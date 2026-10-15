# Técnica del navegador (Playwright MCP)

El SKILL.md dice QUÉ hacer; esto dice CÓMO. Todo se hace con `browser_run_code_unsafe` (o `browser_navigate` + `browser_evaluate`).

## Reglas generales
- **Varias búsquedas en una sola llamada:** `page.goto` en bucle y luego `return` del resultado.
- Las variables **no persisten** entre llamadas, y dentro del código del navegador no hay `require` ni `fs`: lo que necesites, devuélvelo en el `return`.
- Las descargas se hacen con `curl` desde la terminal, nunca con `<a download>`.
- Las URLs largas (videos de Meta o TikTok) no se reescriben a mano. Se copian tal cual a un heredoc:
  ```bash
  while read archivo url; do curl -sL -A "Mozilla/5.0" -o "media/referencias/$archivo" "$url"; done <<'EOF'
  ref_meta_marca_dolor.mp4 https://video....
  EOF
  ```
- Los links de video de Meta **caducan en horas**: se bajan el mismo día.

## Meta Ad Library (sin conector)
```js
async (page) => {
  const qs = ['plantilla ortopedica', 'arch support insoles'];   // local + nombre en inglés
  const out = [];
  for (const q of qs) {
    await page.goto(`https://www.facebook.com/ads/library/?active_status=active&ad_type=all&country=ALL&is_targeted_country=false&media_type=all&search_type=keyword_unordered&q=${encodeURIComponent(q)}`);
    await page.waitForTimeout(4000);
    for (let i = 0; i < 6; i++) { await page.mouse.wheel(0, 2500); await page.waitForTimeout(1500); }
    const txt = await page.evaluate(() => document.body.innerText);
    for (const b of txt.split(/\n(?=Activo\n)/).slice(1, 40)) {
      out.push({ q,
        id: (b.match(/biblioteca:\s*(\d+)/) || [])[1],
        desde: (b.match(/En circulación desde el ([^\n]+)/) || [])[1],
        usan: (b.match(/(\d+) anuncios usan/) || [])[1],
        texto: b.slice(0, 900) });   // incluye anunciante (línea antes de "Publicidad") y el copy
    }
  }
  return out;
}
```
- Si la página sale en inglés, los textos son `Active`, `Library ID`, `Started running on` y `ads use this creative`.
- **Días activo** = hoy − "En circulación desde". Más de 90 días = PROBADO.
- **Video de un anuncio:** recorre el DOM con un TreeWalker; cada texto `biblioteca: <id>` abre un anuncio, y el primer `<video>` con `src` que aparece después es su video. Devuelve `{id, src}` y bájalo con curl.

## TikTok (sin login)
```js
async (page) => {
  await page.goto('https://www.tiktok.com/search/video?q=' + encodeURIComponent('arch support insoles'));
  await page.waitForTimeout(5000);
  for (let i = 0; i < 8; i++) { await page.mouse.wheel(0, 700); await page.waitForTimeout(1200); }
  return await page.evaluate(() => [...document.querySelectorAll('a[href*="/video/"]')]
    .slice(0, 30).map(a => ({ url: a.href, txt: a.innerText.slice(0, 120) })));
}
```
- **Detalle de un video** (vistas, likes y si es anuncio pagado): abre el video y lee `JSON.parse(document.querySelector('#__UNIVERSAL_DATA_FOR_REHYDRATION__').textContent).__DEFAULT_SCOPE__['webapp.video-detail'].itemInfo.itemStruct` → `desc`, `stats.playCount`, `stats.diggCount`, `isAd` (`true` = anuncio pagado = señal de que vende) y `locationCreated` (país).
- Los anuncios de TikTok Shop no se ven fuera de la app: márcalos "probable (TikTok Shop)".

## AliExpress
```js
async (page) => {
  await page.goto('https://www.aliexpress.com/w/wholesale-arch-support-insoles.html');
  await page.waitForTimeout(5000);
  for (let i = 0; i < 5; i++) { await page.mouse.wheel(0, 1500); await page.waitForTimeout(1000); }
  return await page.evaluate(() => [...document.querySelectorAll('a[href*="/item/"]')]
    .slice(0, 30).map(a => ({ url: a.href.split('?')[0], txt: a.innerText.slice(0, 150) })));
}
```
- Abre el candidato y confirma material, medidas y forma contra las fotos del producto. El **título en inglés** del listing es la llave para buscar en Meta y TikTok.
- El video de la galería del listing (`document.querySelector('video').src`) es material del proveedor y se puede usar.

## Mercado Libre
```js
async (page) => {
  await page.goto('https://listado.mercadolibre.com.co/plantilla-ortopedica');
  await page.waitForTimeout(4000);
  return await page.evaluate(() => [...document.querySelectorAll('li.ui-search-layout__item, div.poly-card')]
    .slice(0, 25).map(c => ({
      titulo: (c.querySelector('h2, h3, a') || {}).innerText,
      precio: (c.querySelector('.andes-money-amount__fraction') || {}).innerText,
      url: (c.querySelector('a') || {}).href })));
}
```
- Ignora los links `click1.mercadolibre...`: son publicidad de otros productos.

## Verificar visualmente (obligatorio antes de llamar "gemelo" a algo)
Toma un `browser_take_screenshot` de la tarjeta o del video, y compáralo con las fotos de `media/producto/`. Si no es el mismo producto, va como "similar", no como "gemelo".
