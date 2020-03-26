---
published: false
title: 'Docker ARG y Variables de Entorno'
cover_image: 'https://raw.githubusercontent.com/ruslanguns/dev.to/master/blog-posts/docker-arg-y-variables-de-entorno/assets/cover_image.png'
description: 'Te enseñaré cómo usar los argumentos o variables de entorno en Docker'
tags: docker, docker-compose, deploy, arg, arguments, development, software, backend, argumentos, variables de entorno
series:
canonical_url:
---
# Introducción

Este artículo esta dirigido para personas que tienen experiencia básica con Docker, o bien desean repasar un poco las bondades que nos ofrece este sistema de contenedores.

Voy a mostrarte aquí un breve ejemplo para usar las variables de entorno en el despliegue de contenedores en Docker, asimismo te enseñaré cómo configurarlas de forma dinámica.

>Pero antes de todo, **¿qué es una variable de entorno?**

Una variable de entorno según la [wikipedia](https://en.wikipedia.org/wiki/Environment_variable) es un valor con nombre dinámico que puede afectar la forma en que los procesos en ejecución se comportarán en una computadora. Son parte del entorno en el que se ejecuta un proceso. En otras palabras, imaginemos estamos creando un contenedor que despliegue una imagen con base de datos como MySQL o Postgres, si vamos a las documentaciones oficiales notaremos que para crear una contraseña o un nombre para una base de datos debemos enviarle unos parámetros:

Ejemplo:

```bash
docker run -e MYSQL_ROOT_PASSWORD=mi-contraseña mysql
```

> Ve a [este enlace](https://hub.docker.com/_/mysql) para saber cómo construir un contenedor de Mysql


Si ejecutamos este comando, notaremos intuitivamente que si pasamos el valor a la variable `MYSQL_ROOT_PASSWORD=mi-contraseña` siendo 'mi-contraseña' la contraseña ROOT que estamos asignando a nuestra base de datos y ésta variable de entorno es la que usaremos a continuación para comunicarnos con la base de datos. Lo más probable que hayas configurado cientos de veces este tipo de variables, y puede que a lo mejor te preguntes cómo instalarla en nuestra imagen, justamente eso es lo que intentaré enseñarte en este artículo.

# Inspiración

Cuando programas y haces el deploy de una aplicación, te enfrentas a diferentes retos y procesos, que en este oficio, siempre van en aumento. Hace un tiempo atrás miraba la necesidad de crear una configuración, que sin las variables de entorno, hubiera sido imposible lograr el cometido. Saber hacerlo me hizo dar un gran paso adelante en la construcción de imágenes personalizadas y creo que se ha convertido en con lo convivo a diario.

El uso de argumentos y variables de entorno nos trae un excelente beneficio para comunicarnos con la aplicación mediante parámetros de configuración, data que preferiblemente, y en la mayoría de los casos, debe ser secreta, incluso nos puede ayudar a crear diferentes entornos de programación utilizando una sola configuración.

# Vamos a la práctica

Vamos a crear una imagen que nos devuelva un Hola mundo en Docker que nos será suficiente para aprender a utilizar esta técnica.

## Creación de nuestro entorno de trabajo

Creamos una carpeta en nuestro ordenador en el lugar que querramos, accedemos a ella y dentro creamos un archivo llamado "Dockerfile".

```bash
$ mkdir docker-env && cd docker-env
```

Estos argumentos, se lo podemos pasar de diferentes maneras, lo único que debemos hacer es configurar una entrada en nuestro `Dockerfile` para que sepa que necesitamos que esté a la escucha de este argumento:

Ejemplo:

```dockerfile
// code/demo-code.ts
RUN echo "Hola ${NAME}!"
ARG NAME
```

En el ejemplo anterior estoy definiendo una variable con el argumento NAME y este lo estoy pintando en un comando RUN para que imprima en consola el típico "Hola \$NAME" siendo NAME la variable dinámica que estamos pasando desde las variables de entorno de nuestro sistema operativo anfitrión las cuales pueden reemplazadas cuando consumamos la imagen.

# El momento del build

# Variables desde el comando del build

Si queremos ejecutar la imagen simplemente lanzamos el build con la bandera - build-arg <Nombre del argumento=valor_del
argumento

Ejemplo para este caso:

```bash
docker build - build-arg NAME=RUSLAN .
```

# Variables desde el sistema operativo

Dependiendo del sistema operativo puede que tengamos que exportar la variable de entorno de diferentes formas pero al menos en LINUX es de la siguiente manera:

```bash
export $NAME=Ruslan
```

Posteriormente para decirle a docker que este argumento se lo pasaremos dinámicamente simplemente no definimos ese valor, `docker build - build-arg NAME .` y él ya sabrá que debe estar a la escucha desde ese argumento.

> Tener en cuenta que si no hemos exportado la variable de entorno puede que dependiendo del fin de esa variable puede ser crucial para que la configuración se ejecute correctamente, pero siempre tener en cuenta que podemos hacer una buena práctica en colocar un valor por defecto desde el mismo Dockerfile, en caso que exista una variable con un valor por defecto Docker sabrá que queremos darle prioridad al argumento que hemos declarado mediante el comando del build por lo tanto éste será el valor definitivo de dicha variable y hará caso omiso del valor por defecto que hemos configurado en el archivo.
> Pues hasta este punto ya sabemos cómo inicializar un valor y cómo dinamizarlo con las variables de entorno, ahora sabiendo que Docker es un sistema de contenedores de capas, nosotros también podemos modificar el valor de este argumento en el momento de consumir la imagen:

## Desde el Docker Compose

> En caso que no conozcas sobre DOCKER COMPOSE te recomiendo vallas hagas click en [este enlace](https://docs.docker.com/compose/) para leer más sobre él.
> Para consumir variables de entorno desde el docker compose simplemente definimos la opcion environment dentro de la configuración de nuestros servicios:

Ejemplo:

```yml
version: "3"
services:
 hola:
 build: .
 environment:
 NAME: "Ruslan desde Compose"
```

> Tener en cuenta que la sintaxis de los archivos yaml los obliga a tener indentaciones siempre. Por lo que mucho ojo al escribir aquí.

## Desde la linea de comandos de Docker

Con Docker run podemos pasar variables de entorno mediante la bandera `-e`.

Ejemplo:

```bash
docker run -e "NAME=RUSLAN" ruslanguns/hello`
```

# Conclusión

Creo que esta bastante claro que Docker es una pasada, y en mi caso estoy en constante evolución en mi aprendizaje con él. En este pequeño artículo puedo mostrarles cómo cubrir una necesidad grande sobre el manejo personalizado de Docker, espero les sea de utilidad.

# Referencias:

- [Docker ARG](https://docs.docker.com/engine/reference/builder/#arg)
- Article: [Docker ARG, ENV and .env - a Complete Guide](https://vsupalov.com/docker-arg-env-variable-guide/)

# ¿Has encontrado un error en mi artículo?

Si has encontrado un error tipográfico, expresión, referencia o cualquier cosa que debería mejorar y que debe ser actualizado en este post, puedes hacer un fork de [mi repositorio](https://github.com/ruslanguns/dev.to) y enviarme un Pull Request con la corrección, o bien, en lugar de hacer un comentario, ruego me lo reportes en [el apartado de los issues de mi repositorio](https://github.com/ruslanguns/dev.to/issues).
