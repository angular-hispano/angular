# Espacios de Trabajo y estructura de archivos del proyecto

Desarrollas aplicaciones en el contexto de un [Espacio de Trabajo](guide/glossary#workspace) de Angular. Un espacio de trabajo contiene los archivos de uno o más [proyectos](guide/glossary#project). Un proyecto es un conjunto de archivos que conforma una aplicación unica o una biblioteca compartible.

El comando `ng new` del CLI de Angular crea un espacio de trabajo.

<code-example language="bash">
ng new &lt;my-project&gt;
</code-example>

Cuando ejecutas este comando, el CLI instala los paquetes npm que Angular necesita junto con otras dependencias en un nuevo espacio de trabajo. Con una aplicación llamada *my-project*.
La carpeta raiz del espacio de trabajo contiene varios achivos de apoyo y configuración junto con un archivo README con texto generado que puedes personalizar.

Por defecto, `ng new` crea un estructura inicial de una aplicación en la raiz del espacio de trabajo, junto con sus pruebas end-to-end.
La estructura es para un aplicación sencilla de Bienvenida que esta lista para ser ejecutada y modificada.
La aplicación a nivel raiz tiene el mismo nombre que el espacio de trabajo, y los archivos fuentes residen en la subcarpeta `src/` del espacio de trabajo.

Este comportamiento por defecto es adecuado para un estilo tipico de desarrollo "multi-repo" donde cada aplicación reside en su propio espacio de trabajo.
Usuarios principiantes e intermedios se anima a usar `ng new` para crear un espacio de trabajo separado para cada aplicación.

Angular también soporta espacios de trabajo con [multiples proyectos](#multiple-projects).
Este tipo de entorno de desarrollo es apropiado para usuarios avanzados que están desarrollando [bibliotecas compartibles](guide/glossary#library),
y para empresas que usan un estilo de desarrollo "monorepo", con un unico repositorio y una configuración global para todos los proyectos Angular.

Para configurar un espacio de trabajo monorepo, debes omitir la creación de la aplicación raiz.
Mira [Configuración para espacios de trabajo con multiples proyectos](#multiple-projects).

## Archivos de configuración para espacios de trabajo

Todos los proyectos dentro de un espacio de trabajo comparten un [Contexto de configuración del CLI](guide/workspace-config).
El nivel superior del espacio de trabajo contiene archivos de configuración de todo el espacio de trabajo, archivos de configuración para el nivel raiz de la aplicación, las subcarpetas fuente para el nivel raíz de la aplicación y los archivos de pruebas.

| ARCHIVOS DE CONFIGURACION DEL ESPACIO DE TRABAJO    | PROPÓSITO |
| :--------------------- | :------------------------------------------|
| `.editorconfig`        | Configuraciones para editores de codigo. Mira [EditorConfig](https://editorconfig.org/). |
| `.gitignore`           | Especificar intencionalmente archivos sin seguimiento que [Git](https://git-scm.com/) debería ignorar. |
| `README.md`            | Introducir documentacion para la aplicación raiz. |
| `angular.json`         | Configuración por defecto del CLI para todos los proyectos en el espacio de trabajo, incluyendo opciones de configuración para crear, servir, y probar las herramientas que el CLI usa, como [TSLint](https://palantir.github.io/tslint/), [Karma](https://karma-runner.github.io/), y [Protractor](http://www.protractortest.org/). Para detalles, mira [Configuracion de espacios de trabajo Angular](guide/workspace-config). |
| `package.json`          | Configura [npm package dependencies](guide/npm-packages) que estan disponibles para todos los proyectos en el espacio de trabajo. Mira [npm documentation](https://docs.npmjs.com/files/package.json) para tener el formato y el contenido especifico en este archivo. |
| `package-lock.json`     | Proporciona infromación de la versión de todos los paquetes instalados en `node_modules` por el cliente de npm. Mira [npm documentation](https://docs.npmjs.com/files/package-lock.json) para detalles. Si tu usas el cliente de yarn, sera este archivo [yarn.lock](https://yarnpkg.com/lang/en/docs/yarn-lock/) en su lugar. |
| `src/`                  | Archivos fuente para el proyecto de aplicación a nivel raíz. |
| `node_modules/`         | Proporciona [npm packages](guide/npm-packages) a todo el espacio de trabajo. En todo el espacio de trabajo las depenciencias `node_modules` son visibles para todo el proyecto. |
| `tsconfig.json`         | El archivo `tsconfig.json` es un archivo de configuración de typescript de ["Estilo de solución"](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-9.html#support-for-solution-style-tsconfigjson-files). Los editores de codigo y los servicios del lenguaje de Typescript usan este archivo para mejorar la experiencia de desarrollo. Los copiladores no usan este archivo. |
| `tsconfig.base.json`    | La configuración base de [TypeScript](https://www.typescriptlang.org/) para proyectos en el espacio de trabajo. Todos los demas archivos de configuración heredan desde este archivo base. Para mas información, mira [Herencia de configuración con extends](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#configuration-inheritance-with-extends) sección de la documentación de Typescript.|
| `tslint.json`           | Configuración de [TSLint](https://palantir.github.io/tslint/) por defecto para los proyectos en el espacio de trabajo. |


## Archivos de aplicaciones

Por defecto, el comando `ng new my-app` del CLI crea una carpeta de espacio de trabajo llamada "my-app" y genera una nueva aplicación con una estructura inicial en la carpeta `src/` en el nivel superior del espacio de trabajo.
Una aplicación recien generada contiene archivos fuentes para un modulo raíz, con un componente raiz y una plantilla.

Cuando la estructura de archivos del espacio de trabajo esta en su lugar, tu puedes usar en la linea de comandos la instrucción `ng generate` para agregar funcionalidad y datos a la aplicación.
Esta aplicación inicial a nivel raiz es la *aplicación default* para los comandos del CLI(a menos que cambies el valor predeterminado después de crearla) [aplicaciones adicionales](#multiple-projects)).

<div class="alert is-helpful">

   Además de usar los comando del CLI, tu también puedes manipular archivos directamente en la carpeta de origen de las aplicaciones y los archivos de configuración.

</div>

Para espacio de trabajo de una sola aplicación, la subcarpeta `src/` del espacios de trabajo contiene archivos fuente (logica de la aplicación, información y recursos) para la raiz de la aplicación
Para espacios de trabajo con multiples proyectos, los proyectos adicionales en la carpeta `projects/` contiene una subcarpeta `project-name/src/` con la misma estructura.

### Archivos fuente de aplicaciones

Archivos en el nivel superior de `src/` soportan pruebas y ejecutan tu aplicación. Las subcarpetas contienen la aplicación fuente y configuración especifica de la aplicación.

| ARCHIVOS DE SOPORTE DE APLICACIONES    | PROPÓSITO |
| :--------------------- | :------------------------------------------|
| `app/`                 | Contiene los archivos de los componentes en los cuales la lógica y datos son definidos. Ver Detalles [abajo](#app-src). |
| `assets/`              | Contiene imagenes y otros archivos para ser copiados cuando creas tu aplicación. |
| `environments/`        | Contiene diferentes configuraciones de creeación para entornos especificos. Por defecto hay un entorno de desarrollo estandar sin nombre y un entorno de producción ("prod"). Tu puedes definir configuraciones adicionales para entornos especificos. |
| `favicon.ico`          | Es un icono usado para esta aplicación en la barra de marcadores. |
| `index.html`           | La pagina principal de HTML que es servida cuando visitan tu sitio. El CLI agrega automaticamente todo los archivos Javascript y CSS cuando se contruyes tu aplicación, por lo que normalmente no necesitas agregar aquí ningún `<script>` o etiqueta` <link>` manualmente. |
| `main.ts`              | El punto de entrada principal a tu aplicación. Copila aplicación con [JIT compiler](guide/glossary#jit) y carga el modulo raiz de la aplicación (AppModule) para ejecuar en el navegador. también puedes usar el [copilador AOT](guide/aot-compiler) sin cambiar nada de codigo y agregando la bandera `--aot` al CLI y los comandos `build` y `serve`. |
| `polyfills.ts`         | Proporciona scripts polyfill para soporte al navegador. |
| `styles.sass`          | Muestra una lista de archivos CSS que proporcionan estilos al proyecto. La extención refleja el estilo del preprocesador que tu has configurado para el proyecto. |
| `test.ts`              | El punto de entrada principal para las pruebas unitarias, con algunas configuraciones especificas de Angular. Tu tipicamente no necesitas editar este archivo. |

<div class="alert is-helpful">

Si tu creas una aplicación Angular usando el mode estricto, También tendrás un archivo adicional `package.json` en el directorio `src/app`. Para mas información, mira [modo estricto](/guide/strict-mode).

</div>

{@a app-src}

Dentro del directorio `src/`, la carpeta `app/` contiene la lógica y los datos de tu proyecto.
Los componentes Angular, las plantillas, y estilos van aquí.

| ARCHIVOS `src/app/` | PROPÓSITO |
| :-------------------------- | :------------------------------------------|
| `app/app.component.ts`      | Defina la lógica para el componente de raíz de la aplicación, llamado `AppComponent`. Las vistas asociadas con este componente raiz se convierte en la de [vistas jererquicas](guide/glossary#view-tree) a medida que agregas componentes y servicos a tu aplicaciones. |
| `app/app.component.html`    | Define la plantilla HTML asiciada a la raíz `AppComponent`. |
| `app/app.component.css`     | Define la hoja de estilos CSS base para la raíz `AppComponent`. |
| `app/app.component.spec.ts` | Define una prueba unitaria para la raíz `AppComponent`. |
| `app/app.module.ts`         | Define el modulo raíz, llamado `AppModule`, que le indica a Angular como ensamblar la aplicación. Inicialmente solo declara el `AppComponent`. A medida que tu añades componentes a tu aplicación, Ellos deben ser declarados aquí. |
| `app/package.json`              | Este archivo es generado solo en aplicaciones creadas con el modo `--strict`. Este archivo no es usado por el gestor de paquetes. Es usado para indicar a las herramientas y empaquetadores si el codigo debajo de este directorio esta libre de [efectos secundarios](guide/strict-mode#side-effect) no locales. |

### Archivos de configuración de la aplicación

Los archivos que especifican la configuración de la aplicación para la aplicación raíz residen en el nivel raíz del espacio de trabajo.
Para un espacio de trabajo con multiples proyectos, Los archivos de que especifican la configuración estan en la raiz del proyecto, dentro de `projects/project-name/`.

Los archivos que especifican la configuración de [TypeScript](https://www.typescriptlang.org/) en el proyecto heredan del `tsconfig.base.json` de todo el espacio de trabajo, y los archivos que especifican la configuración [TSLint](https://palantir.github.io/tslint/) heredan de `tslint.json` de todo el espacio de trabajo.

| ARCHIVOS DE CONFIGURACIÓN ESPECÍFICOS DE LA APLICACIÓN    | PROPÓSITO |
| :--------------------- | :------------------------------------------|
| `.browserslistrc`       | Configura el uso compartido de los navegadores destino y las versiones de Node.js entre varias herramientas de front-end. Mira [Browserslist on GitHub](https://github.com/browserslist/browserslist) para más información.  |
| `karma.conf.js`      | Configuración especifica de la aplicación [Karma](https://karma-runner.github.io/2.0/config/configuration-file.html). |
| `tsconfig.app.json`    | Configuración especifica de la aplicación [TypeScript](https://www.typescriptlang.org/), incluye una plantilla de las opciones del copilador TypeScript y Angular. Mira [TypeScript Configuration](guide/typescript-configuration) y [Angular Compiler Options](guide/angular-compiler-options). |
| `tsconfig.spec.json`   | [TypeScript](https://www.typescriptlang.org/) configuración para las pruebas de la aplicación. Mira [TypeScript Configuration](guide/typescript-configuration). |
| `tslint.json`          | Configuración especifica  de la aplicación [TSLint](https://palantir.github.io/tslint/). |

### Archivos de pruebas End-to-end

Una carpeta `e2e/` en el nivel superior contiene los archivos fuente para configurar las pruebas end-to-end que corresponde a la aplicación a nivel raiz, junto con sus archivos especificos de configuración para las pruebas.

Para un espacio de trabajo con multiples proyectos, los test end-to-end especificos para la aplicación estan en la raíz del proyecto, debajo de `projects/project-name/e2e/`.

<code-example language="none">
  e2e/
     src/                 (end-to-end tests for my-app)
        app.e2e-spec.ts
        app.po.ts
      protractor.conf.js  (test-tool config)
      tsconfig.json       (TypeScript config inherits from workspace)
</code-example>

{@a multiple-projects}

## Múltiples proyectos

Un espacio de trabajo con multiples proyectos A multi-project workspace es adecuado para una empresa  que usa un solo repositorio y una configuración global para todos los proyectos Angular (el modelo "monorepo"). Un espacio de trabajo con multiples proyectos también es compatible con el desarrollo librerías.

### Configuración para un espacio de trabajo de multiples proyectos

Si tu intentas tener multiples proyectos en un espacio de trabajo, tu puedes saltarte la generación de la aplicación incial cuando creas el espacio de trabajo, y das al espacio de trabajo un nombre unico.
El comando siguiente crea un espacio de trabajo con todos los archivos de configuración para todos los espacios de trabajo, pero sin la aplicación a nivel raíz.

<code-example language="bash">
ng new my-workspace --createApplication="false"
</code-example>

Puedes generar aplicaciones y librerías con nombres que sean unicos dentro del espacio de trabajo.

<code-example language="bash">
cd my-workspace
ng generate application my-first-app
</code-example>

### Estructura de archivos para multiples proyectos

La primera aplicación generada explícitamente entra en la carpeta `projects/` junto con todos los demás proyectos en el espacio de trabajo.
Las librerías generadas también son agregadas dentro de `projects/`.
Cuando creas proyectos de esta forma, la estructura del espacio de trabajo es enteramente consistente con la estructura del [archivo de configuración del espacio de trabajo](guide/workspace-config), `angular.json`.

<code-example language="none">
my-workspace/
  ...             (workspace-wide config files)
  projects/       (generated applications and libraries)
    my-first-app/ --(an explicitly generated application)
      ...         --(application-specific config)
      e2e/        ----(corresponding e2e tests)
         src/     ----(e2e tests source)
         ...      ----(e2e-specific config)
      src/        --(source and support files for application)
    my-lib/       --(a generated library)
      ...         --(library-specific config)
      src/        --source and support files for library)
</code-example>

## Archivos de librería

Cuando creas una librería usando CLI (con comandos como `ng generate library my-lib`), los archivos generados van en la carpeta `projects/` del espacio de trabajo. Para mas información creación de librerías propias, mira  [Creación de librerías](guide/creating-libraries).

Las librerías (a diferencias de las aplicaciones y sus proyectos e2e asociados) tienen su propio archivo de configuración `package.json`.

En la carpeta `projects/`, la carpeta `my-lib` contiene el codigo de la librería.

| ARCHIVOS FUENTE DE LA LIBRERÍA | PROPÓSITO                                                                      |
| :------------------- | :----------------------------------------------------------------------------|
| `src/lib`           |  Contiene la lógica y la información de la librería. Igual a un aplicación, una librería puede contener componentes servicios, modulos, directivas, y tuberías.                                                            |
| `src/test.ts`       | El punto de entrada principal para tus pruebas unitarios, con algunas configuraciones especificas de la librería. Tu no necesitas editar este archivo.                                                                                            |
| `src/public-api.ts`  | Especifica todos los archivos que son exportados de tu librería.                                                                                                                                                                     |
| `karma.conf.js`      | Configuración especifica de la libería [Karma](https://karma-runner.github.io/2.0/config/configuration-file.html) configuration.                                                                                                                   |
| `ng-package.json`    | Archivo de configuración usadao para [ng-packagr](https://github.com/ng-packagr/ng-packagr) para crear tu librería.                                                                                                                 |
| `package.json`       | Configuración [npm package dependencies](guide/npm-packages) requerida para esta librería.                                                                                                                                |
| `tsconfig.lib.json`  | Configuración especifica de la liberías [TypeScript](https://www.typescriptlang.org/), incluye una plantilla de las opciones del copilador TypeScript y Angular. Mira [TypeScript Configuration](guide/typescript-configuration).            |
| `tsconfig.spec.json` | [TypeScript](https://www.typescriptlang.org/) configuración para las pruebas de la aplicación. Mira [TypeScript Configuration](guide/typescript-configuration).                                                                     |
| `tslint.json`        | Configuración especifica  de la aplicación [TSLint](https://palantir.github.io/tslint/). |
