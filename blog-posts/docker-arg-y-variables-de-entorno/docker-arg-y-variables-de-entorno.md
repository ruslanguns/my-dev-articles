---
published: true
title: "Argumentos y variables de entorno en Docker"
cover_image: "https://raw.githubusercontent.com/ruslanguns/my-dev-articles/master/blog-posts/docker-arg-y-variables-de-entorno/assets/cover_image.png"
description: "Te enseñaré cómo usar los argumentos o variables de entorno en Docker"
tags: docker, arguments, arg, env
series:
canonical_url:
---

En la parte inferior del artículo colocaré el enlace al repositorio con el código resultante.

## Introducción

Este artículo esta dirigido para personas que tienen experiencia básica con Docker, o bien desean repasar un poco las bondades que nos ofrece este sistema de contenedores.

Voy a mostrarte aquí un breve ejemplo para usar los argumentos y variables de entorno en el despliegue de contenedores en Docker, asimismo te enseñaré cómo configurarlas para que sean dinámicas y fáciles de gestionar.

> Pero antes de todo, **¿qué es una variable de entorno?**

Una variable de entorno según la [wikipedia][wiki-env] — es un valor con nombre dinámico que puede afectar la forma en que los procesos en ejecución se comportarán en una computadora. Son parte del entorno en el que se ejecuta un proceso —. En otras palabras, imaginemos estamos creando un contenedor que despliegue una imagen con base de datos como MySQL o Postgres, si vamos a las documentaciones oficiales notaremos que para crear una contraseña o un nombre para una base de datos debemos enviarle unos parámetros mediante variables con nuestros valores, pues éstas variables son argumentos la imagen de docker recibirá a través de las variables de entorno.

Es importante que sepamos qué es un argumento, según nuestra [wikipedia][wiki-arg], — un parámetro es una variable utilizada para recibir valores de entrada en una rutina, subrutina o método. Dichos valores, que serán enviados desde la rutina invocante, son llamados argumentos —. Entonces es un argumento que "recibe" el valor del parámetro por parte de las variables de entorno para ejecutar una rutina, o un proceso.

Si estos conceptos no están aún claros, no te preocupes, intentaré explicártelo en el resto del artículo, pero ahora, con un breve ejemplo, en el que tú podrás diferenciar el uno del otro.

Ejemplo:

```bash
docker run -e MYSQL_ROOT_PASSWORD=mi-contraseña mysql
```

> Ve a [este enlace][docker-hub-mysql] para saber cómo construir un contenedor de Mysql

Si ejecutamos este comando, notaremos intuitivamente que si pasamos el valor a la variable `MYSQL_ROOT_PASSWORD=mi-contraseña` siendo 'mi-contraseña' la contraseña ROOT que estamos asignando a nuestra base de datos y ésta variable de entorno y el parámetro resultante es el que usaremos a continuación para comunicarnos con la base de datos.

Lo más probable que hayas configurado cientos de veces este tipo de variables, y puede que a lo mejor te preguntes cómo instalar en nuestra imagen el argumento para que esté a la escucha de estos parámetros, pues hoy es tu día de suerte, justamente es eso lo que intentaré enseñarte a continuación.

## Inspiración

Cuando programas y haces el deploy de una aplicación, te enfrentas a diferentes retos y procesos, que en este oficio, siempre van en aumento. Hace un tiempo atrás miraba la necesidad de crear una configuración, que sin las variables de entorno, hubiera sido imposible lograr el cometido.

El uso de argumentos y variables de entorno nos trae un excelente beneficio para comunicarnos con la aplicación mediante parámetros de configuración, data que preferiblemente, y en la mayoría de los casos, debe ser secreta, incluso nos puede ayudar a crear diferentes entornos de programación utilizando una sola configuración.

## Vamos a la práctica

Vamos a crear una imagen que nos devuelva un Hola mundo en Docker que nos será suficiente para aprender a utilizar esta técnica.

### Creación de nuestro entorno de trabajo

Creamos una carpeta en nuestro ordenador en el lugar que queramos, accedemos a ella y dentro creamos un archivo llamado "Dockerfile".

```bash
$ mkdir docker-env && cd docker-env
$ touch Dockerfile
```

A continuación vamos a introducirle los siguientes datos al archivo Dockerfile:

```sh
// Dockerfile
FROM alpine:3.7
ARG NAME
CMD echo "Hola ${NAME}!"
```

¿Qué tenemos aquí?

* **FROM alpine:3.7**: Con esto vamos a indicarle a Docker la fuente o sistema operativo de nuestra imagen.
* **ARG NAME**: Con la opción ARG indicamos el argumento que deseamos para nuestra imagen. Con esto le decimos a Docker, tu esperarás un argumento personalizado al momento de crear la imagen.
* **CMD echo "Hola ${NAME}!"** Finalmente con esto estamos diciéndole a Docker el comando que queremos que ejecute al momento de lanzar la imagen.

En esta configuración lo que hemos logrado, es crearnos una imagen con un sistema operativo basado en la imagen alpine desde su versión 3.7, puedes encontrár más información sobre esta imagen haciendo [click aquí][alpine_images], 

A lo mejor te estas preguntando, ¿qué pasaría si no se define el argumento? bueno, Docker lo único que hace es ignorar y continuar con la ejecución del programa, lo que nos quiere decir que esta variable es undefined, o bien, es como si nunca hubiera existido. Podríamos crear una condición para validar y evitar que el programa se ejecute, o al menos introducirle un valor por defecto, te lo explicaré más adelante en este artículo.

Ya llego el momento de probar esto en la práctica. Lo primero es crear la imagen con el nombre 'saludo', para ello ejecutaremos la siguiente instrucción:

```bash
docker build -t saludo .
```

Después de ejecutarlo, Docker comenzará a descargar las dependencias y creará la imagen en nuestro sistema operativo, y si no nos lanza ningún error raro quiere decir que la imagen ya está lista.

Ejemplo de lo que obtendremos por consola:

```bash
Sending build context to Docker daemon  3.072kB
Step 1/3 : FROM alpine:3.7
 ---> 6d1ef012b567
Step 2/3 : ARG NAME
 ---> Using cache
 ---> 0cd8f552966f
Step 3/3 : CMD echo "Hola ${NAME}!"
 ---> Using cache
 ---> dc225ab6e03c
Successfully built dc225ab6e03c
Successfully tagged saludo:latest
```

Ya tenemos nuestra imagen compilada y lista para usarse, ejecuta el siguiente comando con las instrucciones a continuación, para crearnos un contenedor con nombre 'mi-contenedor':

```bash
docker run -e NAME=Ruslan --name mi-contenedor saludo

// output: Hola Ruslan!
```

Si el mensaje de salida es 'Hola Ruslan!', lo has logrado. Y has conseguido configurar un contenedor con una imagen que recibe un argumento dinámicamente.

Ahora probaremos con el docker-compose, si no le conoces, es una herramienta que nos facilita la vida, nos simplifica la manera de cómo trabajamos con Docker, con él podemos configurar un script con todas las configuraciones para administrar los contenedores de Docker.

Es momento de probar nuestro argumento dinámico con docker-compose, para esto utilizaremos la instrucción environment.

Crea un archivo en la raíz de la carpeta con el nombre 'docker-compose.yml, con el contenido siguiente:

```yaml
// docker-compose.yml
version: "3.7"
services:
  app:
    build: .
    environment:
      - NAME=Ruslan # Aquí estamos haciendo uso del argumento
```

> Hay muchas formas para invocar las variables de entorno desde docker-compose, yo he elegido la forma más sencilla para evitar complicaciones, si quieres leer más sobre esto, ve a [éste enlace][docker-compose-variables].

Perfecto, ya tenemos lista la configuración y ahora vamos a usarla, ejecuta el siguiente comando en la consola:

```bash
docker-compose up
```

Esta instrucción nos tiene que devolver algo como esto:

```bash
Recreating docker-env_app_1 ... done
Attaching to docker-env_app_1
app_1  | Hola Ruslan!
docker-env_app_1 exited with code 0
```

Listo!, ya has conseguido configurar variables de entorno en un docker-compose pasándole parámetros mediante los argumentos creados en el Dockerfile de nuestra imagen.

## Repaso

1. Hemos creado un Dockerfile que recibe un argumento dinámico
2. Hemos lanzado la imagen y nos ha devuelto el valor que hemos pasado con Docker.
3. Y finalmente hemos configurado un archivo docker-compose con el mismo objetivo.

## Bonus

Para proteger nuestra imagen para que no se configure sin un parámetro — en el momento del build — podemos darle un valor por defecto a ese argumento. Veámoslo en código:

Vamos a editar el archivo Dockerfile y cambiaremos de esto:

```bash
// Dockerfile
FROM alpine:3.7
ARG NAME
CMD echo "Hola ${NAME}!"
```

a esto:

```bash
// Dockerfile
FROM alpine:3.7
ARG NAME=mundo
RUN echo "Hola ${NAME}!"
CMD echo "Hola ${NAME}!"
```

¿Qué ha pasado? hemos añadido el valor mundo al argumento NAME. Y he añadido una linea más a la instrucción `RUN echo "Hola ${NAME}!"` para que nos imprima en el texto del build el valor que hemos puesto, ya que éste solo quedará para reservada para el momento del build de la imagen únicamente guardándose en ella el valor en la configuración por defecto, permitiéndonos a nosotros siempre reconstruir la imagen con otro valor si es necesario.

Lo primero es recrear la imagen:

```bash
docker build -t saludo .
```

Analicemos con atención el output de este comando:

![alt text][docker2]

Notemos que el paso 3/4 nos ha devuelto el Hola mundo!, con total seguridad sabemos que la imagen ha sido creada con esa variable tomada en consideración.

Sabiendo eso, ahora vamos a consumir la imagen en un contenedor:

```bash
docker run --name mi-contenedor-2 saludo
// output: Hola !
```

Notemos que a la instrucción sin proveer una variable de entorno, con el nombre "mi-contenedor-2" el output de la consola nos ha devuelto solo 'Hola !', pero no nos ha devuelto el mundo. Esto es un comportamiento esperado, la imagen solo en el momento de crearse ha a alojado el valor del argumento por defecto, y la ha usado para configurar el sistema.

Si deseamos crear una imagen pero que use otro valor, simplemente debemos proveer la variable de entorno.

Vamos a probarlo entonces, ejecuta la siguiente instrucción en la consola:

```bash
docker run -e NAME=Alexander --name mi-contenedor-3 saludo
// output= Hola Alexander!
```

Excelente, el contenedor nos ha enviado el resultado que esperábamos y con esto, creo que ya estas listo para que con tu creatividad crees entornos personalizados para tus desarrollos con docker.

## Conclusión

Creo que esta bastante claro que Docker es una solución completa y muy personalizable, el uso de las variables de entorno apenas constituye una pequeña parte de todo lo que Docker puede hacer.

Recuerda que puedes encontrar contenido mio en mi [canal de Youtube](https://youtube.com/ruslangonzalez), en mis directos por [Twitch](https://twitch.com/rusgunx), o a través de [Twitter](https://twitter.com/ruslangonzalez).

### Leer más

* Documentación oficial: [Argumentos en Docker][docker-docs-arg].
* Repositorio con el código: [Github][code-repo]

## ¿Has encontrado un error en mi artículo?

Si has encontrado un error tipográfico, expresión, referencia o cualquier cosa que debería mejorar y que debe ser actualizado en este post, puedes hacer un fork de [mi repositorio][repositorio] y enviarme un Pull Request con la corrección, o bien, en lugar de hacer un comentario, ruego me lo reportes en [la sección de issues en el repositorio][issues] de mis artículos.

<!-- TAGGED LINKS -->
[alpine_images]: https://hub.docker.com/_/alpine
[docker-compose-variables]: https://docs.docker.com/compose/environment-variables/
[docker-docs-arg]: https://docs.docker.com/engine/reference/builder/#arg
[docker-hub-mysql]: https://hub.docker.com/_/mysql
[wiki-env]: https://en.wikipedia.org/wiki/Environment_variable
[wiki-arg]: https://es.wikipedia.org/wiki/Argumento_(inform%C3%A1tica)
<!-- images -->
[docker2]: https://raw.githubusercontent.com/ruslanguns/my-dev-articles/master/blog-posts/docker-arg-y-variables-de-entorno/assets/docker2.jpg "Imagen de consola de docker"
<!-- Repositorio -->
[issues]: https://github.com/ruslanguns/my-dev-articles/issues
[repositorio]: https://github.com/ruslanguns/my-dev-articles
[code-repo]: https://github.com/ruslanguns/online-resources/tree/master/articles/docker-arg-y-variables-de-entorno
