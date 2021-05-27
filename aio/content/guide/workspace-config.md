# Configuración del espacio de trabajo Angular

Un archivo llamado `angular.json` al nivel raíz de un [espacio de trabajo](guide/glossary#workspace) Angular proporciona los valores por defecto de configuración específicos del proyecto y todo el espacio de trabajo para construir y desarrollar herramientas proporcionadas por el CLI de Angular. 
Los valores de ruta dados en la configuración son relativos a la carpeta raíz del espacio de trabajo.

## Estructura general de JSON

En la parte superior del `angular.json`, algunas pocas propiedades configuran el espacio de trabajo, y una sección de `projects` contiene las opciones de configuración restantes por proyectos. Los valores por defecto del CLI establecidos al nivel del espacio de trabajo pueden ser anulados por los valores por defecto establecidos al nivel del proyecto, y valores por defecto establecidos al nivel del proyecto pueden ser anulados en la línea de comandos.

Las siguientes propiedades, en la parte superior del archivo, configuran el espacio de trabajo.

* `version`: La versión del archivo de configuración.
* `newProjectRoot`: Ruta donde los nuevos proyectos son creados. Absolutas o relativas a la carpeta de espacio de trabajo.
* `defaultProject`: Nombre por defecto del proyecto para usar en comandos, donde no se proporciona como un argumento. Cuando uses `ng new` para crear una nueva aplicación en un nuevo espacio de trabajo, esta aplicación será el proyecto por defecto en el espacio de trabajo hasta que lo cambies aquí.
* `schematics` : Un grupo de [esquemas](guide/glossary#schematic) que personaliza la opción por defecto del subcomando `ng generate` para este espacio de trabajo. Ver [Generación de esquemas](#schematics) a continuación.
* `projects` : Contiene una subsección por cada proyecto (librería o aplicación) en el espacio de trabajo, con las opciones de configuración por proyecto.

La aplicación inicial que crees con `ng new app_name` se listara debajo de "projects":

<code-example language="json">

"projects": {
"app_name": {
...
}
...
}

</code-example>

Cada aplicación adicional que crees con `ng generate application` tiene su correspondiente prueba end-to-end del proyecto, con su propia sección de configuración.
Cuando creas un proyecto como librería con `ng generate librery`, el proyecto de librería es además agregada a la sección de `projects`.

<div class="alert is-helpful">

Observe que la sección `projects` del archivo de configuración no corresponde exactamente a la estructura de archivos del espacio de trabajo.
- La aplicación inicial creada con `ng new` está en la parte superior de la estructura de archivos del espacio de trabajo.
- Aplicaciones y librerías adicionales van dentro de la carpeta `projects` en el espacio de trabajo.

Para más información, ver [Espacio de trabajo y estructura de archivos del proyecto](guide/file-structure).

</div>

## Modo Estricto

Cuando creas nuevos proyectos y espacios de trabajo, tienes la opción de usar el modo estricto de Angular, que puede ayudarte a escribir mejor código y más mantenible.
Para más información, ver [Modo estricto](/guide/strict-mode).

## Opciones de configuración del proyecto

Las siguientes propiedades de configuración superiores están disponibles para cada proyecto, debajo de `projects:<project_name>`.

<code-example language="json">

    "my-app": {
      "root": "",
      "sourceRoot": "src",
      "projectType": "application",
      "prefix": "app",
      "schematics": {},
      "architect": {}
    }

</code-example>

| PROPIEDADES | DESCRIPCIONES |
| :-------------- | :---------------------------- |
| `root`          | La carpeta raíz para los archivos del proyecto, relativa a la carpeta del espacio de trabajo. Vacía para la aplicación inicial, que reside en la parte superior del espacio de trabajo. |
| `sourceRoot`    | La carpeta raíz de los archivos fuente del proyecto |
| `projectType`   | Admite solo uno de "application" o "library". Una aplicación puede corren independientemente en un navegador, mientras que una librería no. |
| `prefix`        | Una cadena de caracteres que Angular antepone a los selectores generados. Pueden ser personalizados para identificar una aplicación o área de características. |
| `schematics`    | Un grupo de esquemas que personaliza las opciones por defecto del subcomando `ng generate ` para el proyecto. Ver [Esquemas de generación](#schematics) a continuación. |
| `architect`     | Valores por defecto de configuración para los objetivos del constructor del Arquitect para este proyecto. |

{@a schematics}

## Esquemas de generación

Los [esquemas](guide/glossary#schematic) de generación Angular son instrucciones para modificar un proyecto agregando o modificando archivos existentes.
Los esquemas individuales en los valores por defecto del subcomando `ng generate` del CLI de Angular son recolectados en el paquete `@angular`.
Especifique el nombre del esquema para un subcomando en el formato `schematic-package:schematic-name`;
Por ejemplo, el esquema para generar un componente es `@angular:component`.

Los esquemas JSON en los esquemas predeterminados usados en el CLI para generar proyectos y partes de proyectos son recolectados en el paquete [`@schematics/angular`](https://github.com/angular/angular-cli/blob/7.0.x/packages/schematics/angular/application/schema.json).
El esquema describe las opciones disponibles en el CLI para cada uno de los subcomandos `ng generate`, como muestra en el resultado de `--help`.

Los campos dados en el esquema corresponden a los valores de los argumentos permitidos y los valores por defecto para las opciones del subcomando CLI.
Puedes actualizar tu archivo de esquema del espacio de trabajo para establecer diferentes valores por defecto para una opción de subcomando.

{@a architect}

## Opciones de configuración de la herramienta de proyecto

Arquitect es la herramienta que usa el CLI para realizar tareas complejas, como compilación y ejecutar pruebas de funcionamiento.
Arquitect es una cáscara que corre un [constructor](guide/glossary#builder) específico para realizar una tarea dada, de acuerdo a un [objetivo](guide/glossary#target) específico de configuración.
Puedes definir y configurar nuevos constructores y objetivos para extender el CLI.
Ver [Constructores del CLI de Angular](guide/cli-builder).

{@a default-build-targets}

### Constructores y objetivos por defecto del Architec 

Angular define los constructores por defecto que usara con comandos específicos del CLI, o con el comando general `ng run`.
Los esquemas JSON que definen las opciones por defecto de cada uno de estos constructores por defecto son recolectados en el paquete [`@angular-devkit/build-angular`](https://github.com/angular/angular-cli/blob/8.0.x/packages/angular/cli/lib/config/schema.json) 
Los esquemas configuran opciones para los siguientes constructores.

- app-shell
- browser
- dev-server
- extract-i18n
- karma
- protractor
- server
- tslint

### Configurando los objetivos del constructor

La sección `architect` del `angular.json` contiene un grupo de objetivos del Architect.
Muchos de los objetivos corresponden a los comandos CLI que se ejecutan.
Algunos objetivos adicionales predefinidos pueden ser ejecutados usando el comando `ng run`, y puedes definir sus propios objetivos.

Cada objeto de destino especifica el `builder` para el propósito, que es el paquete npm para la herramienta que utiliza Architect.
Adicionalmente, acá objetivo tiene una sección `options` que configura las opciones por defecto para los objetivos, y una sección `configurations` que especifica y nombra configuraciones alternativas para los objetivos.
Ver el ejemplo en [Crear objetivo](#build-target) a continuación.

<code-example language="json">

      "architect": {
        "build": { },
        "serve": { },
        "e2e" : { },
        "test": { },
        "lint": { },
        "extract-i18n": { },
        "server": { },
        "app-shell": { }
      }

</code-example>

* La sección `architect/build` configura los valores por defecto para las opciones del comando `ng build`. 
Ver [build target](#build-target) a continuación para más información.

* La sección `architect/serve` anula los valores por defecto de creacion y suministra los valores por defecto servidos( <--PREGUNTAR A NICO) por el comando `ng serve`. Además de las opciones disponibles por el comando `ng build`, Esta agrega opciones relacionadas con el servicio de la aplicación.

* La sección `architect/e2e` anula las opciones por defecto de creación para la crear aplicaciones de pruebas end-to-end usando el comando `ng e2e`.

* La sección `architect/test` anula las opciones por defecto de creacion para crear pruebas y adicional suministrar los valores por defecto para la ejecucion de pruebas para el comando `ng test`.

* La sección `architect/lint` configura los valores por defecto para las opciones del comando `ng lint`, que realiza analisis de codigo sobre los archivos fuentes del proyecto. La herramienta LINTING por defecto para Angular es [TSLint](https://palantir.github.io/tslint/).

* La sección `architect/extract-i18n` configura los valores por defecto para las opciones de la herramienta `ng-xi18n` usada por el comando `ng xi18n`, la cual extrae  cadenas de texto marcadas como mensaje desde el codigo fuente y las salidas de archivos de traducción.

* La sección `architect/server` configura los valores por defecto para la creacion de una aplicación universal con server-side rendenring, usando el comando `ng run <project>:server`.

* La sección `architect/app-shell` configura los valores por defecto para la creacion de una aplicación carcaza para una aplicación web progresiva (PWA), usando el comando `ng run <project>:app-shell`.

En general, las opciones para las cuales puedes configurar valores por defecto corresponden a las opciones de comandos listados en la [pagina de referencia del CLI](cli) para cada comando.
Observe que todas las opciones en el archivo de configuración debe usar [camelCase](guide/glossary#case-conventions), en vez de dash-case.

{@a build-target}

## Crear destino

La sección `architect/build` configura los valores por defecto para las opciones del comando `ng build`. Este tiene las siguientes propiedades de nivel superior.

| PROPIEDAD | DESCRIPCIÓN |
| :-------------- | :---------------------------- |
| `builder`       | El paquete npm en la herramienta de creación se uso para crear este destino, el constructor por defecto para una aplicación (`ng build myApp`) es `@angular-devkit/build-angular:browser`, que usa el paquete empaquetador[webpack](https://webpack.js.org/). Observe que un constructor diferente es usado para construir una libreria (`ng build myLib`). |
| `options`       |Esta sección contiene las opciones por defecto para crear destino, usadas cuando una configuración alternativa no nombrada es especificada. Ver [Crear destinos por defecto](#default-build-targets) a continuación. |
| `configurations`| Esta seccion define y nombra configuraciones alternativas para diferentes destinos deseados. Este contiene una seccion para cada configuracion nombrada, que establece las opciones por defecto para los ambientes planeados. Ver [configuraciones de creación alternas](#build-configs) a continuación.|(PREGUNTAR A NICO)


{@a build-configs}

### Configuraciones de creación alternas

Por defecto, una configuracion de `production` es definida, y el comando `ng build` tiene la opción `--prod` que crea usando esta configuracion. La configuracion de `production` establece valor por defecto que optimizan la aplicación de varias formas, como empaquetar archivos, minimizando el exceso de espacios en blanco, removiendo comentarios y codigo muerto, y reescribiendo codigo para usar nombres breves y cripticos ("minificación").

Puedes definir y nombrar configuraciones alternas adicionales (como `stage`, por ejemplo) apropiandas para tu proceso de desarrollo. algunos ejemplo de diferentes configuraciones de creacion son `stable`, `archive` y `next` usados por el mismo AIO, y las configuraciones locales especificas individuales requeridas para la creacion de versiones localizadas de una aplicación. Para más detalles, ver [Internacionalizacion (i18n)](guide/i18n#merge-aot).

Puedes seleccionar una configuración alterna pasando el nombre en la linea de comandos con la bandera `--configuration`

Puedes ademas pasar más que un nombre de configuración como una lista separada por comas. Por ejemplo, para aplicar las configuraciones creadas de `stage` y `fr`, use el comando `ng build --configuration stage,fr`. En este caso, el comando analiza las configuraciones nombradas de izquierda a derecha. Si multiples configuraciones cambian la misma configuracion, el ultimo valor establecido es el valor final.

Si la bandera `--prod` es usada en la linea de comandos, esta sé aplica primero, y estas configuraciones puedes ser anuladas por cualquier configuración especificada por medio de la bandera `--configuration`.

{@a build-props}

### Opciones adicionales de creacion y prueba

Las opciones configurables por defecto o un destino de creacion generalmente corresponden a las opciones disponibles para los comandos [`ng build`](cli/build), [`ng serve`](cli/serve), y [`ng test`](cli/test). Para mas detalles de aquellas opciones y sus posibles valores, ver el [Referencia del CLI](cli). 

Algunas opciones adicionales puedes solo ser establecidas a traves de el archivo de configuracion, o ya sea directamente editando o con el comando [`ng config`](cli/config).

| PROPIEDADES OPCIONALES | DESCRIPCIÓN |
| :------------------------- | :---------------------------- |
| `assets`                   | Un objeto contiene rutas a activos estaticos para agregar el contexto global del proyecto. Las rutas por defecto apuntan a el archivo icono del proyecto y su carpeta `assets`. Ver mas en [Configuración de activos](#asset-config) a continuación |
| `styles`                   | Un array de archivos de estilo para agregar al contexto global del proyecto. El CLI de Angular soporta importaciones de CSS y todos los principales pre-pocesadores de CSS: [sass/scss](http://sass-lang.com/), [less](http://lesscss.org/), y [stylus](http://stylus-lang.com/). Ver mas en [Configuración de estilos y scripts](#style-script-config) a continuación. |
| `stylePreprocessorOptions` | Un objeto contiene pares de opciones y valores para pasar a los pre-procesadores de estilo. Ver más en [Configuración de estilos y scripts](#style-script-config) a continuación.|
| `scripts`                  | Un objeto que contiene archivos de script JavaScript para agregar a el contexto global del proyecto. Los scripts son cargados exactamente como si los hubieras agregado en una etiqueta `<script>` dentro de `index.html`. Ver más en [Configuración de estilos y scripts](#style-script-config) a continuación.|
| `budgets`                  | Tipo de tamaño presupuestado por defecto y umbrales para toda tu aplicación o partes de esta. Puedes configurar el constructor para notificar una advertencia o un error cuando la salida alcance o exceda un umbrar de tamaño. Ver [configurar tamaño presupuestado](guide/build#configure-size-budgets).(No disponible en la sección de `test`) | 
| `fileReplacements`         | Un objeto que contiene archivos y sus reemplazos en tiempo de copilación. Ver más en [Configurar reemplazos de archivos especificos de destino](guide/build#configure-target-specific-file-replacements).| 

{@a complex-config}

## Valores complejos de configuración

Las opciones `assets`, `styles`, y `scripts` pueden tener o un valor de cadena de ruta simple, o valores de objeto con campos especificos.
Las opciones `sourceMap` y `optimization` pueden ser establecidas con un valor Booleano simple con un comando bandera, pero puede ademas ser dado un valor complejo usando el archivo de configuración.
Las siguientes secciones proveen mas detalles de como estos valores complejos son usados en cada caso .

{@a asset-config}

### Configuración de activos

Cada destino de configuración `build` puede incluir un array `assets` que lista archivos o carpetas que quieras copiar como es cuando construyes tu proyecto.
Por defecto, la carpeta `src/assets/` y `src/favicon.ico` son copiadas.

<code-example language="json">

"assets": [
  "src/assets",
  "src/favicon.ico"
]

</code-example>