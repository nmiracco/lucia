# Portfolio OBRERO — Lucía Maisano

Sitio estático de una sola página (artista visual). Idioma del contenido: español.

## Estructura

```
.
├── index.html      # toda la página (HTML + CSS + JS inline, sin build)
├── favicon.png
├── CNAME           # luciamaisano.ar (legado de GitHub Pages, ya no se usa)
└── CLAUDE.md
```

No hay package.json, no hay build, no hay dependencias. Es un solo archivo.

## Hosting y dominio

El sitio está hosteado en **Cloudflare Pages**, no en GitHub Pages.

- **Cuenta Cloudflare:** Lulimaisano@gmail.com
- **Proyecto Pages:** `luxxx` (URL interna: `luxxx.pages.dev`)
- **Repo conectado:** `nmiracco/lucia`, rama `main`
- **Build:** sin build command, output `/`
- **Dominio custom:** `luciamaisano.ar` y `www.luciamaisano.ar`, ambos con SSL
- **DNS (en Cloudflare):** dos CNAMEs `Proxied` (nube **naranja**) apuntando a `luxxx.pages.dev`

**Importante:** GitHub Pages está activo en este repo por residuo histórico (publica en `nmiracco.github.io/lucia/`), pero el dominio NO mira ahí. **No proponer migrar a GitHub Pages** — agregaría complejidad sin beneficio. El hosting real es Cloudflare Pages.

Para Cloudflare Pages con dominio en la misma zone, los CNAMEs van **Proxied (naranja)** — al revés que GitHub Pages, donde irían DNS-only (gris). No confundir.

## Workflow para cambiar el sitio

1. Editar `index.html` localmente.
2. `git add index.html && git commit -m "..." && git push origin main`.
3. Cloudflare Pages republica solo en <1 minuto.
4. Verificar en `https://luciamaisano.ar`.

No hay que tocar Cloudflare manualmente para deploys — el push dispara todo.

## Trabajar con bundles de Claude Design

El usuario itera el diseño en `claude.ai/design` y a veces pasa un comando tipo:

> Fetch this design file, read its readme, and implement the relevant aspects of the design. https://api.anthropic.com/v1/design/h/...?open_file=index.html

Esos URLs devuelven un **tarball gzipped** (no HTML legible). Pasos para procesarlo:

1. `WebFetch` la URL — el resultado se guarda como binario en el cache de tool results.
2. `gunzip` + `tar -xf` el archivo.
3. La estructura típica es: `<bundle-name>/project/index.html` + `chats/chat1.md` + `uploads/*.png`.
4. Leer el `chat1.md` para entender intención del usuario y dónde quedaron las iteraciones.
5. **Las imágenes en `uploads/` son screenshots de referencia que el usuario le pasó al diseñador, NO assets que la página consume.** No copiar al repo a menos que el `index.html` las referencie por src.
6. Reemplazar `index.html` del repo con el del bundle. Restaurar `<link rel="icon" type="image/png" href="favicon.png">` en `<head>` si el bundle no lo tiene.
7. Commit + push.

## Convenciones del HTML

- Tipografías de Google Fonts (Archivo Black, Inter, IBM Plex Mono, Instrument Serif, Fraunces, etc.).
- Paleta papel/tinta (`--paper:#f4efe6`, `--ink:#0b0b0b`).
- Variables CSS en `:root` controlables desde un panel de **Tweaks** (visible al final del HTML, con persistencia en localStorage).
- Lenguaje editorial: nav minimalista (`PROYECTOS · SOBRE MÍ · CONTACTO`), OBRERO ultra-bold a gran escala, coma serif itálica entre OBRERO y "Lucía Maisano".

## Restos sin uso

- **GitHub Pages:** activo pero ignorado. Si en el futuro estorba, se puede deshabilitar desde Settings → Pages del repo.
- **Proyecto Cloudflare Pages `lux`** (`lux-9jx.pages.dev`): huérfano (su repo `wsdsddw22sd/lux` fue borrado de GitHub). Sin dominios y sin repo conectado. Se puede borrar cuando se quiera desde Workers & Pages → lux → Settings → Delete project.
