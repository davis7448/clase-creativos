# Plantillas HTML (modos 2 y 3)

Se usan con `setContent` + `screenshot` en Playwright (ver SKILL.md §4). Lienzo: 1080×1350.
- Las imágenes se cargan desde `http://localhost:8777/...`, es decir, la carpeta del producto servida con `npx -y http-server <carpeta> -p 8777 -s`.
- Se reemplaza todo lo que está entre `{{ }}`.
- La fuente base es Inter (Google Fonts) y siempre se espera a `document.fonts.ready` antes de la captura.

Cabecera común (va al inicio de cada plantilla):
```html
<html><head><meta charset="utf-8">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700;800&display=swap" rel="stylesheet">
<style>*{box-sizing:border-box}body{margin:0;width:1080px;height:1350px;overflow:hidden;font-family:Inter,-apple-system,sans-serif;position:relative}</style>
```

## 1. Nota del iPhone (nativo, TOFU)
```html
<style>
body{background:#fff}
.top{height:110px;display:flex;align-items:center;justify-content:space-between;padding:0 50px;color:#E0A100;font-size:38px;font-weight:500}
.n{padding:10px 70px}
.n h1{font-size:62px;font-weight:800;margin:10px 0 34px;color:#111}
.d{color:#8a8a8e;font-size:30px;margin-bottom:28px}
.i{display:flex;gap:26px;align-items:flex-start;font-size:44px;line-height:1.3;color:#111;margin:0 0 26px}
.c{flex:0 0 46px;height:46px;border-radius:50%;border:3px solid #c7c7cc;margin-top:4px}
.c.ok{background:#E0A100;border-color:#E0A100}
.i.ok span{color:#8a8a8e;text-decoration:line-through}
.ph{position:absolute;right:60px;bottom:60px;width:430px;height:430px;object-fit:cover;border-radius:28px;box-shadow:0 10px 30px rgba(0,0,0,.15)}
</style></head><body>
<div class="top"><span>‹ Notas</span><span>Listo</span></div>
<div class="n"><div class="d">{{fecha, ej. 15 de octubre de 2026}}</div>
<h1>{{titular, ej. Cosas que me quitaron el dolor de pies}}</h1>
<div class="i ok"><div class="c ok"></div><span>{{lo que no funcionó 1}}</span></div>
<div class="i ok"><div class="c ok"></div><span>{{lo que no funcionó 2}}</span></div>
<div class="i"><div class="c"></div><span><b>{{el producto}}</b> {{lo que sí funcionó}} ✨</span></div>
</div>
<img class="ph" src="http://localhost:8777/media/producto/{{foto}}">
</body></html>
```

## 2. Chat de WhatsApp (nativo, MOFU)
```html
<style>
body{background:#efe7dd}
.h{height:130px;background:#075E54;color:#fff;display:flex;align-items:center;gap:24px;padding:0 36px;font-size:38px;font-weight:700}
.av{width:78px;height:78px;border-radius:50%;background:#cfd8dc}
.h small{display:block;font-size:26px;font-weight:400;opacity:.85}
.chat{padding:36px 40px;display:flex;flex-direction:column;gap:22px}
.m{max-width:760px;padding:20px 28px;border-radius:22px;font-size:38px;line-height:1.3;color:#111;box-shadow:0 1px 2px rgba(0,0,0,.15)}
.r{align-self:flex-start;background:#fff;border-top-left-radius:6px}
.s{align-self:flex-end;background:#dcf8c6;border-top-right-radius:6px}
.m img{display:block;width:100%;max-height:380px;object-fit:cover;border-radius:14px;margin-bottom:12px}
.t{font-size:24px;color:#667781;text-align:right;margin-top:6px}
</style></head><body>
<div class="h"><div class="av"></div><div>{{nombre, ej. Caro 💛}}<small>en línea</small></div></div>
<div class="chat">
<div class="m r">{{mensaje 1, ej. Amiga, ¿qué te pusiste que ya no te quejas de los pies?}}<div class="t">7:42 p. m.</div></div>
<div class="m s"><img src="http://localhost:8777/media/producto/{{foto}}">{{respuesta con el producto}}<div class="t">7:43 p. m. ✓✓</div></div>
<div class="m r">{{mensaje 3, ej. ¿Y dónde la pido? 😍}}<div class="t">7:43 p. m.</div></div>
<div class="m s">{{respuesta con la oferta real, ej. Te llega a la casa y pagas al recibir 🙌}}<div class="t">7:44 p. m. ✓✓</div></div>
</div></body></html>
```

## 3. Búsqueda de Google (nativo, TOFU)
```html
<style>
body{background:#fff}
.logo{text-align:center;margin-top:120px;font-size:120px;font-weight:500;letter-spacing:-4px}
.logo span:nth-child(1){color:#4285F4}.logo span:nth-child(2){color:#EA4335}.logo span:nth-child(3){color:#FBBC05}.logo span:nth-child(4){color:#4285F4}.logo span:nth-child(5){color:#34A853}.logo span:nth-child(6){color:#EA4335}
.box{margin:60px 70px 0;border:2px solid #dfe1e5;border-radius:60px;padding:30px 44px;font-size:42px;color:#202124;box-shadow:0 2px 8px rgba(0,0,0,.08)}
.sug{margin:0 70px;border:2px solid #dfe1e5;border-top:0;border-radius:0 0 30px 30px;padding:10px 0}
.sug div{padding:18px 44px;font-size:36px;color:#202124}.sug b{font-weight:700}
.ph{display:block;margin:60px auto 0;width:520px;height:520px;object-fit:cover;border-radius:24px}
</style></head><body>
<div class="logo"><span>G</span><span>o</span><span>o</span><span>g</span><span>l</span><span>e</span></div>
<div class="box">🔍 {{búsqueda del dolor, ej. por qué me duelen los pies al final del día}}</div>
<div class="sug"><div>{{sugerencia 1}}</div><div>{{sugerencia 2}}</div><div><b>{{sugerencia que lleva al producto}}</b></div></div>
<img class="ph" src="http://localhost:8777/media/producto/{{foto}}">
</body></html>
```

## 4. Ticket de compra (nativo, BOFU; con precios reales)
```html
<style>
body{background:#e9e6df;display:flex;align-items:center;justify-content:center}
.t{width:760px;background:#fff;padding:60px 56px;font-family:'Courier New',monospace;color:#222;font-size:34px;box-shadow:0 20px 40px rgba(0,0,0,.18)}
.t h2{text-align:center;font-size:44px;margin:0 0 6px}.c{text-align:center;color:#666;font-size:28px}
.l{display:flex;justify-content:space-between;margin:16px 0}.hr{border-top:3px dashed #aaa;margin:26px 0}
.tot{font-size:46px;font-weight:700}.ok{text-align:center;margin-top:30px;font-size:30px}
</style></head><body><div class="t">
<h2>{{MARCA}}</h2><div class="c">Pedido #{{número}} · {{fecha}}</div><div class="hr"></div>
<div class="l"><span>{{producto x2}}</span><span>${{precio pack}}</span></div>
<div class="l"><span>Envío</span><span>{{GRATIS}}</span></div><div class="hr"></div>
<div class="l tot"><span>TOTAL</span><span>${{precio pack}}</span></div>
<div class="ok">{{Pagas al recibir ✅}}</div></div></body></html>
```

## 5. Historia de Instagram (nativo, foto casera de Codex + texto)
```html
<style>
.bg{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.tx{position:absolute;left:50%;transform:translateX(-50%);top:{{y, ej. 180}}px;background:#fff;color:#000;font-size:46px;font-weight:700;padding:14px 26px;border-radius:12px;max-width:860px;text-align:center;line-height:1.25}
</style></head><body>
<img class="bg" src="http://localhost:8777/estaticos/raw/{{ID}}_raw.png">
<div class="tx">{{texto como lo escribiría una persona, ej. no puedo creer que esto me haya cambiado los días 😭}}</div>
</body></html>
```

## 6. Bullets de beneficios + oferta (argumento, modo 2)
```html
<style>
.bg{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.box{position:absolute;left:60px;right:60px;top:60px}
.k{display:inline-block;background:{{color acento, ej. #0B2545}};color:#fff;font-size:30px;font-weight:700;padding:10px 22px;border-radius:40px;letter-spacing:1px}
h1{font-size:78px;line-height:1.02;font-weight:800;color:#111;margin:22px 0 26px}
.b{font-size:42px;line-height:1.35;color:#222;margin:0 0 12px}.b b{color:{{color acento}}}
.of{position:absolute;left:60px;right:60px;bottom:60px;background:{{color acento}};color:#fff;border-radius:28px;padding:26px 34px;display:flex;justify-content:space-between;align-items:center}
.of .p{font-size:58px;font-weight:800}.of .s{font-size:30px;opacity:.9}
</style></head><body>
<img class="bg" src="http://localhost:8777/estaticos/raw/{{ID}}_raw.png">
<div class="box"><span class="k">{{KICKER}}</span><h1>{{titular}}</h1>
<div class="b">✅ <b>{{beneficio 1}}</b></div><div class="b">✅ <b>{{beneficio 2}}</b></div><div class="b">✅ <b>{{beneficio 3}}</b></div></div>
<div class="of"><div><div class="p">{{2 por $…}}</div><div class="s">{{Envío gratis · Pagas al recibir}}</div></div><div style="font-size:44px">👉</div></div>
</body></html>
```

## 7. Nosotros vs. ellos (argumento, modo 2)
```html
<style>
.bg{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
h1{position:absolute;top:50px;left:50px;right:50px;text-align:center;font-size:68px;font-weight:800;margin:0;color:#111}
.col{position:absolute;bottom:60px;width:470px;border-radius:24px;padding:26px;font-size:34px;line-height:1.45}
.no{left:50px;background:rgba(255,255,255,.92);color:#444}.si{right:50px;background:{{color acento}};color:#fff}
.col h3{margin:0 0 10px;font-size:40px}
</style></head><body>
<img class="bg" src="http://localhost:8777/estaticos/raw/{{ID}}_raw.png">
<h1>{{titular, ej. ¿Por qué la tuya se aplasta?}}</h1>
<div class="col no"><h3>❌ {{la alternativa}}</h3>{{defecto 1}}<br>{{defecto 2}}<br>{{defecto 3}}</div>
<div class="col si"><h3>✅ {{el producto}}</h3>{{ventaja 1}}<br>{{ventaja 2}}<br>{{ventaja 3}}</div>
</body></html>
```

## Para 9:16 (1080×1920)
Se usa la misma plantilla con `height:1920px` y el viewport de 1080×1920. Los textos se bajan 250 px y se dejan libres los 250 px de abajo (ahí van los botones de la historia).
