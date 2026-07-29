# LuxTime — Maqueta web corporativa

Prototipo de sitio corporativo para una marca de relojería de lujo, construido **100% con HTML5 y CSS3 nativo**: sin JavaScript, sin frameworks (React, Bootstrap, etc.) y sin librerías externas. Todo el comportamiento interactivo —menú móvil, zoom, carrusel, personalizador de producto y validación de formulario— está simulado exclusivamente con CSS.

## Índice

- [Stack y restricciones](#stack-y-restricciones)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Páginas](#páginas)
- [Sistema de diseño](#sistema-de-diseño)
- [Interactividad simulada con CSS](#interactividad-simulada-con-css)
- [Cómo visualizar la maqueta](#cómo-visualizar-la-maqueta)
- [Compatibilidad de navegadores](#compatibilidad-de-navegadores)
- [Accesibilidad](#accesibilidad)
- [Guía de commits (Conventional Commits)](#guía-de-commits-conventional-commits)
- [Historial de commits sugerido](#historial-de-commits-sugerido)
- [Limitaciones conocidas y siguientes pasos](#limitaciones-conocidas-y-siguientes-pasos)

## Stack y restricciones

| | |
|---|---|
| Maquetación | CSS Grid + Flexbox (el footer usa Flexbox de forma obligatoria) |
| Responsive | Mobile-first, 3 breakpoints: `<768px` · `≥768px` (tablet) · `≥1024px` (desktop) |
| JavaScript | Ninguno |
| Dependencias externas | Ninguna (sin CDN, sin Google Fonts — solo pilas de fuentes de sistema) |
| Imágenes de producto | Ilustradas en CSS puro (ver [Sistema de diseño](#sistema-de-diseño)) — evita derechos de imagen y enlaces externos rotos |

## Estructura del proyecto

```
luxtime/
├── index.html              Inicio
├── catalogo.html           Catálogo (20 modelos)
├── producto.html           Detalle de producto (Meridian)
├── historia.html           Historia de la empresa
├── contacto.html           Contacto
├── README.md
│
├── css/
│   ├── styles.css          Global: variables, reset, tipografía, utilidades,
│   │                       header/nav, footer, grid de producto, placeholders CSS
│   ├── home.css            Hero, resumen de historia, apoyo visual
│   ├── catalogo.css        Ajustes propios del grid de 20 modelos
│   ├── producto.css        Zoom, ficha técnica, personalizador, carrusel
│   ├── historia.css        Línea de tiempo y galería mosaico
│   └── contacto.css        Formulario y validación
│
├── docs/                   Documentos de proceso (no forman parte del sitio final)
│   ├── wireframe-propuesta.html      Wireframe mobile/tablet/desktop (Paso 1)
│   ├── guia-estilos.html             Style tile de validación (Paso 2)
│   └── componentes-nav-footer.html   Preview aislado de header/footer (Paso 3)
│
└── assets/
    ├── img/{hero,productos,historia}/  Vacías — reservadas para fotografía real
    └── icons/                          Vacía
```

> `styles.css` concentra todo lo reutilizable (variables, reset, header/nav, footer, grid de producto, tarjetas, badges y los placeholders `watch-plate`/`photo-plate`). Cada página solo añade en su propio `.css` lo que le es exclusivo. Esto evita duplicar CSS entre plantillas, aunque el HTML del header y el footer sí se repite igual en cada `.html`, por ser HTML estático sin motor de includes.

## Páginas

| Página | Contenido clave |
|---|---|
| `index.html` | Hero rotativo (3 slides), 6 relojes destacados con rating, resumen de historia, testimonios y video mock |
| `catalogo.html` | Grid de 20 modelos con nombre, precio y etiqueta de estado (sin filtros ni buscador) |
| `producto.html` | Imagen con zoom en hover, ficha técnica, personalizador de correa/grabado, carrusel de relacionados |
| `historia.html` | Línea de tiempo interactiva (7 hitos + 1 meta a futuro) y galería mosaico |
| `contacto.html` | Formulario con validación CSS nativa + información de contacto y redes |

## Sistema de diseño

- **Paleta** — inspirada en materiales de relojería fina, no en el cliché "lujo = negro y dorado": `--c-paper` (esfera plateada/argenté), `--c-ink` (placa de movimiento), `--c-gold` (oro/latón de la caja) y `--c-blued` (acero azulado de las agujas, el acento distintivo del proyecto).
- **Tipografía** — Georgia (display serif) + Optima (texto) + monoespaciada de sistema (precios, referencias, datos técnicos). Solo pilas de sistema: cero dependencias externas.
- **Firma visual** — `.rule-ticks`, una "regla de minutos" (`repeating-linear-gradient`) inspirada en el bisel de un reloj, reutilizada como divisor y como subrayado de navegación.
- **Placeholders de producto** — `.watch-plate` (esfera de reloj dibujada con `conic-gradient` y manecillas) y `.photo-plate` (textura de archivo/heritage), en vez de fotografía real.
- Ver `docs/guia-estilos.html` para la muestra visual completa.

## Interactividad simulada con CSS

| Comportamiento | Técnica CSS |
|---|---|
| Menú móvil | *Checkbox hack*: `<input type="checkbox">` oculto + `:checked ~` |
| Rotación del hero | `@keyframes` con `opacity`/`transform` y `animation-delay` escalonado |
| Zoom de producto | `:hover` / `:focus-within` + `transform: scale()` |
| Carrusel de relacionados | `overflow-x: auto` + `scroll-snap-type: x mandatory` |
| Personalizador (correa/grabado) | Radios ocultos + `label` + selector de hermanos (`~`), mismo patrón que unas "tabs" sin JS |
| Línea de tiempo interactiva | `:hover` / `:focus` revelan detalle adicional con `max-height` + `opacity` |
| Validación de formulario | `:required`, `:focus`, `:user-invalid` (con fallback `:invalid:not(:placeholder-shown)`), `:valid` |

## Cómo visualizar la maqueta

No requiere instalación ni build. Dos opciones:

**A. Abrir directamente**
Haz doble clic en `index.html` (o cualquier otra página) y navega con el menú. Al no usar `fetch`/JS, funciona igual vía `file://` que en un servidor.

**B. Servidor local (recomendado)**
```bash
cd luxtime
python3 -m http.server 8000
# abrir http://localhost:8000
```
También sirve la extensión "Live Server" de VS Code, o `npx serve .` si tienes Node instalado.

## Compatibilidad de navegadores

Construido con CSS moderno: `clamp()`, `aspect-ratio`, `conic-gradient`, `scroll-snap`, `:focus-visible` y `:user-invalid`. Todas las versiones recientes de Chrome, Edge, Firefox y Safari lo soportan; para `:user-invalid` se incluyó `:invalid:not(:placeholder-shown)` como fallback en navegadores algo más antiguos.

## Accesibilidad

- `aria-current="page"` en el enlace activo de la navegación.
- Textos ocultos (`.visually-hidden`) para controles que solo muestran un icono (menú, swatches de correa).
- Estados de foco visibles y con buen contraste (`:focus-visible`, color `--c-gold`/`--c-blued`).
- El menú móvil, el personalizador y la validación del formulario son 100% operables por teclado (elementos nativos `<input>`/`<label>`, sin trampas de foco).
- Los paneles de "revelar al pasar el cursor" (línea de tiempo, mosaico) también responden a `:focus` mediante `tabindex="0"`, no solo a `:hover`.
- Se respeta `prefers-reduced-motion` desactivando animaciones y transiciones.

## Guía de commits (Conventional Commits)

| Tipo | Uso |
|---|---|
| `feat` | Nueva funcionalidad o sección visible (una página, un componente) |
| `fix` | Corrección de un error (ej. un atributo mal usado) |
| `style` | Cambios de estilo que no alteran la estructura/lógica (ajustes visuales puntuales) |
| `refactor` | Reorganización de código sin cambiar el resultado visual (extraer una utilidad, mover archivos) |
| `docs` | Documentación (README, wireframes, guías) |
| `chore` | Tareas de mantenimiento (estructura de carpetas, configuración) |

Formato: `tipo: descripción breve en minúsculas, en infinitivo o presente`.

## Historial de commits sugerido

Orden cronológico recomendado si vas a inicializar el repositorio ahora, agrupado por lo construido en cada paso:

```bash
git init

# Paso 1 — planificación
git commit -m "chore: crear estructura inicial de carpetas del proyecto"
git commit -m "docs: agregar propuesta de wireframe mobile/tablet/desktop"

# Paso 2 — sistema de diseño global
git commit -m "feat: agregar variables globales de diseño (paleta, tipografía, espaciado)"
git commit -m "style: implementar reset y estilos tipográficos base"
git commit -m "feat: agregar utilidades reutilizables (contenedor, botones, regla de minutos)"
git commit -m "docs: agregar guía de estilos de validación"

# Paso 3 — header, nav y footer
git commit -m "feat: implementar header y navegación con menú móvil sin JS (checkbox hack)"
git commit -m "feat: implementar footer reutilizable con Flexbox"
git commit -m "style: añadir subrayado de navegación con la firma regla de minutos"
git commit -m "docs: agregar vista previa de componentes header/footer"

# Paso 4 — páginas
git commit -m "feat: agregar placeholder ilustrado de reloj y foto en CSS puro"
git commit -m "feat: agregar sistema de grid de producto, tarjetas y rating con estrellas"
git commit -m "feat: maquetar pagina de Inicio (hero, destacados, historia, apoyo visual)"

git commit -m "refactor: extraer utilidad .section-pad y quitar estilos inline"
git commit -m "style: anclar badge de estado en la esquina de la tarjeta (badge--pin)"
git commit -m "feat: maquetar pagina de Catalogo con grid de 20 modelos"

git commit -m "feat: maquetar detalle de producto con imagen zoom y ficha tecnica"
git commit -m "feat: implementar personalizador de correa y grabado sin JavaScript"
git commit -m "feat: implementar carrusel horizontal de relacionados con scroll-snap"

git commit -m "feat: implementar linea de tiempo interactiva con CSS (hover y focus)"
git commit -m "feat: implementar galeria mosaico con CSS Grid auto-fit"

git commit -m "feat: maquetar formulario de contacto con validacion CSS nativa"
git commit -m "fix: quitar atributo novalidate mal usado que desactivaba la validacion"
git commit -m "feat: agregar informacion de contacto e info de redes sociales"

# Paso 5 — documentación final
git commit -m "chore: reorganizar documentos de proceso dentro de docs/"
git commit -m "docs: agregar README con planificacion y guia de commits"
```

## Limitaciones conocidas y siguientes pasos

- **Sin backend**: el formulario de contacto valida en el navegador pero no envía datos a ningún servidor.
- **Sin fotografía real**: `watch-plate` y `photo-plate` sustituyen imágenes de producto reales; al incorporarlas, sustituir esos bloques por `<img>` optimizadas dentro de `assets/img/`.
- **Header y footer duplicados**: al no usar un generador de sitios estáticos ni SSR, ese bloque se repite igual en las 5 páginas. Si el sitio crece, valorar un SSG (Eleventy, Astro) para centralizarlo sin añadir un framework de frontend.
- **Un solo template de producto**: las 20 fichas del catálogo enlazan hoy al catálogo general; en producción cada modelo necesitaría su propia página o enrutamiento dinámico.
