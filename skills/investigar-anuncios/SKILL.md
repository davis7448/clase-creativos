---
name: investigar-anuncios
description: Investiga los anuncios que ya funcionan para un producto (Meta Ad Library, TikTok, AliExpress y Mercado Libre) y entrega una matriz de anuncios lista para producir (ángulo × formato, con gancho, texto y CTA). Úsala cuando el usuario diga "investiga el producto X", "busca anuncios ganadores", "qué anuncios hace la competencia", "Ad Library", "espiar competencia", "ideas de anuncios" o "qué creativos testear".
---

# Investigar anuncios → matriz lista para producir

**Para qué sirve:** pasar de "tengo un producto" a "tengo 12 anuncios definidos, con evidencia de que el enfoque funciona".
**Qué entrega:** 4 archivos en la carpeta del producto y los links en el chat. El último archivo, `MATRIZ.md`, es la entrada de la skill `estaticos-codex`.

## 0. Antes de empezar
- La carpeta de trabajo es `~/mis-creativos/<producto>/`. Si no existe, créala con las subcarpetas `media/producto/` y `media/referencias/`. Si el usuario nunca corrió el setup, sugiérele `/clase-creativos:setup-clase`.
- Herramientas que usa esta skill:
  - **Navegador (Playwright MCP)**: obligatorio.
  - **Conector de Meta (Facebook)**: opcional; si está, usa `ads_library_search`.
  - **Conector de Shopify**: opcional; si está, lee el producto de la tienda.
- Si falta el navegador, detente y manda al usuario a la guía de instalación (`docs/GUIA-INSTALACION.pdf`).
- Responde siempre en el idioma del usuario.

## 1. Verificar el producto REAL (no negociable)
Nunca investigues un producto supuesto: si se asume mal el producto, se pierde toda la investigación.
1. Pide la fuente: link de la tienda (Shopify), link de AliExpress o del proveedor, o fotos. Si hay conector de Shopify, busca el producto ahí (`search_products` o `get-product`).
2. Guarda 2 o 3 **fotos reales** del producto en `media/producto/`. Son las referencias que Codex va a usar después. Bájalas con `curl -L -o media/producto/foto1.jpg "<url>"`.
3. Escribe `PRODUCTO.md` con la plantilla [templates/PRODUCTO.md](templates/PRODUCTO.md): qué es, material, medidas o tallas, cómo funciona, para quién es, precio y costo, y la tabla de **claims: ✅ se puede decir / ❌ no se puede decir** (nada de "cura", "elimina" ni "garantizado" sin respaldo).
4. Si no puedes ver el producto, **pregunta antes de buscar**.

## 2. Buscar anuncios que ya funcionan
Busca con el nombre local del producto, **con el nombre en inglés del listing de AliExpress** (es el que destapa las redes globales de vendedores) y con las palabras del problema que resuelve.

**a) Meta Ad Library.**
- Con conector: `ads_library_search` con `search_terms` y el país, o `ALL`. Del primer resultado saca el `page_id` y vuelve a consultar con `page_ids` para traer la biblioteca completa de ese anunciante.
- Sin conector: abre en el navegador
  `https://www.facebook.com/ads/library/?active_status=active&ad_type=all&country=<CO|MX|ALL>&media_type=all&search_type=keyword_unordered&q=<términos>`
  haz scroll 5–8 veces y extrae de cada tarjeta el texto, "En circulación desde" y el ID de la biblioteca. Detalle en [references/tecnica-navegador.md](references/tecnica-navegador.md).

**b) TikTok** (sin login): `https://www.tiktok.com/search/video?q=<términos>`. Haz scroll lento y extrae los links de video con sus likes.

**c) AliExpress:** `https://www.aliexpress.com/w/wholesale-<términos>.html`. Sirve para identificar el producto exacto y su nombre en inglés. Los videos de la galería del proveedor se pueden usar como material propio.

**d) Mercado Libre del país:** `https://listado.mercadolibre.com.co/<términos-con-guiones>` (cambia `.co` por `.com.mx`, `.cl` o `.com.ar` según el país). Anota los precios mínimo, típico y máximo: la oferta se fija contra eso.

### Cómo saber si un anuncio "funciona"
A cada referencia le pones una etiqueta de evidencia. Nunca presentes un INDICIO como si fuera PROBADO.
| Etiqueta | Cuándo |
|---|---|
| `PROBADO` | Más de 90 días activo, o relanzado después de una pausa |
| `INDICIO` | Muchos anuncios o muchas tiendas con el mismo creativo, pero con poco tiempo |
| `SIN PROBAR` | Recién lanzado: sirve para vigilar, no para copiar |

**Señal fuerte:** cuando el mismo video o copy aparece en varias tiendas ("N anuncios usan este contenido"), es el mismo producto revendido y ese corte está vendiendo.

**e) Formato de cada referencia + radar de formatos nuevos.** A cada referencia le anotas también su **formato**, con el nombre del catálogo de [references/formatos.md](references/formatos.md): UGC, unboxing, nota del iPhone, mito vs. realidad… Además, cada investigación corre un **radar**: busca 5 anuncios PROBADOS de **otras categorías** de contraentrega (hogar, belleza, mascotas, juguetes) con "pago contra entrega" y anota solo su formato. Si una referencia usa una estructura repetible que no está en el catálogo, es un **formato nuevo**: se agrega a `FORMATOS.md` → "🆕 Formatos descubiertos" y se le avisa al usuario. Procedimiento completo y puntos: [references/seleccion-formatos.md](references/seleccion-formatos.md), Pasos 0 y 1.

## 3. Entregar los links (siempre en el chat)
Por cada referencia: anunciante · gancho (primera frase) · **formato** · etiqueta de evidencia · link.
- **Un anuncio:** link filtrado por una frase única de su texto, sin tildes:
  `https://www.facebook.com/ads/library/?active_status=active&ad_type=all&country=ALL&media_type=all&search_type=keyword_unordered&q=<frase>`
- **Toda la biblioteca de una marca:**
  `https://www.facebook.com/ads/library/?active_status=active&ad_type=all&country=ALL&media_type=all&search_type=page&view_all_page_id=<page_id>`

Guarda todo en `REFERENCIAS.md`.
⚠️ **Los videos e imágenes de terceros son referencia de estructura, no material para pautar** (derechos de autor y rechazo de Meta). Solo se usa material propio, del proveedor o generado.

## 4. Ángulos (método RECON)
Un **ángulo** es una razón concreta por la que alguien compra. Saca **4 ángulos base**, uno de cada tipo:
| Tipo | Pregunta | Ejemplo (plantilla ortopédica) |
|---|---|---|
| **Dolor** | ¿Qué problema le quita? | "Me dolía la cadera al final del día" |
| **Identidad** | ¿Quién es el cliente? | "Mamá que cuida la pisada de su hijo" |
| **Oportunidad** | ¿Qué logra? | "Terminar el día de pie sin dolor" |
| **Autoridad** | ¿Por qué creerle? | "TPR firme que no se aplasta: presiónala y mira" |

Cada ángulo sale de lo que encontraste: los dolores que repiten los anuncios PROBADOS, los comentarios y las reseñas de AliExpress o Mercado Libre. Por cada ángulo escribe **2 ganchos** distintos, usando tipos diferentes del catálogo de ganchos en [references/formatos.md](references/formatos.md) (pregunta, primera persona, dato, mito, reverso…).

## 5. Matriz ángulo × formato (el entregable que importa)
Regla madre: **un anuncio = un solo ángulo**, y cada ángulo se dice en varios formatos.
- Mínimo **4 ángulos × 3 formatos = 12 anuncios**.
- ⚠️ **El formato lo elige la investigación, no la costumbre.** No uses siempre los mismos (nota del iPhone, WhatsApp, Google…): elige con [references/seleccion-formatos.md](references/seleccion-formatos.md).
  1. Arma la tabla **"Formatos del mercado"** con los formatos de tus referencias y sus puntos de evidencia (PROBADO = 3 · INDICIO = 2 · SIN PROBAR = 1).
  2. **Reglas:**
     - al menos la mitad de los formatos tiene que venir de esa tabla;
     - el formato más fuerte del mercado va siempre;
     - un formato no se repite en dos ángulos;
     - como máximo 4 formatos repetidos del producto anterior (lee `REGISTRO-FORMATOS.md`; plantilla en `templates/`);
     - incluye 1 formato comodín nunca probado.
  3. Por cada ángulo, mezcla familias distintas: **nativo** (no parece anuncio), **de argumento** (vs., mito, bullets…) y **de oferta** (pack, precio por día).
  4. Al menos una pieza tiene que ser **TOFU**: educativa, que habla del problema sin vender al inicio.
- Formatos disponibles: el catálogo [references/formatos.md](references/formatos.md) **más los que descubra el radar** (sección 2e).

Escribe `MATRIZ.md` con la plantilla [templates/MATRIZ.md](templates/MATRIZ.md). Cada fila es un **brief completo** que `estaticos-codex` puede producir sin preguntar nada:
- **ID:** `PRODUCTO_ANGULO_FORMATO_GANCHO_v1`, por ejemplo `P360_DOLOR_NOTAIPHONE_H1_v1`.
- **Etapa:** TOFU, MOFU o BOFU.
- **Titular** (gancho, máximo 8 palabras), **texto de apoyo** (máximo 20 palabras) y **CTA**.
- **Oferta real:** precio y pack. Nunca un precio "antes" inventado.
- **Escena / composición:** qué se ve y dónde va el producto.
- **Foto del producto** que se usa como referencia (de `media/producto/`).
- **Referencia:** el anuncio de la competencia en el que se inspira (link).
- **Por qué este formato:** la referencia y los puntos de la tabla "Formatos del mercado".
- **Copy para Meta:** texto principal (máximo 3 líneas), título y descripción.

## 6. Cierre
Entrega en el chat:
1. Resumen en 5 líneas: qué es el producto, quién lo compra, precio de mercado y la oferta recomendada.
2. Las 5–10 mejores referencias con sus links.
3. La tabla "Formatos del mercado" y la de la matriz (ID · ángulo · formato · titular · por qué este formato).
3b. Los **formatos nuevos** que encontró el radar, con su referencia, o "radar sin formatos nuevos".
4. Siguiente paso: **"crea los estáticos de la matriz"** (skill `estaticos-codex`).

Archivos que deben quedar en `~/mis-creativos/<producto>/`:
- `PRODUCTO.md`
- `REFERENCIAS.md`
- `MATRIZ.md`
- `REGISTRO-FORMATOS.md` en `~/mis-creativos/` (uno para todos los productos; se actualiza al publicar)
- `media/producto/` (fotos reales)
- `media/referencias/` (opcional)

## Reglas
- Nada de claims de salud ni de resultados sin respaldo. Las reseñas, la escasez y las estadísticas salen de datos reales del producto o de la operación.
- No se publica nada en Meta ni en Shopify sin el visto bueno explícito del usuario.
- Si una búsqueda no encuentra nada útil, dilo. No rellenes.
