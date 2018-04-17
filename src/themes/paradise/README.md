# Paradise Theme

## Installation

Inside the folder of your Hugo site run:

    $ cd themes
    $ git clone https://github.com/digitalcraftsman/hugo-agency-theme

For more information read the official [setup guide](//gohugo.io/overview/installing/) of Hugo.


Si ya sabes cómo utilizar Hugo, y nunca antes has usado las páginas de GitHub, y solo quieres saber cómo hacer que todo funcione y se implemente con la menor cantidad de alboroto necesario, entonces muchas de las publicaciones de blog y los tutoriales que encontrará serán un poco frustrantes.

Algunos tutoriales explicarán la parte de GitHub Pages en detalle, pero harán suposiciones sobre cómo está generando su sitio, lo que no necesariamente coincidirá con lo que hace Hugo.

Otros tutoriales supondrán que no sabes nada y explicarán cada paso de todo desde cero.

En esta publicación se supone que sabes cuáles son todas las piezas (hugo, repositorios, un poco de DNS), y solo quieres descubrir la forma más fácil de unir todo.

Primero, una advertencia:

### No hagas una gh-pagesrama
Gran parte de la documentación que encontrarás hablará sobre la creación de una rama llamada gh-pagesHTML. Esto es genial si está creando un sitio de cartera con subsitios para diferentes proyectos en GitHub.

Si está buscando crear un sitio independiente asignado a un dominio personalizado, esa no es la documentación que necesita.

Las gh-pagesramas son a lo que GitHub se refiere como páginas de proyectos . Lo que necesita al crear un sitio independiente son las páginas de usuario o las páginas de organización (que son lo mismo, solo depende de si su usuario de GitHub es un usuario o una organización).

Ok, sigue.

## Una historia de dos repositorios

El truco para implementar un sitio autónomo generado por hugo que se alojará en un dominio personalizado es que todo lo que se necesita debe estar en su propio repositorio `public/`, y ese repositorio debe ser nombrado <username>.github.io, donde <username> está su nombre de usuario real de GitHub.

Esto significa que todas las marcas y plantillas y configuraciones deben ir en un repositorio separado. El repositorio con todas las cosas de Hugo se puede nombrar como prefieras. Por el bien del argumento, supongamos que se llama este repositorio blog.

La configuración inicial depende de cuál es tu situación actual. Lo más probable es que:

1. nada está comprometido con el control de la fuente aún, o
2. ya tienes tu sitio hugo comprometido y enviado al blogrepositorio en GitHub.

### Configuración cuando nada está comprometido
Crea dos repositorios nuevos y vacíos en GitHub:

1. blog
2. <username>.github.ioAsegúrese de marcar Initialize this repository con un cuadro README , ya que facilitará el siguiente paso.
Mata a tu servidor hugo para que deje de regenerar el HTML.

Eliminar el public/directorio con rm -r public/.

Inicializa un repositorio git y agrega el control remoto:


    $ git init
    $ git remote add origin git@github.com:<username>/blog.git

### Configuración cuando ya has cometido y apresurado
Si ya tiene su sitio Hugo comprometido con el control de código fuente y lo envió a GitHub, entonces el proceso es similar, excepto que necesita hacer espacio para el submódulo que va a agregar justo después de que se complete la instalación.

Mata a tu servidor hugo para que deje de regenerar el HTML.

Cree un nuevo repositorio vacío, nombrado <username>.github.ioen GitHub, asegurándose de marcar Inicializar este repositorio con un cuadro LÉAME .

Eliminar el public/directorio de git:

$ git rm -r public

### Agregar el submódulo

Clona el <username>.github.iorepositorio en un submódulo en public:

$ git submodule add git@github.com:<username>/<username>.github.io.git public
Agregue todo y empuje hacia GitHub:

$ git add .
$ git push -u origin master

Debería poder ver la página de índice en .github.io unos minutos después.

Agregue un guión de implementación útil como el guión de [Spencer Lyon's script][deploy]  para simplificar un poco las cosas.

## Asignación de un dominio personalizado
Tanto si va a usar un blog.yoursite.com dominio de tipo subdominio como un dominio de apex yoursite.com, primero debe agregar un archivo denominado CNAME al depósito de submódulos que contiene el dominio al que está asignando.

Tenga en cuenta que este archivo debe tener un nombre CNAME, incluso si el registro DNS que está creando es un registro A o un registro ALIAS en lugar de un registro CNAME.

Si está mapeando un subdominio, cree un registro CNAME con su proveedor de DNS. Para un dominio apex necesitarás un registro ALIAS en un registro A. Depende del proveedor.

Para obtener más información acerca de las asignaciones de DNS, consulte la [guia en GitHub][dns].

Regenerando todas las URL
Una vez que el DNS se ha propagado, necesitará cambiar el nombre de host base en el archivo de configuración de Hugo, regenerar el sitio con las direcciones URL correctas y volver a desplegarlo.



### Usando este tema ejemplo

### Cambia el background

El protagonista actúa como un punto de atracción para su sitio. Así que considere darle un buen historial. Solo necesita reemplazar el header-bg.jpg de static/img con su propia imagen de fondo. Pero es importante que guardes el nombre del archivo original.


### Presenta tus servicios clave

Esta sección debe mostrar sus capacidades y habilidades. Puede cambiar los servicios `[params.services.list]` en el `config.toml`.

Todos los iconos son parte de la fuente del icono de Fontawesome. Mira el sitio web de [Fontawesome](//fortawesome.github.io/Font-Awesome/icons/) para más iconos. Los iconos están representados por su correspondiente clase CSS de Fontawesome. Una habilidad se define como este ejemplo:


```toml
[[params.services.list]]
      icon = "fa-shopping-cart"
      title = "E-Commerce"
      description = "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Minima maxime quam architecto quo inventore harum ex magni, dicta impedit."
```


### Crea tu cartera de productos


Además config.toml, hay data otra subcarpeta llamada `data/projects` que aloja los archivos que aparecerán como proyectos en la sección de cartera. Tal archivo de proyecto podría verse como este escrito en YAML:


```yaml
modalID: 1
title: Round Icons
subtitle: Lorem ipsum dolor sit amet consectetur.
date: 2014-07-05
img: roundicons.png
preview: roundicons-preview.png
client: Start Bootstrap
clientLink: "#"
category: Graphic Design
description: Use this area to describe your project. Lorem ipsum dolor sit amet, consectetur adipisicing elit. Est blanditiis dolorem culpa incidunt minus dignissimos deserunt repellat aperiam quasi sunt officia expedita beatae cupiditate, maiores repudiandae, nostrum, reiciendis facere nemo! <br><br>**Want these icons in this portfolio item sample?** You can download 60 of them for free, courtesy of [RoundIcons.com](//getdpd.com/cart/hoplink/18076?referrer=bvbo4kax5k8ogc), or you can purchase the 1500 icon set [here](//getdpd.com/cart/hoplink/18076?referrer=bvbo4kax5k8ogc).
```

Copie projects dentro de la data carpeta en el directorio raíz de su sitio. Hagamos algunos cambios.

Presta atención al modalID. Debe ser un número entero único y aumentarse con cada nuevo proyecto que desee agregar a la cartera. De lo contrario, no se puede representar el modal correspondiente.

Además, puede usar la sintaxis de Markdown para URL como aquí [text](//url.to/source)en la descripción.

Para darle una imagen a tus proyectos, guárdalos debajo static/img/portfolio. No se olvide de establecer el apropiado nombre de archivo bajo imgen su proyecto.


### Muestra los eventos por venir

Este tema presenta una línea de tiempo para eventos importantes en su empresa. Puede agregar un nuevo evento copiando el siguiente fragmento en la [params.about] sección de config.toml.

```toml
[[params.about.events]]
      img = "1.jpg"
      date = "2009-2011"
      title = "Our Humble Beginnings"
      description = "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Sunt ut voluptatum eius sapiente, totam reiciendis temporibus qui quibusdam, recusandae sit vero unde, sed, incidunt et ea quo dolore laudantium consectetur!"
```

La imagen configurada debajo img necesita ser almacenada en static/img/about. Los eventos se enumerarán de arriba a abajo.

### Renueva las opiniones de los clientes


Deje que los visitantes o posibles clientes sepan que opinan de usted. Para agregar una opinion, pegue el siguiente código en el `config.toml`. El `img` campo se refiere a la imagen mostrada. Pega aquellas imagenes de los que autoricen usar sus imagenes de perfil en static/img/team

```toml
[[params.team.members]]
    img = "1.jpg"
    name = "Kay Garland"
    position = "Lead Designer"
    social = [
        ["fa-twitter", "#"],
        ["fa-facebook", "#"],
        ["fa-linkedin", "#"]
    ]
```

As you can see there's an option to link individual social networks. The first index of the array represents the icon (or CSS class) of [Fontawesome](//fortawesome.github.io/Font-Awesome/icons/). The last index is simply the link to the social network profiles.


### Lista de clientes

También puede mostrar algunos de sus clientes. Para hacerlo, pegue los logotipos del cliente [`static/img/logos`] y agregue el ejemplo a continuación al config.toml.

```toml
[[params.clients]]
    logo = "designmodo.jpg"
    link = "#"
```

***The logos require a dimension of 200 x 50 pixels.***


### Hacer que el formato de contacto funcione

Como esta página será estática, puede usar [formspree.io](//formspree.io/) como proxy para enviar correo electrónico real. Cada mes, los visitantes pueden enviarle hasta mil correos electrónicos sin incurrir en cargos adicionales. Comience la configuración siguiendo los pasos a continuación:

1. Ingrese su dirección de correo electrónico en 'correo electrónico' en el config.toml
2. Suba el sitio generado a su servidor
3. Envíe un correo electrónico falso para confirmar su cuenta
4. Haga clic en el enlace de confirmación en el correo electrónico de [formspree.io](//formspree.io/)
5. Ya terminaste ¡Correo feliz!



Since this page will be static, you can use [formspree.io](//formspree.io/) as proxy to send the actual email. Each month, visitors can send you up to one thousand emails without incurring extra charges. Begin the setup by following the steps below:

1. Enter your email address under 'email' in the [`config.toml`](//github.com/digitalcraftsman/hugo-agency-theme/blob/master/exampleSite/config.toml)
2. Upload the generated site to your server
3. Send a dummy email yourself to confirm your account
4. Click the confirm link in the email from [formspree.io](//formspree.io/)
5. You're done. Happy mailing!


## License

This theme is released under the Apache License 2.0 For more information read the [License](//github.com/digitalcraftsman/hugo-agency-theme/blob/master/LICENSE).


## Expresiones de Gratitud 

Este tema esta personalizado pero so lo debemos al excelente tema  *hugo-agency-theme*

Gracias a

- [David Miller](//github.com/davidtmiller) for creating this theme
- [Steve Francia](//github.com/spf13) for creating Hugo and the awesome community around the project
- [Michael Grosser](https://github.com/stp-ip) for contributing a significant amount of improvements


[deploy]: https://github.com/spencerlyon2/hugo_gh_blog/blob/master/deploy.sh
[dns]: https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/
