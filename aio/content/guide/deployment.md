# Despliegue 

Cuando estas listo para hacer el despliegue de tu aplicación en Angular hacia un servidor, tienes varias opciones para hacer el despliegue.

{@a dev-deploy}
{@a copy-files}

## Opciones de un despliegue simple

Antes de desplegar su aplicación completamente, puede probar el proceso, crear la configuración, y el comportamiento de la implementación mediante uso de una de las técnicas provisionales.

### Creando y sirviendo desde el disco.

Durante el desarrollo, típicamente se usa el comando `ng serve` para crear, visualizar y servir la aplicación desde la memoria local usando [webpack-dev-server](https://webpack.js.org/guides/development/#webpack-dev-server).

Usando `ng build` y `ng serve` limpian la carpeta de salida antes de crear el proyecto, pero solo el comando `ng build` escribe los artefactos de construcción generados en la carpeta de salida.

<div class="alert is-helpful">

El folder de salida es `dist/project-name/` por defecto.
Para obtener la salida en un folder diferente, cambia el `outputPath` en `angular.json`.

</div>

Conforme se termina el proceso de desarrollo, servir el contenido que se encuentra en la carpeta de salida hacia un servidor web local, le dará una mejor idea de como se comportara su aplicación cuando se despliegue en un servidor remoto.

Necesitara dos terminales para tener la experiencia de recarga automática.

* En la primera terminal, ejecuta el comando [`ng build` command](cli/build) en modo *observador* para compilar la aplicación que se encuentra en la carpeta `dist`.

  <code-example language="none" class="code-shell">

   ng build --watch

  </code-example>

  Asi como el comando `ng-serve`, esto regenerara los archivos de salida cuando cambie la fuente de los archivos.

* En la segunda terminal, instala un servidor web (como [lite-server](https://github.com/johnpapa/lite-server))), y ejecutalo en la carpeta de salida. Por ejemplo:

  <code-example language="none" class="code-shell">

   lite-server --baseDir="dist/project-name"

  </code-example>

   El servidor automáticamente se recargara en el navegador con los nuevos archivos de salida.

<div class="alert is-critical">

Este método es solamente para desarrollo y ambiente de pruebas, y no es soportado o seguro para desplegar la aplicación a producción.

</div>

### Despliegue automático con la CLI

El comando de Angular CLI `ng deploy` (lanzado en la version 8.3.0) ejecuta `deploy` [CLI builder](guide/cli-builder) asociado con su proyecto. Varios constructores de terceros tienen la capacidad de desplegar en diferentes plataformas. Puede agregar cualquiera de ellos para ejecutar su proyecto como `ng add [nombre del paquete]`.

Cuando agrega un paquete con capacidad de despliegue, Se ejecutara automáticamente la actualización en su entorno de configuración (archivo `angular.json`) con una sección `deploy` para el proyecto seleccionado. Puede usar el comando `ng deploy` para desplegar su proyecto.

Por ejemplo, el siguiente comando automáticamente desplegara su proyecto en Firebase.

<code-example language="none" class="code-shell">
ng add @angular/fire
ng deploy
</code-example>

El comando es interactivo. En este caso, debe tener o crear una cuenta de Firebase, y autenticarse con su cuenta. El comando solicita que seleccione un proyecto en Firebase para el despliegue

Después que el comando produce una compilación optima de la aplicación (equivalente a `ng deploy --prod`), se cargaran los archivos activos a Firebase.

En la tabla a continuación, se encuentra una lista de paquetes, con los cuales se implementa el despliegue hacia diferentes plataformas. El comando `deploy` para cada paquete puede requerir diferentes opciones en la linea de comandos. Para mas información puede acceder a los enlaces asociados en cada nombre de los paquetes:

| Desplegar hacia                                                 | Paquete                                                                        |
|---------------------------------------------------------------|--------------------------------------------------------------------------------|
| [Firebase hosting](https://firebase.google.com/docs/hosting)  | [`@angular/fire`](https://npmjs.org/package/@angular/fire)                     |
| [Azure](https://azure.microsoft.com/en-us/)                   | [`@azure/ng-deploy`](https://npmjs.org/package/@azure/ng-deploy)               |
| [Now](https://zeit.co/now)                                    | [`@zeit/ng-deploy`](https://npmjs.org/package/@zeit/ng-deploy)                 |
| [Netlify](https://www.netlify.com/)                           | [`@netlify-builder/deploy`](https://npmjs.org/package/@netlify-builder/deploy) |
| [GitHub pages](https://pages.github.com/)                     | [`angular-cli-ghpages`](https://npmjs.org/package/angular-cli-ghpages)         |
| [NPM](https://npmjs.com/)                                     | [`ngx-deploy-npm`](https://npmjs.org/package/ngx-deploy-npm)                   |
| [Amazon Cloud S3](https://aws.amazon.com/s3/?nc2=h_ql_prod_st_s3) | [`@jefiozie/ngx-aws-deploy`](https://www.npmjs.com/package/@jefiozie/ngx-aws-deploy) |

Si se esta desplegando hacia un servidor auto gestionado o no se encuentra un constructor para su plataforma favorita, se puede crear un constructor que permita usar el comando `ng deploy`, o lea esta documentación para aprender como desplegar manualmente su aplicación.

### Despliegue básico a un servidor remoto

Para un despliegue simple, se debe crear un constructor de producción y copiar el directorio de salida hacia un servidor web.

1. Empieza con el constructor de producción:

  <code-example language="none" class="code-shell">

    ng build --prod

  </code-example>

2. Copiar _everything_ desde la carpeta de salida (`dist/` por defecto) hacia el folder del servidor.

3. Configure el servidor para redirigir los archivos no existentes hacia `index.html`.

Aprenda mas acerca de redirecciones en el servidor [a continuación](#fallback).

Este es un despliegue simple de producción para su aplicación.

{@a deploy-to-github}

### Desplegar en Github pages

Otro forma de desplegar su aplicación de Angular usando [GitHub Pages](https://help.github.com/articles/what-is-github-pages/).

1. Necesita [crear una cuenta en GitHub](https://github.com/join) si es que no tiene una, para después [crear un repositorio](https://help.github.com/articles/create-a-repo/) para su proyecto.

Guarde su nombre de usuario y el nombre del proyecto en GitHub.

2. Cree su proyecto usando el nombre de su proyecto en GitHub, con el comando [`ng build`](cli/build) en Angular CLI y las opciones que a continuación se muestran:

  <code-example language="none" class="code-shell">

    ng build --prod --output-path docs --base-href /&lt;project_name&gt;/

  </code-example>

1. Cuando su proyecto esta construido completamente, haga una copia de `docs/index.html` y asigne el nombre de `docs/404.html`.

2. Haga un commit de sus cambios y suba los cambios a su repositorio.

3. En la pagina del proyecto en GitHub, configure todo para [publicar su aplicación desde la carpeta de documentos](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/#publishing-your-github-pages-site-from-a-docs-folder-on-your-master-branch).

Puede ver su aplicación desde la pagina `https://<user_name>.github.io/<project_name>/`.

<div class="alert is-helpful">

Verifique [angular-cli-ghpages](https://github.com/angular-buch/angular-cli-ghpages), un paquete con todas las funciones que hace todo esto por usted y tiene una funcionalidad adicional.

</div>

<hr>

{@a server-configuration}


## Configuración del servidor

Esta sección cubre los cambios que debe hacer en el servidor o los archivos que se desplegaron en el servidor.

{@a fallback}

### Las aplicaciónes con rutas deben redirigirse a `index.html`

Las aplicaciónes de Angular son los candidatos perfectos para desplegar con un servidor HTML estático.
No se necesita un server-side engine para crear su aplicación dinámica porque
Angular hace eso desde el lado del cliente.

Si la aplicación usar rutas de Angular, debe configurar el servidor
para que regresen a la pagina principal (`index.html`) cuando se pida una ruta que no existe.

{@a deep-link}

Una aplicación con rutas debe soportar "enlaces profundos".
Un _deep link_ es una URL que especifica la ruta a un componente dentro de la aplicación.
Por ejemplo, `http://www.mysite.com/heroes/42` es un _deep link_ para la pagina que contiene el detalle del héroe,
la cual despliega el héroe con el `id: 42`.

No hay problema cuando el usuario navega hacia la URL dentro de un cliente en ejecución.
Las rutas de Angular interpreta la URL y las rutas de esa pagina.

Pero al hacer clic en un correo electrónico, se debe ingresar en la barra de direcciones,
o simplemente refresque el navegador mientras se encuentra en la pagina del detalle del héroe &mdash;
todas estas acciones son manejadas por el navegador, _outside_ fuera de la aplicación.
El navegador hace la redirección con un pedido hacia la URL del servidor, pasando por alto el router.

Un servidor estático retorna rutinariamente index.html cuando recibe un pedido para `http://www.misitio.com/`.
Pero es rechazada para `http://www.mysite.com/heroes/42` y retorna un error `404 - Not Found` *a menos* 
que este este configurado para devolver `index.html`.

#### Ejemplos de configuración de reserva

No hay una sola manera para trabajar con cada servidor.
Las siguientes secciones se describe la configuración para algunos de los servidores mas populares.
La lista de ninguna manera es exhaustivo, pero debería proporcionarle un buen punto de partida.

* [Apache](https://httpd.apache.org/): agregar a
[regla de sobre escritura](http://httpd.apache.org/docs/current/mod/mod_rewrite.html) a el archivo `.htaccess` como muestra
  (https://ngmilk.rocks/2015/03/09/angularjs-html5-mode-or-pretty-urls-on-apache-using-htaccess/):

  <code-example>
    RewriteEngine On
    &#35 If an existing asset or directory is requested go to it as it is
    RewriteCond %{DOCUMENT_ROOT}%{REQUEST_URI} -f [OR]
    RewriteCond %{DOCUMENT_ROOT}%{REQUEST_URI} -d
    RewriteRule ^ - [L]<br>
    &#35 If the requested resource doesn't exist, use index.html
    RewriteRule ^ /index.html
  </code-example>


* [Nginx](http://nginx.org/): usa `try_files`, como se describe en
[Front Controller Pattern Web Apps](https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/#front-controller-pattern-web-apps),
modifica el servidor `index.html`:

  ```
  try_files $uri $uri/ /index.html;
  ```

* [Ruby](https://www.ruby-lang.org/): crea un servidor Ruby usando ([sinatra](http://sinatrarb.com/)) con un archivo basico que configure el servidor `server.rb`:

  ``` ruby
  require 'sinatra'

  # Folder structure
  # .
  # -- server.rb
  # -- public
  #    |-- dist
  #        |-- index.html

  get '/' do
      folderDir = settings.public_folder + '/dist'  # ng build output folder
      send_file File.join(folderDir, 'index.html')
  end
  ```


* [IIS](https://www.iis.net/): agregar la regla de sobreescritura a `web.config`, similar al mostrado
[aqui](http://stackoverflow.com/a/26152011/2116927):

  <code-example format='.' language="xml">
    &lt;system.webServer&gt;
      &lt;rewrite&gt;
        &lt;rules&gt;
          &lt;rule name="Angular Routes" stopProcessing="true"&gt;
            &lt;match url=".*" /&gt;
            &lt;conditions logicalGrouping="MatchAll"&gt;
              &lt;add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" /&gt;
              &lt;add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" /&gt;
            &lt;/conditions&gt;
            &lt;action type="Rewrite" url="/index.html" /&gt;
          &lt;/rule&gt;
        &lt;/rules&gt;
      &lt;/rewrite&gt;
    &lt;/system.webServer&gt;
  </code-example>


* En [GitHub Pages](https://pages.github.com/): no puedes
[configurar directamente](https://github.com/isaacs/github/issues/408)
el servidor de GitHub, pero puedes agregar una pagina 404.
Solo copia `index.html` dentro `404.html`.
Esto servirá una respuesta 404, y el navegador procesara la pagina y la cargara apropiadamente.
También otra buena idea es
[servir desde `docs/` en master](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/#publishing-your-github-pages-site-from-a-docs-folder-on-your-master-branch)
y
[crear un archivo `.nojekyll`](https://www.bennadel.com/blog/3181-including-node-modules-and-vendors-folders-in-your-github-pages-site.htm)


* [Firebase hosting](https://firebase.google.com/docs/hosting/): Agregar una
[regla de sobreescritura](https://firebase.google.com/docs/hosting/url-redirects-rewrites#section-rewrites).

  <code-example language="json">
    "rewrites": [ {
      "source": "**",
      "destination": "/index.html"
    } ]
  </code-example>

{@a cors}

### Requiriendo servicios desde diferentes servidores (CORS)

Los desarrolladores de Angular pueden encontrar un error
<a href="https://en.wikipedia.org/wiki/Cross-origin_resource_sharing" title="Cross-origin resource sharing">
<i>cross-origin resource sharing</i></a> cuando se hace una solicitud al servicio (normalmente una solicitud de servicio de datos)
hacia un servidor de otra aplicación que no sea el host del propio servidor.
Los navegadores prohíben estas solicitudes a menos que el servidor las permita explícitamente.

No existe nada que el cliente de la aplicación pueda hacer con estos errores.
El servidor debe ser configurado para aceptar las solicitudes de las aplicaciónes.
Para mas información acerca de como activar las CORS para servidores específicos en
<a href="http://enable-cors.org/server.html" title="Enabling CORS server">enable-cors.org</a>.

<hr>

{@a optimize}

## Optimizacion en producción

La bandera `--prod` _meta-flag_ compromete las siguientes características para la optimización.

* [Ahead-of-Time (AOT) Compilation](guide/aot-compiler): pre compila las plantillas de los componentes de Angular
* [Modo de producción](#enable-prod-mode): despliega un ambiente de producción que habilita el _production mode_.
* Empaquetado: concatena tus aplicación y los archivos de las librerías dentro de algunos empaquetados.
* Minificación: remueve los espacios en blanco excedentes, comentarios y token adicionales.
* Uglificación: reescribe el código para hacerlo mas corto, encripta las variables y los nombres de las funciones.
* Eliminación de código muerto: remueve módulos sin referencia y el código que no es usado en la aplicación.

Ver [`ng build`](cli/build) para mas información acerca de las opciones de construcción y que hacen en el CLI.


{@a enable-prod-mode}

### Habilitar el tiempo de ejecución en modo de producción

Adicionalmente para optimizar la creación de aplicaciónes, Angular también posee tiempo de ejecución en el modo de producción. Las aplicaciónes de Angular se ejecutan en modo de desarrollo por defecto, tal como se puede ver en los siguientes mensajes de la consola:

<code-example format="nocode">

  Angular se esta ejecutando en modo desarrollador. Para habilitar el modo de producción habilite enableProdMode().

</code-example>

Cambiando a _production mode_ hará que se ejecute mas rápido al deshabilitar las comprobaciones especificas del desarrollo, como los ciclos de detección de cambio dual.

Cuando la habilita las compilaciones de producción a través de la bandera `--prod` al termino del comando, el modo de producción en tiempo de ejecución también se habilita.

{@a lazy-loading}

### Carga lenta

Puede reducir dramáticamente el tiempo de carga solo cargando los módulos de la aplicación que
solo estará presentes cuando se inicie la aplicación.

Para configurar las rutas en Angular para cargar los demás módulos (u otros asociados en el código), ya sea
[esperando hasta que se inicie la aplicación](guide/router-tutorial-toh#preloading  "Preloading")
o bien, con la [_lazy loading_](guide/router#lazy-loading "Lazy loading")
dependiendo la demanda.

<div class="callout is-helpful">
<header>No importe algo de un modulo cargándolo de forma diferida</header>

Si piensa hacer una carga lenta de un modulo de forma diferida, sea cuidadoso
de no importarlo dentro de un archivo que se inicia con la aplicación (como la raíz en `AppModule`)
si lo hace, el modulo se cargara inmediatamente.

La configuración de agrupación debe tener en cuenta la carga diferida.
Debido a que los módulos de carga diferida no se importan en JavaScript, los paquetes los excluyen de forma predeterminada.
Los empaquetadores no conocen la configuración del enrutador y no pueden crear paquetes separados para módulos de carga diferida.
Tendría que crear estos paquetes manualmente.

El CLI ejecuta el 
[Angular Ahead-of-Time Webpack Plugin](https://github.com/angular/angular-cli/tree/master/packages/ngtools/webpack)
que reconoce automáticamente los `NgModules` cargados de forma diferida y crea paquetes separados para ellos.

</div>

{@a measure}

### Medición del rendimiento

Puede tomar una mejor decision acerca de la optimizacion cuando conoce y tiene un claro entendimiento de
porque su aplicación es lenta.
La causa es posible no sea lo que piense.
Puede llegar a gastar mucho tiempo y dinero optimizando algo que no le dará un beneficio tangible o a veces la aplicación sera mas lenta.
Puede medir el comportamiento de su aplicación cuando ejecuta los ambientes correctos y de importancia para usted.

La
<a href="https://developers.google.com/web/tools/chrome-devtools/network-performance/understanding-resource-timing" title="Chrome DevTools Network Performance">
herramienta de la pagina en Chrome DevTools Network Performance</a> es un buen lugar para comenzar a aprender como medir el rendimiento de su aplicación.

La herramienta [WebPageTest](https://www.webpagetest.org/) es otra buena opción
que también le ayuda a verificar si su despliegue ha sido exitoso.

{@a inspect-bundle}

### Inspeccionado de paquetes

La herramienta <a href="https://github.com/danvk/source-map-explorer/blob/master/README.md">source-map-explorer</a>
es otro buen camino para inspeccionar los paquetes generados de JavaScript después de la creación de producción.

Instala `source-map-explorer`:

<code-example language="none" class="code-shell">

  npm install source-map-explorer --save-dev

</code-example>

Crea tu aplicación para producción _including the source maps_

<code-example language="none" class="code-shell">

  ng build --prod --source-map

</code-example>

Los paquetes generados esta en la carpeta `dist/`.

<code-example language="none" class="code-shell">

  ls dist/*.bundle.js

</code-example>

Ejecuta el explorador para generar una representación gráfica de uno de los paquetes.
El siguiente ejemplo muestra el gráfico del paquete _main_.

<code-example language="none" class="code-shell">

  node_modules/.bin/source-map-explorer dist/main.*.bundle.js

</code-example>

El `source-map-explorer` analiza el mapa fuente generado con el paquete y crea un mapa de todas las dependencias,
mostrando exactamente todas las clases incluidas en el paquete.

La salida del paquete _main_ puede ser llamado `cli-quickstart` por ejemplo.

<div class="lightbox">
  <img src="generated/images/guide/deployment/quickstart-sourcemap-explorer.png" alt="quickstart sourcemap explorer">
</div>

{@a base-tag}

## La etiqueta `base`

El HTML [_&lt;base href="..."/&gt;_](/guide/router)
especifica una ruta base para resolver URL relativas a activos como imágenes, scripts y hojas de estilo.
Por ejemplo, teniendo `<base href="/my/app/">`, el navegador resolverá la URL como `some/place/foo.jpg`
dentro de el llamado del servidor para `my/app/some/place/foo.jpg`.
Durante la navegación, La ruta de Angular usara _base href_ como base para el componente, la plantilla y los archivos del modulo.

<div class="alert is-helpful">

Puede ver también [*APP_BASE_HREF*](api/common/APP_BASE_HREF "API: APP_BASE_HREF") como alternativa.

</div>

En desarrollo, típicamente el servidor comienza cargando el `index.html`.
Eso esta en la carpeta raíz y se agrega como `<base href="/">` al principio del `index.html` porque `/` es la raíz de la aplicación.

Pero en el servidor compartido o de producción, puede servir la aplicación desde una subcarpeta.
Por ejemplo cuando la URL de carga se algo como `http://www.mysite.com/my/app/`,
la subcarpeta es `my/app/` y debera agregar `<base href="/my/app/">` para la version en el servidor del `index.html`.

Cuando la etiqueta `base` no se haya configurado, la aplicación fallara en la carga y el navegador imprimirá el error `404 - Not Found` en consola
para los archivos no encontrados. Vea donde _tried_ encontrar los archivos y ajuste la etiqueta base de manera apropiada.

{@a differential-loading}

## Carga diferencial

En la aplicación web, quiere asegurar que la aplicación se compatible con la mayoría de los navegadores.
Incluso a medida que JavaScript continúa evolucionando, con la introducción de nuevas funciones, no todos los navegadores se actualizan con soporte para estas nuevas funciones al mismo ritmo.

El código que se encuentra usando TypeScript es compilado y empaquetado para  ES2015, y asi la sintaxis de JavaScript es compatible con la mayoría de los navegadores.
Todos los navegadores modernos soportan ES2015 o menores, pero en la mayoría de los casos, debe tener en cuenta los navegadores que no cuentan con esta compatibilidad.
Cuando el objetivo sean navegadores antiguos, [polyfills](guide/browser-support#polyfills) puede cerrar la brecha al proporcionar una funcionalidad que no existe en las versiones anteriores de JavaScript compatibles con esos navegadores.

Para maximizar la compatibilidad, puede enviar un solo paquete que incluya todo su código compilado, más cualquier polyfills que pueda ser necesario.
Los usuarios con navegadores modernos, sin embargo, no deberían tener que pagar el precio de un paquete de mayor tamaño que viene con polyfills que no necesitan.
La carga diferencial, que es compatible con Angular CLI versión 8 y superior, puede ayudar a resolver este problema.

La carga diferencial es una estrategia que permite que su aplicación web admita varios navegadores, pero solo cargue el código necesario que el navegador necesita. Cuando la carga diferencial está habilitada, la CLI crea dos paquetes separados como parte de su aplicación implementada.

* El primer paquete contiene la sintaxis moderna de ES2015. Este paquete aprovecha la compatibilidad integrada en los navegadores modernos, envía menos polyfills y da como resultado un tamaño de paquete más pequeño.

* El segundo paquete contiene código en la antigua sintaxis de ES5, junto con todos los polyfills necesarios. Este segundo paquete es más grande, pero es compatible con navegadores más antiguos.

### Creadores diferenciales

Cuando el despliegue utiliza el proceso de creación de Angular CLI, puede elegir como y cuando soportara la carga diferencial.
El [comando `ng build`](cli/build) consulta la configuración del navegador y el destino de compilación configurado para determinar si se requiere compatibilidad con navegadores heredados y si la compilación debe producir los paquetes necesarios para la carga diferencial.
Las siguientes configuraciónes determinan sus requerimientos.

* Browserslist

   El archivo de configuración Browserslist esta incluido en la [estructura del proyecto de la aplicación](guide/file-structure#application-configuration-files) y provee del mínimo soporte a la aplicación desde el navegador. Ver la [especificación Browserslist](https://github.com/browserslist/browserslist) para todas las opciones de la configuración.

* Configuración de TypeScript

   En el archivo de configuración de TypeScript, la opción "target" en la sección `compilerOptions` determina la versión de destino de ECMAScript en la que se compila el código.
   Los navegadores modernos admiten ES2015 de forma nativa, mientras que ES5 se usa más comúnmente para admitir navegadores heredados.

<div class="alert is-helpful">
   
   Actualmente, la carga diferencial solo se admite cuando se usa "es2015" como destino de compilación. Cuando se usa con objetivos superiores a "es2015", el proceso de compilación emite una advertencia.

</div>

Para una compilación de desarrollo, la salida producida por `ng build` es más simple y más fácil de depurar, lo que le permite depender menos de los mapas fuente del código compilado.

Para una compilación de producción, su configuración determina qué paquetes se crean para la implementación de su aplicación.
Cuando es necesario, el archivo `index.html` también se modifica durante el proceso de compilación para incluir etiquetas de secuencia de comandos que permiten la carga diferencial, como se muestra en el siguiente ejemplo.

<code-example language="html" header="index.html">
&lt;body>
  &lt;app-root>&lt;/app-root>
  &lt;script src="runtime-es2015.js" type="module">&lt;/script>
  &lt;script src="runtime-es5.js" nomodule>&lt;/script>
  &lt;script src="polyfills-es2015.js" type="module">&lt;/script>
  &lt;script src="polyfills-es5.js" nomodule>&lt;/script>
  &lt;script src="styles-es2015.js" type="module">&lt;/script>
  &lt;script src="styles-es5.js" nomodule>&lt;/script>
  &lt;script src="vendor-es2015.js" type="module">&lt;/script>
  &lt;script src="vendor-es5.js" nomodule>&lt;/script>
  &lt;script src="main-es2015.js" type="module">&lt;/script>
  &lt;script src="main-es5.js" nomodule>&lt;/script>
&lt;/body>
</code-example>

Cada etiqueta del script tiene un `type="module"` o un atributo `nomodule`. Los navegadores con soporte nativo para módulos ES solo cargan los scripts con el atributo de tipo `module` e ignoran los scripts con el atributo` nomodule`. Los navegadores heredados solo cargan los scripts con el atributo `nomodule` e ignoran las etiquetas del script con el tipo de` module` que cargan los módulos ES.

<div class="alert is-helpful">

   Algunos navegadores heredados aún descargan ambos paquetes, pero solo ejecutan los scripts apropiados según los atributos mencionados anteriormente. Puedes leer más sobre el tema [aqui](https://github.com/philipwalton/webpack-esnext-boilerplate/issues/1).

</div>

### Configuración de carga diferencial

Para incluir la carga diferencial en la compilación de su aplicación, debe configurar el Browserslist y la configuración de TypeScript en los archivos de su aplicación.

Los siguientes ejemplos muestran un archivo `browserlistrc` y` tsconfig.json` para una aplicación Angular recién creada. En esta configuración, los navegadores heredados como IE 9-11 se ignoran y el objetivo de compilación es ES2015.

<code-example language="none" header="browserslistrc">
# El sistema de compilación utiliza este archivo para ajustar la salida de CSS y JS para admitir los navegadores especificados a continuación.
# Para obtener información adicional sobre las opciones de formato y regla, consulte:
# https://github.com/browserslist/browserslist#queries

# Para obtener la lista completa de navegadores compatibles con el marco Angular, consulte:
# https://angular.io/guide/browser-support

# Puede ver qué navegadores fueron seleccionados por sus consultas ejecutando:
#   npx browserslist

last 1 Chrome version
last 1 Firefox version
last 2 Edge major versions
last 2 Safari major version
last 2 iOS major versions
Firefox ESR
not IE 9-11 # For IE 9-11 support, remove 'not'.
</code-example>

<code-example language="json" header="tsconfig.json">

{
  "compileOnSave": false,
  "compilerOptions": {
    "baseUrl": "./",
    "outDir": "./dist/out-tsc",
    "sourceMap": true,
    "declaration": false,
    "module": "esnext",
    "moduleResolution": "node",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "importHelpers": true,
    "target": "es2015",
    "typeRoots": [
      "node_modules/@types"
    ],
    "lib": [
      "es2018",
      "dom"
    ]
  }
}

</code-example>

<div class="alert is-important">

   Para ver que navegador es soportado y determinar que configuración se asemeja a los requerimientos de su navegador, vea la [pagina de compatibilidad de Browserslist](https://browserl.ist/?q=%3E+0.5%25%2C+last+2+versions%2C+Firefox+ESR%2C+not+dead%2C+not+IE+9-11).

</div>

La configuración de la lista de navegadores le permite ignorar los navegadores que no son compatibles con ES2015. En este caso, se produce una sola construcción.

Si su configuración de Browserslist incluye soporte para navegadores heredados, el destino de compilación en la configuración de TypeScript determina si la compilación admitirá la carga diferencial.

{@a configuration-table }

| Browserslist | ES target | Build result |
| -------- | -------- | -------- |
| ES5 soporte deshabilitado | es2015  | Empaquetado simple, ES5 no requerido |
| ES5 soporte habilitado  | es5     | Empaquetado simple con condiciones polyfills solo es para ES5 |
| ES5 soporte habilitado  | es2015  | Carga diferencial (dos o mas condiciones polyfills) |

{@a test-and-serve}

## Desarrollo local en navegadores viejos

En Angular CLI versión 8 y superior, la carga diferencial está habilitada de forma predeterminada para el comando `ng build`.
Sin embargo, los comandos `ng serve`,` ng test` y `ng e2e` generan una única compilación de ES2015 que no se puede ejecutar en navegadores antiguos que no admiten los módulos, como IE 11.

Si desea ejecutar el código ES5 durante el desarrollo, puede deshabilitar la carga diferencial por completo.
Sin embargo, para mantener los beneficios de la carga diferencial, una mejor opción es definir múltiples configuraciónes para "ng serve", "ng e2e" y "ng test".

{@a differential-serve}
{@a configuring-serve-for-es5}
### Configurar el servicio para ES5

Para hacer esto para `ng serve`, cree un nuevo archivo,` tsconfig-es5.app.json` junto a `tsconfig.app.json` con el siguiente contenido.

<code-example language="json">

{
 "extends": "./tsconfig.app.json",
 "compilerOptions": {
     "target": "es5"
  }
}

</code-example>

En `angular.json` agregue dos nuevas secciones en la configuración debajo de `build` y `serve` para apuntar a la nueva configuración de TypeScript.

<code-example language="json">

"build": {
  "builder": "@angular-devkit/build-angular:browser",
  "options": {
      ...
  },
  "configurations": {
    "production": {
        ...
    },
    "es5": {
      "tsConfig": "./tsconfig-es5.app.json"
    }
  }
},
"serve": {
  "builder": "@angular-devkit/build-angular:dev-server",
  "options": {
      ...
  },
  "configurations": {
    "production": {
     ...
    },
    "es5": {
      "browserTarget": "&lt;app-name&gt;:build:es5"
    }
  }
},

</code-example>

Luego puede ejecutar el comando `ng serve` con esta configuración. Asegúrese de reemplazar `<app-name>` (en `" <app-name>: build: es5 "`) con el nombre real de la aplicación, como aparece en `projects` en` angular.json`. Por ejemplo, si el nombre de su aplicación es `myAngularApp`, la configuración se convertirá en` "browserTarget": "myAngularApp: build: es5" `.

<code-example language="none" class="code-shell">

ng serve --configuration es5

</code-example>

{@a differential-test}

### Configuración del comando de prueba

Cree un nuevo archivo, `tsconfig-es5.spec.json` junto a` tsconfig.spec.json` con el siguiente contenido.

<code-example language="json">

{
 "extends": "./tsconfig.spec.json",
 "compilerOptions": {
     "target": "es5"
  }
}

</code-example>

<code-example language="json">

"test": {
  "builder": "@angular-devkit/build-angular:karma",
  "options": {
      ...
  },
  "configurations": {
    "es5": {
      "tsConfig": "./tsconfig-es5.spec.json"
    }
  }
},

</code-example>

Luego puede ejecutar las pruebas con esta configuración

<code-example language="none" class="code-shell">

ng test --configuration es5

</code-example>

### Configurando el comando e2e

Cree una [configuración de servicio ES5](guide/deployment#configuring-serve-for-es5) como se explica anteriormente, y configure una configuración para el destino E2E.

<code-example language="json">

"e2e": {
  "builder": "@angular-devkit/build-angular:protractor",
  "options": {
      ...
  },
  "configurations": {
	  "production": {
		  ...
	  },
    "es5": {
      "devServerTarget": "&lt;app-name&gt;:serve:es5"
    }
  }
},

</code-example>

Luego puede ejecutar el comando `ng e2e` con esta configuración. Asegúrese de reemplazar `<app-name>` (en `" <app-name>: serve: es5 "`) con el nombre real de la aplicación, como aparece en `projects` en` angular.json`. Por ejemplo, si el nombre de su aplicación es `myAngularApp`, la configuración se convertirá en` "devServerTarget": "myAngularApp: serve: es5" `.

<code-example language="none" class="code-shell">

ng e2e --configuration es5

</code-example>
