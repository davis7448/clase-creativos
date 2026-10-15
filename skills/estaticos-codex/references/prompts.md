# Plantillas de prompt para Codex (en inglés: Codex responde mejor)

Estructura de todo prompt:
**formato y medida → escena → producto (fiel a la referencia) → estilo → texto (o "no text") → nombre del archivo.**
Los textos en español van entre comillas simples, cortos y con sus tildes escritas.

Bloque fijo que va al final de **todos** los prompts:
> The product must look EXACTLY like the reference photos (same shape, color, parts, proportions, no extra parts, no logos). Photorealistic. Save/copy the generated image into the current working directory as `<ID>_raw.png`.

## Estilo según la categoría
| Categoría | Estilo |
|---|---|
| Salud / ortopedia / bienestar | Clínico y limpio: fondo blanco o gris claro, azul marino, luz suave, sin caricatura |
| Juguetes / niños | Cálido, hogar real, luz de ventana, colores vivos, niños de 4–8 años |
| Papelería / regalo | Pastel, kawaii, mesa ordenada desde arriba |
| Hogar / cocina | Lifestyle cálido, madera, plantas |
| Belleza | Macro de textura, piel real, luz de baño |

## Modo 1 — Codex completo (titular de ≤ 6 palabras)
**Problema visual (TOFU)**
```
Use your image generation tool to create a vertical 4:5 Meta ad photo. A tired woman in her 40s at the end of a work day, sitting on the edge of her bed holding her lower back, warm evening light, real home, smartphone-photo look (not studio). Big bold headline at the top in Spanish: '¿Te duele al final del día?'. No product in this image. <bloque fijo>
```

**Producto en uso (MOFU)**
```
Use your image generation tool to create a vertical 4:5 Meta ad photo. Close-up of the product from the reference photos being used correctly: <cómo se usa en la vida real>. Clean <estilo>. Headline at the top in Spanish: '<titular corto>'. <bloque fijo>
```

**Antes / después (lado a lado)**
```
Use your image generation tool to create a vertical 4:5 split-screen Meta ad. Left half labeled 'ANTES': <el problema, sin el producto>. Right half labeled 'DESPUÉS': <la misma escena resuelta con el producto de la referencia>. Same person and lighting on both sides. <bloque fijo>
```
⚠️ El "después" muestra uso y comodidad, no un resultado médico.

## Modo 2 — escena sin texto (el texto se pone en HTML)
Termina siempre con:
> No text, no letters, no numbers, no logos anywhere in the image. Leave clean empty space in the <top third / bottom third> for text.

**Fondo para bullets de beneficios**
```
Use your image generation tool to create a vertical 4:5 product photo: the product from the reference photos on a clean light-grey surface, soft shadow, placed in the bottom half, slightly angled. Top 45% of the image is empty clean background. No text... <bloque fijo>
```

**Nosotros vs. ellos (dos productos)**
```
Use your image generation tool to create a vertical 4:5 comparison photo. Left: a generic worn-out, flattened <alternativa genérica, sin marca>. Right: the product from the reference photos, brand new. Same table, same light, a thin vertical line separating them. Empty space at the top for labels. No text... <bloque fijo>
```

**Foto casera (para nativos: historia de IG, ugly ad)**
```
Use your image generation tool to create a casual smartphone photo, vertical 4:5, slightly imperfect framing, natural indoor light: <la escena con el producto de la referencia en una casa real>. It must look like a real person took it with their phone, not an ad. No text... <bloque fijo>
```

## Errores comunes y cómo corregirlos
| Problema | Corrección en el prompt |
|---|---|
| Inventa piezas o cambia el color | Agrega "Copy the product pixel-faithfully from reference image 1. Do not redesign it." y usa solo la foto más clara |
| Texto con errores | Pasa al modo 2 |
| Se ve "de stock", muy perfecto | "smartphone photo, natural imperfect light, real home, not a studio" |
| Pone una marca inventada | "no brand names, no logos, no packaging text" |
| El producto se ve chico | "the product fills at least 40% of the frame" |
| Uso incorrecto | Describe el uso paso a paso ("the insole is INSIDE the shoe, only the heel cup is visible") |
