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

- Esto debería publicar los cambios en el repositorio remoto y habilitar la configuración de la fuente de publicación a GitHub Pages, la cual debería verse como a continuación:  
![Configuracion de la fuente de GitHub Pages][conf-sources-pages]  
Si se usa esta opción, que es tomar el código de la rama principal y publicarlo, deja un límite de 10 compilaciones cada hora, por eso es recomendable usar un flujo de trabajo de GitHub Actions personalizado.  

Para evitar estar usando compilación a cada momento en GitHub Actions, es recomendable trabajar con el generador de sitios estático Jekyll, que viene integrado en GitHub Pages y soporta [Markdown][markdown], [HTML][html] y el lenguaje de plantillas [Liquid][liquid]. Para eso vamos a trabajar nuestro sitio con esta herramienta y usando todas sus bondades.

### Crea tu sitio con Jekyll
Puedes usar tanto Linux como MacOS y dispondrás de Jekyll, luego de instalar los requerimientos de la herramienta, los cuales son: 
- Ruby 2.5.0 o superior
- RubyGems
- GCC y Make  


#### Instalación de Ruby
Debido a que para esta instalación se usó MacOS Monterey, me dispuse a instalaer [Rbenv][rbenv] que permite disponer de diferentes versiones de Ruby y aplicar las configuraciones necesarias a la variable de entorno.
- Primero procedemos a instalar Rbenv, donde puedes usar cualquier gestor de paquetes, en este caso se hizo una clonación del repositorio base de rbenv en `~/.rbenv`.  

```bash

git clone https://github.com/rbenv/rbenv.git ~/.rbenv

```  

- Luego, se procede a configurar la shell en uso para que cargue rbenv, en este caso se configura la shell `Zsh`  

```bash

echo 'eval "$(~/.rbenv/bin/rbenv init - zsh)"' >> ~/.zshrc

```

- Reinicie la shell o abra una nueva sesión de la terminal y liste las versiones estables disponibles  

```bash

# listar las versiones estables
rbenv install -l

# instalar una version actual
rbenv install 3.1.4

# comprueba las versiones instaladas
rbenv versions

# Elije una version global
rbenv global 3.1.4

```  

- Comprobar la instalación de Ruby, RubyGem, GCC y Make  

```bash

# version de ruby
ruby -v

# version de gem
gem -v

# version de gcc
gcc -v

# version de make
make -v

```  

#### Instalación de Jekyll y Bundler
Luego de haber cumplido los requerimientos de las herramientas, es momento de instalar Jekyll y Bundler en la máquina. Esto garantizará que las gemas se mantengan actualizadas y evitar errores en la construcción con Jekyll y el entorno en el que se ejecuta.

```bash

gem install jekyll bundler

```
#### Crea tu sitio y configura los aspectos básicos
- Ingresa a la raíz de tu sitio web en construcción y genera un sitio con Jekyll usando el siguiente comando:  

```bash

jekyll new --skip-bundle .

```
- Abre el nuevo fichero Gemfile que se creó con Jekyll y comenta agregando `#` al principio de la línea que contiene la gema de `jekyll`, y agregue la gema de `github-pages` con la [versión más reciente][versiones_pages] que encuentre hasta el momento. Para el momento de realizar esta instalación, la versión más actualizada era la 228, entonces mi línea quedaba como a continuación:  

```bash

gem "github-pages", "~> 228", group: :jekyll_plugins

```  

- Proceda a guardar los cambios realizados en el fichero y ejecute el siguiente comando, que va a instalar el plugin de `github_pages` con la versión de `jekyll` compatible.  

```bash

bundle install

```

- Puede realizar cambios en el fichero `_config.yml` para agregar la información necesaria que identifique su sitio.  

```yaml

domain: yesserm.github.io
url: https://yesserm.github.io
baseulr: "" # agregar si el sitio se encuentra en un subdirectorio

```  

#### Prueba los cambios localmente
Si publicamos estos cambios a GitHub, debemos suponer que se han de publicar, pero para evitar muchas compilaciones remotas, podemos ejecutar un servidor local utilizando `bundle` con el siguiente comando:  

```

bundle exec jekyll serve

```  

> Como instalamos Ruby 3.0 o posterior, es posible recibir un error al intentar ejecutar el servidor, esto se debe a que Ruby ya no incluye `webrick` y debemos agregarlo con: `bundle add webrick` y de nuevo volver a ejecutar el comando anterior.  

El comando anterior genera un sitio local en la dirección `http://localhost:4000`, pero si queremos ejecutar el sitio en una red local podemos ejecutar una variante del comando que ejecuta un servidor local.  

```bash

# el parametro --host corresponde a la ip de su maquina
bundle exec jekyll serve -w --host=0.0.0.0

```

### Elije un tema para tu sitio
Si generaste el sitio usando Jekyll, y verificas el fichero `_config.yml`, puedes comprobar que el parámetro `theme: minima` ya existe y se ha aplicado, pero si se quiere otro tema, solo se debe buscar en la [lista de temas admitidos][temas]  

Los temas instalados se deben especificar primero en el fichero Gemfile `gem: "minima"`, y luego ejecutar el comando:  

```bash

bundle install
bundle exec jekyll serve

```  

En el caso que desee realizar modificaciones sobre el tema, únicamente debe realizar una copia del fichero HTML, CSS correspondiente en la raíz del sitio, en su carpeta correspondiente, por ejemplo en `assets`.  

#### Uso de un flujo de trabajo de GitHub Actions personalizado
Aunque esta opción se encuentra aún en versión beta, viene a ser a la más sencilla de administrar, ya que con esta nos quitamos las limitaciones de compilación. Solamente, se debe cambiar el tipo de fuente de publicación en la configuración del repositorio del sitio. Luego de eso preparar un flujo de trabajo de GitHub Actions, en el momento de cambiar el tipo de fuente, GitHub propone un flujo de trabajo base que sirve para automatizar nuestro sitio almacenado en la rama principal del repositorio en la raíz del proyecto.  

El fichero de configuración puede verse como el siguiente código:  

<script src="https://gist.github.com/yesserm/d2ff50b76173f870972c3c67926a85db.js"></script>  

El flujo de trabajo general funciona de la siguiente manera:  
- Se desencada al realizar un evento sobre la rama seleccionada, por ejemplo, un publicación directa con `git`
- Se usa la acción `actions/checkout` en cualquiera de sus versiones, de preferencia la más reciente, para poder extraer el contenido del repositorio.
- En el caso de que el sitio posea ficheros que necesiten compilarse como el formato Markdown, ficheros SASS, etc.
- Se usa la acción `actions/upload-pages-artifact` para cargar los archivos estáticos.
- Se usa la acción `actions/deploy-pages` para publicar en GitHub Pages.

#### Publica los cambios
Prueba una publicación directa en GitHub completando con el flujo de trabajo básico de Git, con los siguientes comandos:  

```bash

git add .
git commit -m "Prueba de publicacion de cambios para usar actions"
git push origin main

```  

Se puede comprobar la compilación, ingresando a la pestaña `Actions` y verificando los cambios en el sitio publicado.  
![Publicando cambios al repositorio remoto usando Actions][publicacion-actions]  

Para probar el sitio se usa la [url generada por el repositorio][sitio_pages], y accediendo a ver los resultados.  
![Imagen resumen][actions-resume]


### Crear Contenido usando Markdown y un procesador de este formato
Para crear una publicación debe crear un directorio llamado `_posts` y en el debe crear un fichero en formato Markdown con la estructura siguiente `AAAA-MM-DD-NOMBRE-POST.md` reemplazando AAAA-MM-DD por la fecha de publicación y NOMBRE-POST por el nombre de la publicación.  

El fichero se crea con el formato Markdown de manera interna, pero se puede manipular la metainformación del sitio en la parte superior siguiendo el siguiente formato:  

```yaml

layout: post
title: "POST-TITLE"
date: YYYY-MM-DD hh:mm:ss -0000
categories: CATEGORY-1 CATEGORY-2

```  

Esto se puede demostrar en el siguiente ejemplo:   

```

---
layout: post
title:  "Mi sitio en GitHub Pages con Jekyll"
date:   2023-04-28 15:57:19 -0600
categories: tutorial
author: "Yesser Miranda"
meta: "Nicaragua"
---

```

> Es posible crear nuevas páginas del sitio creando ficheros en el directorio del sitio y manipulando su vínculo con el atributo `permalink`.


<!--DOCS-->
[github_pages]: https://pages.github.com/ "Pagina de inicio de GitHub Pages"
[github_actions]: https://github.com/features/actions "Característica de GitHub Actions"
[dns]: https://en.wikipedia.org/wiki/Domain_Name_System "Sistema de Nombres de Dominio"
[temas]: https://pages.github.com/themes/ "Temas soportados por GitHub Pages"
[minima_github]: https://github.com/jekyll/minima "Repositorio GitHub de Minima"
[jekyll]: https://jekyllrb.com/docs/ "Herramienta generadora de sitios estáticos"
[ruby]: https://www.ruby-lang.org/es/documentation/installation/ "Documentación del Lenguaje Ruby"
[liquid]: https://shopify.github.io/liquid/ "Lenguaje de plantillas creado por shopify"
[markdown]: https://daringfireball.net/projects/markdown/ "Herramienta para convertir texto simple a HTML"
[html]: https://developer.mozilla.org/es/docs/Web/HTML "Lenguaje de Marcas de Hipertexto"
[rbenv]: https://github.com/rbenv/rbenv "Herramienta administradora de versiones del Lenguaje de programación Ruby"
[versiones_pages]: https://pages.github.com/versions/ "Versionamiento de github pages y plugins de jekyll"
[sitio_pages]: https://yesserm.github.io "Sitio principal publicado"

<!--IMAGES-->
[repo]: /images/create-new-repo-github.jpg "Crear un nuevo repositorio de GitHub"
[conf-sources-pages]: /images/setup-deploy-main-pages.jpg "Configuración de GitHub Pages para tomar cambios de main"
[publicacion-actions]: /images/publicando-sitio-actions.jpg "Publicando sitio en GitHub y usar el flujo personalizado"
[actions-resume]: /images/actions-test.jpg "Probando en la URL generada"
