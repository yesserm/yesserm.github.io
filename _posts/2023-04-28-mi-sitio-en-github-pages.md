---
layout: post
title:  "Mi sitio en GitHub Pages con Jekyll"
date:   2023-04-28 15:57:19 -0600
categories: tutorial
author: "Yesser Miranda"
meta: "Nicaragua"
---

Siempre consideré necesario disponer de un sitio web donde compartiera mi aprendizaje en el mundo de la programación, pero siempre me detuvo mi poca atracción sobre las tecnologías frontend, por eso, me dispuse a probar [GitHub Pages][github_pages], que facilita la creación y publicación de tu información de una manera sencilla, utilizando [GitHub Actions][github_actions] de forma automática, a la vez que te brinda un nombre [DNS][dns] para que sea accedido de manera simple.

Estaré compartiendo paso a paso como logré implementar mi sitio con [GitHub Actions][github_actions] y no haber fracasado en el intento. Es importante tener en cuenta el sistema operativo en donde instalar el software necesario, lo único que puedo recomendar es, trabajar desde Linux o MacOS, debido a la facilidad de instalación de [Ruby][ruby] y [Jekyll][jekyll].

### Contenido
- [Creación del repositorio en GitHub](#creación-del-repositorio-en-github)
- [Crea tu sitio con Jekyll](#crea-tu-sitio-con-jekyll)
  - [Instalación de Ruby](#instalación-de-ruby)
  - [Instalación de Bundler](#instalación-de-bundler)
  - [Crea tu sitio y configura los aspectos básicos](#crea-tu-sitio-y-configura-los-aspectos-básicos)
  - [Publica los cambios](#publica-los-cambios)
- [Elije un tema para tu sitio](#elije-un-tema-para-tu-sitio)
- [Configurar una fuente de publicación en GitHub](#configurar-una-fuente-de-publicación-en-github)
  - [Uso un flujo de trabajo de GitHub Actions personalizado](#uso-de-un-flujo-de-trabajo-de-github-actions-personalizado)
- [Crear contenido usando Markdown y un procesador de este formato](#crear-contenido-usando-markdown-y-un-procesador-de-este-formato)

### Creación del repositorio en GitHub
Lo primero que debemos tener es, un repositorio en GitHub, donde puedas almacenar la información a compartir, para eso debemos ir a la opción de agregar un Nuevo repositorio en nuestra cuenta de GitHub y establecer algunos parámetros necesarios como el nombre del repositorio que debe coincidir con el siguiente formato `<usuario>.github.io`. Listo, tenemos nuestro repositorio que va a hospedar nuestros ficheros informativos.  
![Repositorio de GitHub para Pages][repo]
Hay que tener en cuenta que existen límites de compilación del sitio a producción, por eso recomiendo mejor trabajar con una herramienta generadora de sitios estáticos como [Jekyll][jekyll] que facilita la creación de un sitio base y la construcción de este, usando un empaquetador y gestor de dependencias de las gemas de Ruby para que estas coincidan en versiones en los diferentes entornos por donde pase el proyecto.  
#### Configurar una fuente de publicación en GitHub
Debido a que GitHub siempre automatiza la publicación de los recursos que sean agregados en el repositorio, puedes enviar un simple fichero `index.md`, `index.html` o un `README.md` con un simple título de página y GitHub Actions tomará ese fichero y lo publicará con un flujo de trabajo interno. Pero es necesario configurar cual es la fuente de publicación que se debe usar, por tanto, debes ir a la configuración del repositorio remoto, en el apartado de páginas relacionado a GitHub Pages y seleccionar el tipo de fuente que puede ser desde una rama del repositorio y un flujo de trabajo personalizado con GitHub Actions.  

- Para poder configurar este apartado, es necesario subir contenido al repositorio remoto, lo más sencillo de hacer es clonar el repositorio remoto y agregar al menos un fichero, debemos tener en cuenta que el repositorio debe ser personal:  

```bash

git clone git@github.com:yesserm/yesserm.github.io.git
cd yesserm.github.io
touch index.md
echo "# Mi sitio en GitHub" >> index.md

```  

- Ahora, es necesario publicar los cambios usando el flujo de trabajo básico de Git hacia el repositorio remoto en GitHub.  

```bash

git add .
git commit -m "Confirmacion inicial de mi sitio en GitHub Pages"
git push origin main

```  

- Esto debería publicar los cambios en el repositorio remoto y habilitar la configuración de la fuente de publicación a GitHub Pages.


### Crea tu sitio con Jekyll

#### Instalación de Ruby
#### Instalación de Bundler
#### Crea tu sitio y configura los aspectos básicos
#### Publica los cambios


### Elije un tema para tu sitio

#### Uso de un flujo de trabajo de GitHub Actions personalizado



### Crear Contenido usando Markdown y un procesador de este formato

<!--DOCS-->
[github_pages]: https://pages.github.com/ "Pagina de inicio de GitHub Pages"
[github_actions]: https://github.com/features/actions "Característica de GitHub Actions"
[dns]: https://en.wikipedia.org/wiki/Domain_Name_System "Sistema de Nombres de Dominio"
[temas]: https://pages.github.com/themes/ "Temas soportados por GitHub Pages"
[minima_github]: https://github.com/jekyll/minima "Repositorio GitHub de Minima"
[jekyll]: https://jekyllrb.com/docs/ "Herramienta generadora de sitios estáticos"
[ruby]: https://www.ruby-lang.org/es/documentation/installation/ "Documentación del Lenguaje Ruby"

<!--IMAGES-->
[repo]: /images/create-new-repo-github.jpg "Crear un nuevo repositorio de GitHub"