# Instructivo de la clase — De la investigación al anuncio estático

**Antes de empezar:** tienes que haber completado la `GUIA-INSTALACION` (sección 7 con todo en ✅).
**Qué necesitas a mano:** el link del producto (AliExpress, tu tienda o el proveedor), 2–3 fotos reales del producto, el precio de venta y, si lo tienes, el costo.

---

## Parte 0 — Arrancar (5 min)
1. Abre la terminal (Mac: Terminal · Windows: PowerShell).
2. Escribe `claude` y presiona Enter.
3. Escribe: `/clase-creativos:setup-clase`. Todo tiene que salir en ✅.

## Parte 1 — Las ideas clave (15 min)
**1. Ángulo ≠ formato.**
- El **ángulo** es la razón por la que alguien compra.
- El **formato** es cómo se lo muestras.
- **Un anuncio = un solo ángulo**, y cada ángulo se prueba en varios formatos.

**2. Los 4 ángulos base (método RECON).**
| Tipo | Pregunta |
|---|---|
| Dolor | ¿Qué problema le quita? |
| Identidad | ¿Quién es el que compra? |
| Oportunidad | ¿Qué logra con el producto? |
| Autoridad | ¿Por qué creerle? |
Se prueban los 4 a la vez, con la misma audiencia y el mismo gasto, durante 5–7 días. Gana el de mejor costo por compra.

**3. Estructura de todo anuncio.**
- **Gancho:** primeros 3 segundos o la primera línea; frena el scroll.
- **Cuerpo:** problema → cómo funciona → prueba → objeción.
- **CTA:** oferta real + riesgo cero (envío gratis, pago al recibir) + instrucción ("pídelo aquí").

**4. Tres familias de estáticos.**
| Familia | Ejemplos | Para qué |
|---|---|---|
| **Nativo** (no parece anuncio) | Nota del iPhone, chat de WhatsApp, búsqueda de Google, historia de IG | Frenar el scroll y generar confianza |
| **Argumento** | Beneficios en bullets, nosotros vs. ellos, antes/después | Convencer |
| **Oferta** | Pack, precio por día, ticket de compra | Cerrar |

**5. Evidencia.** No todo lo que ves en la Ad Library funciona.
- **PROBADO:** más de 90 días activo.
- **INDICIO:** muchas tiendas con el mismo anuncio, pero recientes.
- **SIN PROBAR:** recién lanzado.

## Parte 2 — Investigar (30 min)
1. Crea la carpeta de tu producto y pon ahí las fotos. Escríbele a Claude:
   ```
   Crea la carpeta ~/mis-creativos/<nombre-producto>/media/producto y ábrela
   ```
   Arrastra tus 2–3 fotos reales a esa carpeta.
2. Lanza la investigación (cambia lo que está entre < >):
   ```
   Investiga este producto: <link>
   País: <Colombia / México / …>. Precio de venta: <$…>. Costo: <$…>.
   Las fotos están en ~/mis-creativos/<nombre-producto>/media/producto
   ```
3. Claude va a:
   - verificar el producto y escribir `PRODUCTO.md`;
   - buscar en la Ad Library, TikTok, AliExpress y Mercado Libre;
   - darte los **links** de los anuncios que funcionan, con su evidencia;
   - armar los **4 ángulos** con 2 ganchos cada uno;
   - escribir `MATRIZ.md` con **12 anuncios definidos**.
4. **Tu trabajo:** abre 3 links de la Ad Library y revisa que de verdad sean del producto. Si un ángulo no te convence, díselo:
   *"cambia el ángulo de identidad: mi cliente son enfermeras que trabajan de pie"*.

## Parte 3 — Crear los estáticos (40 min)
1. Escríbele:
   ```
   Crea los estáticos de la matriz de <nombre-producto>. Empieza por P01, P02 y P03.
   ```
2. Claude elige el modo de cada anuncio:
   - **Codex completo** para escenas con poco texto;
   - **Codex + texto exacto** para bullets, precios y comparativas;
   - **Nativo HTML** para nota del iPhone, WhatsApp o Google.
   Cada imagen de Codex tarda 1–3 minutos.
3. Revisa cada imagen con esta lista:
   - ¿El producto es **idéntico** al real?
   - ¿El texto está **sin errores**?
   - ¿El precio es el **real**?
   - ¿Se lee en el celular?
4. Para corregir, pide **una cosa a la vez**:
   *"P02: el titular más grande y la plantilla dentro del zapato, no en la mano"*.
5. Cuando estén bien: *"haz el resto de la matriz"*. Quedan en `~/mis-creativos/<producto>/estaticos/` con su copy en `ESTATICOS.md`.

## Parte 4 — Iterar (10 min)
| Lo que pasa en Meta | Qué cambias |
|---|---|
| CTR menor a 1 % (nadie hace clic) | El **gancho** o titular. La misma imagen con 3 titulares nuevos |
| Hacen clic pero no compran | La **oferta** o la landing, no el anuncio |
| Un anuncio vende con buen costo | **No lo toques.** Haz el mismo ángulo en 3–6 formatos más |
| Todo cansa después de 2–3 semanas | Ángulos nuevos: 15 creativos nuevos por semana |

Para pedirlo: *"P04 ganó. Hazme 6 titulares nuevos con la misma imagen y el mismo ángulo en formato WhatsApp, ticket y nosotros vs. ellos"*.

## Reglas de oro
1. **Solo fotos reales del producto** como referencia para Codex. Nunca imágenes de la competencia.
2. **Los anuncios de otros son referencia, no material:** no se pauta su video ni su foto.
3. **Nada de claims médicos ni resultados garantizados**, y nada de precio "antes" inventado.
4. **Nada se publica en Meta sin que tú digas que sí.**

## Si algo falla en la clase
| Problema | Qué decirle a Claude |
|---|---|
| Codex no genera | *"revisa codex login status y genera solo P01 mostrando el error"* |
| La Ad Library pide sesión | Inicia sesión en la ventana de Chrome que abrió Claude y dile *"listo, sigue"* |
| El producto sale distinto | *"regenera P03 usando solo foto1.jpg y copia el producto exacto"* |
| Texto con errores | *"pasa P03 a modo 2 (texto en HTML)"* |
| Me perdí | *"¿en qué paso vamos y qué archivos hay en mi carpeta?"* |
