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
