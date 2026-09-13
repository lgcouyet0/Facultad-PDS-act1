# CLAUDE.md — Convenciones y decisiones del proyecto

## Descripción general

Sitio estático de accesibilidad para un emprendimiento de artesanías. **Restricción dura: solo HTML y CSS. Cero JavaScript en ningún archivo.**

## Convenciones de nombres

- **Slug**: kebab-case ASCII puro, sin tildes ni ñ. Ej: `ruana-de-lana-merino`, `jarro-azul-mediano`. Mismo slug en `.png`, `.pdf` y `.md` de un artículo.
- **Carpetas de categoría**: minúsculas sin tilde en filesystem (`tejidos`, `ceramicos`, `pinturas`), aunque el nombre visible en HTML lleve tilde ("Cerámicos").
- **Clases CSS**: BEM simplificado, en español. Ej: `.tarjeta-articulo`, `.tarjeta-articulo__descripcion`, `.tarjeta-articulo__imagen`.
- **IDs HTML**: kebab-case español. Ej: `#navegacion-principal`, `#opcion-color-protanopia`, `#h-presentacion`.

## Regla de sincronía obligatoria

El atributo `alt=""` de cada `<img>` de artículo **debe coincidir carácter por carácter** con el campo `texto-alternativo` de su ficha `.md` en `contenido/fichas/<categoria>/<slug>.md`.

Cuando se reemplaza una imagen, revisar AMBOS lugares. Si uno diverge del otro, es un error.

## Estilos

`estilosIA.css` es el **único archivo de estilos** del proyecto. No crear otros `.css`, no usar `<style>` en el head ni `style=""` inline a menos que haya justificación explícita registrada (no hay ningún caso actual).

## Invariantes de CSS (no deben romperse)

- Nunca `order`, `flex-direction: row-reverse`, ni `grid-auto-flow: dense`. El orden visual del DOM debe coincidir con el orden de lectura lógico.
- Nunca `font-size` en píxeles en `html`. Queda al 100%, hereda del navegador.
- Nunca `maximum-scale` ni `user-scalable=no` en el `<meta name="viewport">`.
- Nunca `outline: none` sin reemplazo visible equivalente. El foco debe ser siempre visible.
- Nunca transmitir información **solo por color**. Acompañar con texto o forma.
- Nunca anchos fijos en píxeles. Usar `rem`, `em`, `ch`, `%`, `fr`, `min()`, `minmax()`.
- `font-size` se declara **solo** en `.lienzo`, encabezados y elementos de texto hoja (párrafos, listas). Nunca en contenedores intermedios (tarjetas, secciones, etc.).
- Sin CSS nativo `@supports` o `@layer`, sin container queries, sin `:has()`, sin `@media (prefers-color-scheme)` sola (sí incluída en media queries de tema). Por compatibilidad con W3C CSS Validator.
- Foco con `:focus` (no solo `:focus-visible`). Algunos lectores de pantalla no soportan `:focus-visible`.
- Nunca `position: fixed` ni `sticky` en ningún selector. Rompen con el `filter` aplicado a `.lienzo` (para el filtro SVG de daltonismo).

## Mecanismo de accesibilidad sin JavaScript

Los 7 `<input type="radio">` sueltos como hermanos de `.lienzo` al principio del `<body>` forman un **radio-hack CSS puro**:

```html
<input type="radio" name="ajuste-color" id="opcion-color-ninguno" class="conmutador" checked>
<input type="radio" name="ajuste-color" id="opcion-color-protanopia" class="conmutador">
<!-- ... más radios de color ... -->
<input type="radio" name="escala-texto" id="opcion-texto-normal" class="conmutador" checked>
<!-- ... más radios de tamaño ... -->
<div class="lienzo">
  <!-- contenido -->
</div>
```

Los inputs están **ocultos visualmente** pero **focusables**, y las etiquetas actúan como botones visuales. Los selectores `#id:checked ~ .lienzo { ... }` aplican:
- Un `filter: url(#filtro-svg-<tipo>)` que apunta a un `<filter>` SVG definido inline en cada página (con matrices de corrección de color).
- Una custom property `--escala-texto` que controla el tamaño heredado en `.lienzo`.

**Limitación aceptada**: el estado elegido no persiste entre páginas porque no hay JavaScript ni cookies. Está documentado en `accesibilidad.html`.

## Patrón de tarjeta de artículo

Cada `.tarjeta-articulo` contiene un único elemento interactivo: `<a class="tarjeta-articulo__enlace">` que envuelve íntegramente la tarjeta. La estructura interna es:

```html
<a class="tarjeta-articulo__enlace" href="articulos/categoria/slug.pdf" type="application/pdf" download>
  <h3 class="tarjeta-articulo__titulo">Nombre del artículo</h3>
  <img class="tarjeta-articulo__imagen" alt="Descripción detallada (sincronizada con ficha .md)" src="...">
  <p class="tarjeta-articulo__descripcion">Presentación breve</p>
  <p class="tarjeta-articulo__acciones"><span class="enlace-info">Ver ficha en PDF</span></p>
</a>
```

**Convenciones obligatorias:**
- El único `<a>` de la tarjeta es el que la envuelve completamente: `href`, `type="application/pdf"` y `download` van en ese elemento.
- `.enlace-info` es un `<span>` **decorativo sin href**. No es un segundo enlace.
- El `alt=""` de la `<img>` **sigue siendo descripción de la imagen, no del destino**, y debe coincidir carácter por carácter con `texto-alternativo` de la ficha `.md` (regla de sincronía).
- Nunca agregar un segundo elemento interactivo dentro de `.tarjeta-articulo__enlace` (botón, enlace extra, `<label>`, etc.): el navegador lo rechazaría como HTML inválido (`nested-interactive`).

Al navegar con Tab, el lector de pantalla anuncia el nombre accesible completo del enlace (título + alt de imagen + descripción + etiqueta de acción), exponiendo toda la información de la tarjeta antes de descargar el PDF.

## Elemento `aria-current="page"`

El atributo `aria-current="page"` debe estar en el enlace del nav principal **de la página actual y solo en ese**. Ej: en `index.html`, debe estar en `<a href="index.html" aria-current="page">Inicio</a>`.

Es el error más común al copiar la estructura de una página a otra. Revisar siempre.

## Generación de placeholders

Los 9 PNG (800×800) y 9 PDF placeholders se generaron con:
- **Pillow** (Python): imágenes PNG con fondos de color y texto.
- **reportlab** (Python): PDF a partir del contenido de las fichas `.md`.

Son temporales. El dueño del emprendimiento los reemplaza por imágenes reales y PDF elaborados (ver README.md).

## Decisiones ya tomadas (no reabrir sin pedido explícito)

- **Nombre de marca**: "Manos de Barro y Lana" es un nombre provisorio elegido para el bosquejo.
- **Sin precios en tarjetas**: las fichas muestran "A consultar" en el PDF, no hay precio visible en la web.
- **Datos de contacto ficticios**: email, WhatsApp y ciudad de ejemplo. Reemplazables.
- **Filtro de daltonismo es de corrección, no de simulación**: redistribuye color hacia canales distinguibles.
- **Sin formulario de contacto**: requeriría backend. El contacto es por email/WhatsApp.
- **Arquitectura multi-página**: no single-page. Garantiza order de lectura lógico por página sin anclas complejas.

## Trade-offs de accesibilidad documentados

**1. axe-core marca una violación "moderate" de `region`**: los 7 radios de accesibilidad quedan fuera de cualquier `<main>`, `<nav>` u otro landmark. Es inherente al mecanismo CSS puro (los inputs deben estar fuera de `.lienzo` para que `:checked ~ .lienzo` funcione) y no tiene mitigación sin romper el mecanismo o violar otro criterio WCAG. No afecta conformidad WCAG AA. Aceptado.

**2. Nombre accesible verboso y pérdida de selección de texto en tarjetas**: al envolver íntegramente una tarjeta de artículo en un único `<a>`, el nombre accesible del enlace queda largo (~30+ palabras) porque incluye título + alt de imagen + descripción + etiqueta de acción. Es necesario: la regla de sincronía obligatoria `alt` ↔ ficha `.md` impide dejar el `alt` vacío (técnica WCAG H2), y no hay forma de exponer esa información al navegar con Tab sin incluirla en el nombre accesible (se descartó `aria-describedby` porque el Narrador de Windows no lo anuncia sobre enlaces). Además, arrastrar el mouse dentro de la tarjeta inicia un drag del enlace (comportamiento estándar del navegador) en lugar de seleccionar texto. Es un costo necesario para que la información visual de la tarjeta sea accesible al teclado. Aceptado y documentado en `accesibilidad.html`.

## Validadores a correr después de cualquier cambio

1. **W3C Nu HTML Checker**: las 5 páginas. Esperar 0 errores.
2. **W3C CSS Validator** o `csstree-validator`: sobre `estilosIA.css`. Esperar 0 errores.
3. **axe-core**: sobre las 5 páginas. Esperar solo la violación "moderate" conocida de `region`.
4. **html-validate**: verificar no hay violaciones de semántica.
5. **Inspección manual**: confirmar que ninguna de las invariantes CSS del punto 5 se haya violado.

## Archivos clave

- `index.html`, `tejidos.html`, `ceramicos.html`, `pinturas.html`, `accesibilidad.html`: 5 páginas HTML con estructura idéntica en encabezado/footer.
- `estilosIA.css`: único archivo de estilos.
- `favicon.svg`: ícono del sitio.
- `articulos/<categoria>/<slug>.{png,pdf}`: 9 artículos (3 por categoría).
- `contenido/fichas/<categoria>/<slug>.md`: 9 fichas de contenido.
- `README.md`: instrucciones para los dueños.
- `CLAUDE.md`: este archivo — referencia técnica para futuras sesiones.
