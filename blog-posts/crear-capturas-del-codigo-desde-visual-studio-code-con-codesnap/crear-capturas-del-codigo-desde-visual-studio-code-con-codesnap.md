---
published: false
title: "Crear capturas del código desde Visual Studio Code con CodeSnap"
cover_image: "https://raw.githubusercontent.com/ruslanguns/my-dev-articles/master/blog-posts/crear-capturas-del-codigo-desde-visual-studio-code-con-codesnap/assets/cover_image.png"
description: "Te enseñaré cómo realizar una captura de tu código para que lo compartas de una forma elegante"
tags: codesnap, visualstudiocode, polacode, carbon
series:
canonical_url:
---

Si quieres más contenido como éste por favor no dudes en suscribirte y apoyarme con un comentario, muchas gracias.

## Introducción

En este artículo voy a enseñarte cómo capturar el código de tu editor [Visual Studio Code][vscode] (VSCode) de una forma elegante usando una extensión llamada "[CodeSnap][codesnap]" totalmente gratuita para nuestro editor favorito, si quieres ver una muestra del resultado míralo en la siguiente imagen:

![CodeSnap Capture](https://raw.githubusercontent.com/ruslanguns/my-dev-articles/master/blog-posts/crear-capturas-del-codigo-desde-visual-studio-code-con-codesnap/assets/cover_image.png)

## Alternativas

Esta no es la única herramienta con la que puedes realizar capturas de código, por ello antes te mostraré un par de alternativas.

### Carbon 😯

[Carbon][carbon] es una aplicación web pionera de esta moda, en la que tenemos que copiar el código visitar su web, elegir el lenguaje de nuestro código y posteriormente exportar la imagen, aunque sin duda el resultado es impresionante y que en la actualidad una de las herramientas más populares con más de 25 temas y que soporta muchos lenguajes, súmamente intuitivo y con el que puedes obtener resultados impresionantes. Además cuenta con la posibilidad de que copies la imagen al portapapeles, la compartas por twitter, la puedas también exportar en PNG o SVG, y alguna cosa más. Puede que se me olvide alguna característica importante, por favor escríbelo en los comentarios si se me ha escapado algo importante de Carbon.

### Polacode 🤩

[Polacode][polacode_marketplace] es una extensión de [VSCode][vscode] que la descubrí podo después de comenzar a usar Carbon, básicamente es una herramienta que desde el mismo VScode nos permite a partir de una selección de código generar una imagen que en la mayoría de los casos incluso tiene el mismo tema que usamos, sin duda una de las herramientas más populares de su tipo. Solo puede usarse dentro de VScode por lo que no te será compatible con otros editores de texto. Ésta extensión nos permite personalizar el fondo de pantalla, la transparencia y a lo mejor un par de cosas más.

### CodeSpap 😱

[CodeSnap][codesnap], es un proyecto que basado en Polacode pero que extiende sus configuraciones, por lo que funcionan de la misma forma y sus diferencias ya viene en su valor agregado. Por lo que también es exclusiva para usarse dentro de VScode.

#### Características de CodeSnap

* Guarda capturas de tu pantalla de forma rápida.
* Puedes copiar la imagen a tu portapapeles.
* Puedes mostrar o no las lineas con números.
* Rápido acceso con el botón derecho del ratón (botón secundario)
* Posibilidad de configurar un atajo de teclado,
* y muchas otras configuraciones.

## Conclusión

En los pocos días que llevo probando a [CodeSnap][codesnap] creo que, sin lugar a dudas, es mi extensión favorita para compartir trazas de código con mis amigos. Si te gusta VScode tanto como a mi, no querras perder la oportunidad de probarlo esta increíble extensión.

Las tres herramientas que les he traído son totalmente open source y seguro que ustedes podrán contribuir con estos proyectos si así lo quieren.

En el caso que no uses [VSCode][vscode] y uses [Sublime Text][sublimetext], [Atom][atom], o cualquier otro, no cabe duda que la herramienta que yo elegiría sería [Carbon][carbon] ya que es muy fácil de usar y esta al alcance de un Copy & Paste.

## Notas finales

Espero que te haya gustado mi artículo no olvides darme un me gusta y déjame en los comentarios que opinas de mi artículo, también comparte en los comentarios si conoces otras alternativas que puedas conocer. ¡Gracias! 😊

### Leer más

* Descarga [CodeSnap][codesnap].
* Descarga [Polacode][polacode_marketplace]
* Página oficial de [Carbon][carbon]
* Página oficial de [Visual Studio Code][vscode]
* Página oficial de [Atom][atom]
* Página oficial de [Sublime Text][sublimetext]

## ¿Has encontrado un error en mi artículo?

Si has encontrado un error tipográfico, expresión, referencia o cualquier cosa que debería mejorar y que debe ser actualizado en este post, puedes hacer un fork de [mi repositorio][repositorio] y enviarme un Pull Request con la corrección, o bien, en lugar de hacer un comentario, ruego me lo reportes en [la sección de issues en el repositorio][issues] de mis artículos.

## Más artículos de Ruslán González

* [Argumentos y variables de entorno en Docker](https://dev.to/ruslangonzalez/argumentos-y-variables-de-entorno-en-docker-j9o)


<!-- TAGGED LINKS -->
[polacode_marketplace]: hhttps://marketplace.visualstudio.com/items?itemName=pnp.polacode "Marketplace VSCODE Polacode"
[carbon]: https://carbon.now.sh/
[codesnap]: https://marketplace.visualstudio.com/items?itemName=adpyke.codesnap
[vscode]: https://code.visualstudio.com/
[atom]: https://atom.io/
[sublimetext]: https://www.sublimetext.com/

<!-- Repositorio -->
[issues]: https://github.com/ruslanguns/my-dev-articles/issues
[repositorio]: https://github.com/ruslanguns/my-dev-articles
[code-repo]: https://github.com/ruslanguns/online-resources/tree/master/articles/docker-arg-y-variables-de-entorno
