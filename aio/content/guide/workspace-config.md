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

| PROPIEDAD | DESCRIPCIONES |
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
Ver [crear destino](#build-target) a continuación para más información.

* La sección `architect/serve` anula los valores por defecto de creación y suministra los valores por defecto servidos por el comando `ng serve`. Además de las opciones disponibles por el comando `ng build`, Este agrega opciones relacionadas con el servicio de la aplicación.

* La sección `architect/e2e` anula las opciones por defecto de creación para crear aplicaciones de pruebas end-to-end usando el comando `ng e2e`.

* La sección `architect/test` anula las opciones por defecto de creación, para crear pruebas y adicional suministrar los valores por defecto para la ejecución de pruebas para el comando `ng test`.

* La sección `architect/lint` configura los valores por defecto para las opciones del comando `ng lint`, el cual realiza análisis de código sobre los archivos fuentes del proyecto. La herramienta por defecto de linting para Angular es [TSLint](https://palantir.github.io/tslint/).

* La sección `architect/extract-i18n` configura los valores por defecto para las opciones de la herramienta `ng-xi18n` usada por el comando `ng xi18n`, el cual extrae cadenas de texto marcadas como mensaje desde el código fuente y las salidas de los archivos de traducción.

* La sección `architect/server` configura los valores por defecto para la creación de una aplicación universal con renderizado en el lado del servidor, usando el comando `ng run <project>:server`.

* La sección `architect/app-shell` configura los valores por defecto para la creación de un armazón de una aplicación para una aplicación web progresiva (PWA), usando el comando `ng run <project>:app-shell`.

En general, las opciones para las cuales puedes configurar valores por defecto corresponden a las opciones de comandos listados en la [página de referencia del CLI](cli) para cada comando.
Observe que todas las opciones en el archivo de configuración debe usar [camelCase](guide/glossary#case-conventions), en vez de dash-case.

{@a build-target}

## Crear objetivo

La sección `architect/build` configura los valores por defecto para las opciones del comando `ng build`. Este tiene las siguientes propiedades de nivel superior.

| PROPIEDAD | DESCRIPCIÓN |
| :-------------- | :---------------------------- |
| `builder`       | El paquete npm en la herramienta de creación usado para crear este objetivo, el constructor por defecto para una aplicación (`ng build myApp`) es `@angular-devkit/build-angular:browser`, que usa el paquete empaquetador [webpack](https://webpack.js.org/). Observe que un constructor diferente es usado para construir una librería (`ng build myLib`). |
| `options`       | Esta sección contiene las opciones por defecto para crear objetivos, usadas cuando una configuración alternativa no nombrada es especificada. Ver [objetivos de creación por defecto](#default-build-targets) a continuación. |
| `configurations`| Esta sección define y nombra configuraciones alternativas para diferentes destinos deseados. Este contiene una sección para cada configuración nombrada, que establece las opciones por defecto para los ambientes planeados. Ver [configuraciones alternas de creación](#build-configs) a continuación. |


{@a build-configs}

### Configuraciones alternas de creación

Por defecto, una configuración de `production` es definida, y el comando `ng build` tiene la opción `--prod` que crea usando esta configuración. La configuración de `production` establece valores por defecto que optimizan la aplicación de varias maneras, como empaquetar archivos, minimizando el exceso de espacios en blanco, removiendo comentarios y código muerto, y reescribiendo código para usar nombres breves y crípticos ("minificación").

Puedes definir y nombrar configuraciones alternas adicionales (como `stage`, por ejemplo) apropiadas para tu proceso de desarrollo. algunos ejemplos de diferentes configuraciones de creación son `stable`, `archive` y `next` usados por el mismo AIO, y las configuraciones individuales locales y especificas requeridas para la creación de versiones localizadas de una versión de una aplicación. Para más detalles, ver [Internacionalización (i18n)](guide/i18n#merge-aot).

Puedes seleccionar una configuración alterna pasando su nombre en la línea de comandos con la bandera `--configuration`.

Puedes además pasar más que un nombre de configuración como una lista separada por comas. Por ejemplo, para aplicar ambas configuraciones creadas por `stage` y `fr`, use el comando `ng build --configuration stage,fr`. En este caso, el comando analiza las configuraciones nombradas de izquierda a derecha. Si múltiples configuraciones cambian la misma configuración, el último valor establecido es el valor final.

Si la bandera `--prod` es usada en la línea de comandos, esta sé aplica primero, y sus configuraciones puedes ser anuladas por cualquier configuración especificada por medio de la bandera `--configuration`.

{@a build-props}

### Opciones adicionales de creación y pruebas

Las opciones configurables por defecto o por un objetivo de creación generalmente corresponden a las opciones disponibles para los comandos [`ng build`](cli/build), [`ng serve`](cli/serve), y [`ng test`](cli/test). Para más detalles de aquellas opciones y sus posibles valores, ver la [Referencia del CLI](cli). 

Algunas opciones adicionales puedes solo ser establecidas a través del archivo de configuración, o ya sea directamente editando o con el comando [`ng config`](cli/config).

| PROPIEDADES OPCIONALES | DESCRIPCIÓN |
| :------------------------- | :---------------------------- |
| `assets`                   | Un objeto contiene rutas a activos estáticos para agregar al contexto global del proyecto. Las rutas por defecto apuntan al archivo icono del proyecto y su carpeta `assets`. Ver más en [Configuración de activos](#asset-config) a continuación |
| `styles`                   | Un array de archivos de estilo para agregar al contexto global del proyecto. El CLI de Angular soporta importaciones de CSS y todos los principales prepocesadores de CSS: [sass/scss](http://sass-lang.com/), [less](http://lesscss.org/), y [stylus](http://stylus-lang.com/). Ver más en [Configuración de estilos y scripts](#style-script-config) a continuación. |
| `stylePreprocessorOptions` | Un objeto contiene pares de opciones y valores para pasar a los preprocesadores de estilo. Ver más en [Configuración de estilos y scripts](#style-script-config) a continuación. |
| `scripts`                  | Un objeto que contiene archivos de texto JavaScript para agregar al contexto global del proyecto. Los scripts son cargados exactamente como si los hubieras agregado en una etiqueta `<script>` dentro de `index.html`. Ver más en [Configuración de estilos y scripts](#style-script-config) a continuación. |
| `budgets`                  | Tipo de tamaño presupuestado por defecto y umbrales para toda tu aplicación o partes de esta. Puedes configurar el constructor para notificar una advertencia o un error cuando la salida alcance o exceda un umbral de tamaño. Ver [configurar tamaño presupuestado](guide/build#configure-size-budgets). (No disponible en la sección de `test`) | 
| `fileReplacements`         | Un objeto que contiene archivos y sus reemplazos en tiempo de compilación. Ver más en [Configurar reemplazos de archivos específicos de destino](guide/build#configure-target-specific-file-replacements).| 

{@a complex-config}

## Valores complejos de configuración

Las opciones `assets`, `styles`, y `scripts` pueden tener ya sea un valor de cadena de ruta simple, o valores de objeto con campos específicos.
Las opciones `sourceMap` y `optimization` pueden ser establecidas con un valor Booleano simple con un comando bandera, pero también puede ser dado un valor complejo usando el archivo de configuración.
Las siguientes secciones proveen más detalles de como estos valores complejos son usados en cada caso.

{@a asset-config}

### Configuración de activos

Cada configuración de objetivo `build` puede incluir un array `assets` que lista los archivos o carpetas que quieras copiar como es cuando construyes tu proyecto.
Por defecto, la carpeta `src/assets/` y `src/favicon.ico` son copiadas.

<code-example language="json">

"assets": [
  "src/assets",
  "src/favicon.ico"
]

</code-example>

Para excluir un activo, puedes removerlo de la configuración de activos.

Puedes configurar más los activos para ser copiados especificando los activos como objetos, en vez de rutas simples relativas a la raíz del espacio de trabajo.
Un objeto de especificación de activos puede tener los siguientes campos. 

* `glob`:  Un [node-glob](https://github.com/isaacs/node-glob/blob/master/README.md) usa `input` como directorio base.
* `input`: Una ruta relativa a la raíz del espacio de trabajo.
* `output`: Una ruta relativa a `outDir` (el valor por defecto es `dist/`*project-name*). Debido a las implicaciones de seguridad, el CLI nunca escribe archivos fuera de la ruta de salida del proyecto.
* `ignore`: Una lista de globs a excluir.

Por ejemplo, Las rutas por defecto de activos pueden ser representadas en más detalle usando los siguientes objetos.

<code-example language="json">

"assets": [
  { "glob": "**/*", "input": "src/assets/", "output": "/assets/" },
  { "glob": "favicon.ico", "input": "src/", "output": "/" }
]

</code-example>

Puedes usar esta configuración extendida para copiar activos desde afuera de tu proyecto.
Por ejemplo, las siguientes copias de configuración de activos desde un paquete de node.

<code-example language="json">

"assets": [
 { "glob": "**/*", "input": "./node_modules/some-package/images", "output": "/some-package/" },
]

</code-example>

El contenido de  `node_modules/some-package/images/` estará disponible en `dist/some-package/`.

Los siguientes ejemplos usa el campo `ignore` para excluir ciertos archivos en la carpeta de activos de ser copiados dentro del build:

<code-example language="json">

"assets": [
 { "glob": "**/*", "input": "src/assets/", "ignore": ["**/*.svg"], "output": "/assets/" },
]

</code-example>

{@a style-script-config}

### Configuración de estilos y scripts

Una array de entrada para las opciones de `styles` y `scripts` pueden ser una cadena simple de ruta, o un objeto que apunta a un archivo de punto de entrada extra.
El constructor asociado cargará sus archivos y sus dependencias como un paquete separado durante la creación.
Con un objeto de configuración, tienes la opción de nombrar el paquete para un punto de entrada, usando un campo `bundleName`.

El paquete es inyectado de forma por defecto, pero puedes establecer `inject` como false para excluir el paquete en la inyección.
Por ejemplo, los siguientes valores de objetos crean y nombran un paquete que contiene estilos y scripts, y excluyen este en la inyección:  

<code-example language="json">

   "styles": [
     { "input": "src/external-module/styles.scss", "inject": false, "bundleName": "external-module" }
   ],
   "scripts": [
     { "input": "src/external-module/main.js", "inject": false, "bundleName": "external-module" }
   ]

</code-example>

Puedes mezclar referencias de archivos simples y complejos para estilos y scripts.

<code-example language="json">

"styles": [
  "src/styles.css",
  "src/more-styles.css",
  { "input": "src/lazy-style.scss", "inject": false },
  { "input": "src/pre-rename-style.scss", "bundleName": "renamed-style" },
]

</code-example>

{@a style-preprocessor}

#### Opciones de preprocesadores de estilo

En Sass y Stylus puedes hacer uso de la funcionalidad `includePaths` tanto para estilos de componentes y estilos globales, que te permiten agregar rutas de base extra que serán comprobadas para importaciones. 

Para agregar rutas, use la opción `stylePreprocessorOptions`:

<code-example language="json">

"stylePreprocessorOptions": {
  "includePaths": [
    "src/style-paths"
  ]
}

</code-example>

Archivos en esa carpeta, como `src/style-paths/_variables.scss`, pueden ser importados desde cualquier lugar en tu proyecto sin necesitar de una ruta relativa.

```ts
// src/app/app.component.scss
// A relative path works
@import '../style-paths/variables';
// But now this works as well
@import 'variables';
```

Observe que además necesitara agregar cualquier estilo o script al constructor `test` si les necesitas para pruebas unitarias. 
Ver también [Usar librerías globales en tiempo de ejecución dentro de tu aplicación](guide/using-libraries#using-runtime-global-libraries-inside-your-app).


{@a optimize-and-srcmap}

### Configuración y optimización del mapa de origen

Las opciones del comando `optimization` y `sourceMap` son banderas simples Booleanas.
Puedes suministrar un objeto como un valor de configuración para ya sea proveer de estas instrucciones detalladas.

* La bandera `--optimization="true"` aplica para los scripts y los estilos. Puedes suministrar un valor como los siguientes para aplicar optimización a uno u otro:

<code-example language="json">

   "optimization": { "scripts": true, "styles": false }

</code-example>

* la bandera `--sourceMap="true"` produce mapas de origen para los scripts y los estilos.
Puedes configurar la opción para aplicar a uno o a otro.
Puedes también escoger producir mapas de origen ocultos, o resolver mapas de origen de paquetes de vendedores.
Por ejemplo:

<code-example language="json">

   "sourceMap": { "scripts": true, "styles": false, "hidden": true, "vendor": true }

</code-example>

<div class="alert is-helpful">

   Cuando usas mapas de origen ocultos, estos mapas de origen no estarán referenciados en el paquete.
   Estos son útiles si solo quieres mapas de origen para mapear los rastros de la pila de errores en herramientas de informe de errores, 
   pero no quieres exponer tus mapas de origen en las herramientas de desarrollo del navegador.

   Para [Universal](guide/glossary#universal), puedes reducir el código renderizado en la página HTML estableciendo 
   optimización de estilos como `true` y los mapas de origen como `false`. 
   
</div>