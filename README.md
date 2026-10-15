# clase-creativos

Skills de Claude Code para **investigar los anuncios que ya venden un producto** y **convertir esa investigación en anuncios estáticos con Codex**.

| Skill | Qué hace |
|---|---|
| `setup-clase` | Revisa la instalación (Mac o Windows), te dice qué falta y prueba Codex |
| `investigar-anuncios` | Meta Ad Library + TikTok + AliExpress + Mercado Libre → `PRODUCTO.md`, `REFERENCIAS.md` y `MATRIZ.md` (4 ángulos × 3 formatos) |
| `estaticos-codex` | `MATRIZ.md` + fotos reales → anuncios 4:5 y 9:16 con Codex y plantillas HTML (nota del iPhone, WhatsApp, Google, ticket, bullets, comparativa) |

## Instalación rápida
1. Sigue **[docs/GUIA-INSTALACION.pdf](docs/GUIA-INSTALACION.pdf)** (Mac y Windows, más los conectores de Shopify y Meta).
2. Dentro de Claude Code:
   ```
   /plugin marketplace add davis7448/clase-creativos
   /plugin install clase-creativos@clase-creativos
   ```
3. Reinicia Claude Code y escribe `/clase-creativos:setup-clase`.

## Durante la clase
Sigue **[docs/INSTRUCTIVO-CLASE.pdf](docs/INSTRUCTIVO-CLASE.pdf)**:
1. `Investiga este producto: <link>`
2. `Crea los estáticos de la matriz`

## Requisitos
Claude Pro o Max · ChatGPT Plus o superior (Codex) · Node.js · Git · ffmpeg · Google Chrome.
Opcionales: conector de Meta (`https://mcp.facebook.com/ads`) y de Shopify, en claude.ai → Configuración → Conectores.

## Reglas
- Solo fotos reales del producto como referencia para Codex.
- Los anuncios de terceros son referencia de estructura, no material para pautar.
- Nada de claims médicos ni precio "antes" inventado.
- Nada se publica sin el ok del usuario.

Licencia MIT.
