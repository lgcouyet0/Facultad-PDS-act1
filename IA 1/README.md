# Manos de Barro y Lana — Sitio estático de artesanías

## Qué es esto

Este es un sitio web de bosquejo para un emprendimiento de artesanías. Está hecho con HTML y CSS puro, sin JavaScript, sin frameworks ni dependencias externas. Incluye funciones de accesibilidad integradas en CSS para que cualquier persona pueda navegar el catálogo sin barreras.

## Cómo abrir el sitio

Hacé doble clic en `index.html` en la carpeta raíz. Se abrirá en tu navegador de forma inmediata, sin necesidad de servidor ni instalación.

Para probar que los enlaces y rutas funcionan exactamente como en un sitio publicado, podés usar una extensión tipo "Live Server" en tu editor de código, o ejecutar desde la terminal:

```bash
python -m http.server 8000
```

y luego entrar a `http://localhost:8000` en el navegador. Pero para ver el sitio, no es obligatorio.

## Estructura de carpetas

```
C:\...\ IA 1\
├── index.html                    (página de inicio)
├── tejidos.html
├── ceramicos.html
├── pinturas.html
├── accesibilidad.html            (declaración de accesibilidad)
├── estilosIA.css                 (estilos únicos del proyecto)
├── favicon.svg
├── articulos/
│   ├── tejidos/
│   │   ├── ruana-de-lana-merino.png
│   │   ├── ruana-de-lana-merino.pdf
│   │   ├── chal-calado-a-crochet.png
│   │   ├── chal-calado-a-crochet.pdf
│   │   ├── manta-en-telar-de-algodon.png
│   │   └── manta-en-telar-de-algodon.pdf
│   ├── ceramicos/
│   │   ├── <slug>.png, <slug>.pdf (3 artículos)
│   └── pinturas/
│       ├── <slug>.png, <slug>.pdf (3 artículos)
└── contenido/
    └── fichas/
        ├── tejidos/
        │   └── <slug>.md (3 fichas)
        ├── ceramicos/
        │   └── <slug>.md (3 fichas)
        └── pinturas/
            └── <slug>.md (3 fichas)
```

Cada artículo tiene un **slug** en kebab-case (minúscula, sin tildes, sin espacios): ese mismo slug se usa como nombre de archivo en el `.png`, el `.pdf` y el `.md`.

## Reemplazar imagen de un artículo

Querés cambiar una foto placeholder por una real:

1. Conseguí la foto.
2. Recortala a proporción cuadrada (1:1) si es posible, para que se vea bien en la tarjeta.
3. Guardala dentro de `articulos/<categoria>/` con el MISMO nombre exacto de archivo que la que estás reemplazando.
   Por ejemplo, si vas a cambiar la foto de `ruana-de-lana-merino.png`, guardá la nueva foto exactamente con ese nombre, sobrescribiendo la anterior.
4. **Importante**: si la foto muestra algo distinto a la descripción actual del artículo, actualizá el texto alternativo en DOS lugares:
   - En el archivo `.md` correspondiente (`contenido/fichas/<categoria>/<slug>.md`), en el campo `texto-alternativo`.
   - En el atributo `alt=""` de la etiqueta `<img>` en el archivo `.html` de esa categoría.
   - Estos dos textos deben coincidir carácter por carácter.

## Reemplazar o regenerar PDF

Los PDF actuales son placeholders generados automáticamente a partir de cada ficha `.md`. Son temporales.

Podés reemplazarlos por una versión más elaborada (hecha en Word, Canva, LibreOffice, etc.) siempre que:
- La guardes con el MISMO nombre de archivo en `articulos/<categoria>/`, sobrescribiendo el placeholder.
- Por accesibilidad: el PDF debe contener texto real seleccionable (no una imagen escaneada), y es recomendable configurar en el documento un "Título del documento" (metadata) igual al nombre del artículo, para que los lectores de pantalla lo anuncien correctamente.

## Agregar un artículo nuevo

Para agregar un artículo a una categoría existente:

1. Decidí un **slug** (nombre de archivo): minúscula, sin espacios, sin tildes, con guiones para separar palabras. Ej: `jarro-azul-mediano`.
2. Creá o conseguí la imagen (PNG, cuadrada 1:1) y el PDF, y guardalos en `articulos/<categoria>/` con ese slug exacto.
3. Creá un archivo `.md` nuevo en `contenido/fichas/<categoria>/` con el mismo slug, copiando la estructura de uno existente y actualizando los datos (título, descripción, materiales, medidas, técnica, cuidados, etc.). El campo `texto-alternativo` debe ser conciso, descriptivo e igual al `alt=""` de la imagen en el HTML.
4. Abrí el archivo `.html` de esa categoría y agregá una tarjeta nueva copiando el bloque `<li><article class="tarjeta-articulo">...</article></li>` de otro artículo y actualizando:
   - El nombre del artículo (dentro de `<h3>`).
   - El `src` de la imagen (`articulos/<categoria>/<tu-slug>.png`).
   - El `alt` de la imagen (debe coincidir con el campo `texto-alternativo` del `.md`).
   - El texto de descripción (`<p class="tarjeta-articulo__descripcion">`).
   - El `href` del enlace PDF (`articulos/<categoria>/<tu-slug>.pdf`).
   - El texto alternativo visual del enlace (dentro de `<span class="oculto-visual">`).
5. Actualizá el número de artículos disponibles en el `<h2>` de esa categoría: por ejemplo, de "3" a "4".

## Agregar una categoría nueva

Es un cambio más grande. Requiere:

1. Crear una carpeta nueva en `articulos/` con el nombre de la categoría (minúscula, sin tilde).
2. Crear una carpeta nueva en `contenido/fichas/` con el mismo nombre.
3. Agregar 3 artículos siguiendo los pasos de la sección anterior (imagen, PDF, ficha .md).
4. Crear un archivo `.html` nuevo: copiá la estructura de `tejidos.html` (o cualquier otra categoría) y actualizá:
   - `<title>` y metadatos (og:title, og:description, canonical URL).
   - El `<h1>` con el nombre de la categoría.
   - El párrafo introductorio (`<p class="intro">`).
   - La sección de artículos: reemplazá la lista de tarjetas con las 3 de la categoría nueva.
   - El `aria-current="page"` en el menú principal debe apuntar al enlace de la categoría nueva.
   - La sección de consejos o tips, si corresponde (ej. "Cuidado de los tejidos").
5. Actualizá **todas las 5 páginas HTML** (`index.html`, `tejidos.html`, `ceramicos.html`, `pinturas.html`, `accesibilidad.html`) para agregar el enlace a la categoría nueva en el menú de navegación principal.

Si no estás familiarizado con HTML, conviene pedir ayuda técnica para este paso.

## Cambiar el nombre del emprendimiento

"Manos de Barro y Lana" es el nombre provisional de este bosquejo. Para cambiarlo, tenés que actualizar en **todas las 5 páginas HTML**:

1. El `<title>` del `<head>`.
2. El `<meta name="author">`.
3. El `<meta property="og:site_name">`.
4. El `<meta property="og:title">` (puede ser distinto del `<title>`, ej. con emojis o más corto).
5. El nombre de marca en el `<a class="marca">` del header (dentro del `<span class="marca__nombre">`).
6. El texto legal del footer (`<p class="pie__legal">`).

Buscá "Manos de Barro y Lana" en el HTML de cada página y reemplazá con el nombre real del emprendimiento.

## Actualizar datos de contacto

El sitio contiene **datos DE EJEMPLO**, ficticios. Antes de publicar, hay que reemplazarlos:

- Email: `contacto@manosdebarroylana.example` → correo real.
- WhatsApp: `+54 9 11 0000-0000` → número real (es un link `https://wa.me/<número>`).
- Ciudad: `Villa Serrana, Córdoba, Argentina` → ubicación real del taller.

Aparecen en:
- El pie de página de las 5 páginas HTML (en `<section aria-labelledby="h-pie-contacto">`).
- En `index.html`, en la sección "Cómo comprar" y "Sobre el taller".

## Funciones de accesibilidad incluidas

El sitio tiene varias funciones de accesibilidad integradas en CSS:

- **Saltos de navegación**: al inicio de cada página, enlaces para saltar directo al contenido principal o al menú, sin recorrer todo lo anterior.
- **Ajuste de color para daltonismo**: 3 filtros de corrección (protanopia, deuteranopia, tritanopia) que redistribuyen la información de color hacia los canales que la persona sí distingue.
- **3 tamaños de texto** (A, A+, A++) que se aplican a todo el contenido sin cortar ni superponer elementos.
- **Modo oscuro y alto contraste automáticos** según la configuración del sistema operativo.
- **Navegación completa por teclado**: toda la funcionalidad es accesible con Tab, flechas de dirección, Enter y Espacio. El foco siempre es visible.

Para detalle de cómo usar estas funciones, ver `accesibilidad.html`.

## Limitación conocida

Debido a que el sitio NO usa JavaScript, el **ajuste de color y el tamaño de texto elegidos no se mantienen al cambiar de página**. Hay que volver a elegirlos en cada página.

Esta es una decisión inherente al enfoque de accesibilidad sin JavaScript. Está documentado también en `accesibilidad.html`.

## Validadores usados

El sitio fue revisado con los siguientes validadores, y cumple sin errores bloqueantes:

- **W3C Nu HTML Checker**: validación de HTML en las 5 páginas — 0 errores.
- **W3C CSS Validator**: validación de `estilosIA.css` — 0 errores.
- **axe-core**: validación de accesibilidad contra criterios WCAG — la única violación reportada es "moderate" (`region`, best-practice no obligatoria) inherente al mecanismo CSS puro de accesibilidad sin JavaScript (los 7 radios de control quedan fuera de landmarks); no afecta el cumplimiento de ningún criterio WCAG.
- **html-validate**: validación de semántica HTML — 0 violaciones bloqueantes.

## Antes de publicar el sitio

Checklist rápida:

- [ ] Reemplazá todas las imágenes de artículos (`articulos/<categoria>/<slug>.png`) por fotos reales.
- [ ] Reemplazá todos los PDF placeholder por versiones más elaboradas si corresponde.
- [ ] Actualizá los datos de contacto (email, WhatsApp, ciudad) en las 5 páginas.
- [ ] Confirmá o cambiá el nombre del emprendimiento en las 5 páginas.
- [ ] Reemplazá la URL de ejemplo `https://manosdebarroylana.example` en los metadatos (`<link rel="canonical">` y `og:url` de las 5 páginas) por el dominio real cuando exista uno.
- [ ] Revisá la tabla de precios y disponibilidad, si corresponde.
- [ ] Probá que el sitio se vea bien y se navegue bien desde un celular y desde distintos navegadores.
