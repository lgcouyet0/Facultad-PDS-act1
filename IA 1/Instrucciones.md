# Contexto
Se desea generar un sitio web estático (sin scripts) como bosquejo para un emprendimiento que vende artesanías, donde agrupará distintos artículos en diferentes categorías.
Además, el sitio debe ser accesible. Es decir, el bosquejo deberá contar con funcionalidades de accesibilidad para que personas con capacidades distintas puedan navegar a través del mismo sin dificultades.

# La web
## Artículos y categorías
Deberá existir una carpeta dedicada a los artículos del emprendimiento, dentro del scaffold del proyecto. La misma debe llamarse "articulos", y dentro de esta, una carpeta por cada categoría.

El emprendimiento maneja 3 categorías de artículos: Tejidos, Cerámicos y Pinturas.

Por cada artículo se encontrarán 2 archivos, uno .png y otro .pdf. El archivo .png será la representación visual del artículo y el artículo .pdf tendrá información adicional del producto.

Los artículos se deben guardar en la carpeta que corresponda, es decir, un abrigo de tela se encontrará dentro de la carpeta Tejidos, pero no dentro de Cerámicas.

### Representación dentro de la web
Los artículos se representarán, principalmente por la imágen; debajo de ella, un hipervinculo de +info que permitirá descargar el pdf para leer.

## Responsividad
El sitio debe ser responsivo, mobile-first. Además, todos los elementos se deben ver de manera correcta, incluso si una persona hiciera zoom en la página. 

# Accesibilidad
La página debe cumplir con los lineamientos de la WCAG (Pautas de Accesibilidad para el Contenido Web).

Debe ser accesible para todas las personas. Por ejemplo, las imágenes deben tener sus respectivas descripciones alternativas para que personas ciegas puedan saber qué representa la imágen seleccionada, o que el texto sea legible en todo momento y bajo cualquier condición, pero tampoco quedar feo o con contrastes exagerados. También se debe soportar filtros para personas con daltonismo. Estos son solo algunos casos, así que cuanto más accesible sea la página, mejor.

## Navegación
La página debe soportar una navegación coherente por teclado. Es decir, que al utilizar el tab, por ejemplo, no se dirija desde el menú hacia el footer de manera directa.

## Tamaño de letra
El tamaño de la letra debe ser óptimo, ni muy chico, ni muy grande, pero legible para todas las personas.

## Zoom de la web
Considerar que las personas que hagan zoom en la página, podrían tener problemas de vista, por lo tanto el zoom debe servir principalmente para que dichas personas puedan navegar a través de la página de manera coherente y que todos los elementos continúen manteniendo, tanto la armonía de la página y el sentido de la misma.

# Mejores prácticas
Para el desarrollo de la web se deben utilizar las mejores prácticas de HTML, CSS y accesibilidad.
Además, se debe hacer uso correcto de los secciones, meta tags, tags semánticos y más.

# Restricciones
- Para la web, solo se debe utilizar HTML y CSS.
- Las reglas de estilo deben estar en un archivo separado, denominado estilosIA.css.

# Datos iniciales
Para la página, deberás generar 3 artículos por cada categoría. Deberás generar la información de cada artículo en un archivo .pdf y luego yo me encargaré de buscar las imágenes correspondientes en internet.

# Validaciones
La web deberá cumplir con las validaciones de distintos validadores, de HTML, de CSS, de Accesbilidad y de Lector de pantalla.

Además, las personas dueñas del emprendimiento verificarán los siguientes puntos:
- El lector de pantalla debe leer correctamente los textos alternativos de las imágenes.
- Se debe poder navegar por completo el menú de categorías usando únicamente el teclado (Tab, Shift+Tab, Enter)
- El orden en que el lector anuncia los elementos de la página debe ser lógico.
