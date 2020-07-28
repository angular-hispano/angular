# Glosario

Angular tiene su propio vocabulario. La mayoría de los términos en Angular son palabras comunes en inglés o términos informáticos que tienen un significado específico dentro del sistema Angular.

Este glosario enumera los términos más destacados y algunos menos familiares con definiciones inusuales o inesperadas.

[A](#A) [B](#B) [C](#C) [D](#D) [E](#E) [F](#F) [G](#G) [H](#H) [I](#I)
[J](#J) [K](#K) [L](#L) [M](#M) [N](#N) [O](#O) [P](#P) [Q](#Q) [R](#R)
[S](#S) [T](#T) [U](#U) [V](#V) [W](#W) [X](#X) [Y](#Y) [Z](#Z)


{@a A}

{@a aot}

## compilación ahead-of-time (AOT)

La compilación ahead-of-time (AOT) de Angular convierte el código Angular HTML y TypeScript en código JavaScript durante la fase de compilación de una manera eficiente, antes de que el navegador descargue y ejecuta ese código.

Este es el mejor modo de compilación para entornos de producción, con un menor tiempo de carga y un mayor rendimiento en comparación con [compilación just-in-time (JIT)](#jit).

Al compilar su aplicación utilizando la herramienta de línea de comandos `ngc`, puede iniciar directamente a una module factory, por lo que no necesita incluir el compilador Angular en su paquete de JavaScript.


{@a angular-element}

## elemento angular

Un [componente](#component) angular empaquetado como un [elemento personalizado](#custom-element).

Obtiene más información en [Vista general de Elementos Angular](guide/elements).

{@a annotation}

## anotaciones

Una estructura que proporciona metadatos para una clase. Ver [decorador](#decorator).

{@a app-shell}

## shell de aplicación

El shell de aplicación es una forma de representar una parte de su aplicación a través de una ruta en el momento de la compilación.
Esto brinda a los usuarios una primera visualización significativa de su aplicación que aparece rápidamente porque el navegador puede renderizar HTML y CSS estáticos sin la necesidad de inicializar JavaScript.

Obtiene más información en [Modelo Shell de Aplicación](https://developers.google.com/web/fundamentals/architecture/app-shell).

Puedes usar angular CLI para [generar](cli/generate#appshell) un shell de aplicación.
Esto puede mejorar la experiencia del usuario al iniciar rápidamente una página estática renderizada (un esqueleto común a todas las páginas) mientras el navegador descarga la versión completa del cliente y cambia automáticamente después de que se carga el código.

Ver también [Service Worker y PWA](guide/service-worker-intro).

{@a architect}

## architect

Es una herramienta que utiliza la CLI para realizar tareas complejas, como la compilación y la ejecución de pruebas, de acuerdo con una configuración proporcionada.

El architect es un shell que ejecuta un [constructor](#builder) (definido en un [paquete npm](#npm-package)) con una [configuración de destino] dada (#target).

En el [archivo de configuración del espacio de trabajo](guide/workspace-config#project-tool-configuration-options), una sección de "architect" proporciona opciones de configuración para los architects constructores.

Por ejemplo, un constructor incorporado para el linting se define en el paquete `@angular-devkit/build_angular: tslint`, que utiliza la herramienta [TSLint](https://palantir.github.io/tslint/) para realizar el linting, con una configuración especificada en un archivo `tslint.json`.

Utiliza el [comando CLI `ng run`](cli/run) para invocar a un constructor especificando una [configuración de destino](#target) asociada con ese constructor.
Los integradores pueden agregar constructores para permitir que las herramientas y los flujos de trabajo se ejecuten a través de la angular CLI. Por ejemplo, un constructor personalizado puede reemplazar las herramientas de terceros utilizadas por las implementaciones integradas para los comandos de la CLI, como `ng build` o `ng test`.

{@a attribute-directive}

{@a attribute-directives}


## directivas de atributos

Una categoría de [directiva](#directive) que puede escuchar y modificar el comportamiento de otros elementos HTML, atributos, propiedades y componentes. Generalmente están representados como atributos HTML, de ahí el nombre.

Obtenga más información en [Directivas de atributos](guide/attribute-directives).

{@a B}

{@a binding}

## enlaces de datos (binding)

En general, es la práctica de establecer una variable o propiedad en un valor de datos Dentro de Angular, generalmente se refiere a [enlace de datos](#data-binding), que coordina las propiedades del objeto DOM con las propiedades del objeto de datos.

A veces se refiere a una [dependencia de la inyección](#dependency-injection) de enlace
entre un [token](#token) y una dependencia de [proveedor](#provider).

{@a bootstrap}

## bootstrap

Una forma de inicializar e iniciar una aplicación o sistema.

En Angular, el NgModule raíz de una aplicación (`AppModule`) tiene una propiedad `bootstrap` que identifica los [componentes](#component) de nivel superior de la aplicación. 
Durante el proceso de arranque, Angular crea e inserta estos componentes en la página web del host `index.html`. 
Puede arrancar varias aplicaciones en el mismo `index.html`. Cada aplicación contiene sus propios componentes.

Obtiene más información en [Bootstrapping](guide/bootstrapping).

{@a builder}

## constructor

Una función que utiliza la API de [Architect](#architect) para realizar un proceso complejo como "compilar" o "probar". 
El código del constructor se define en un [paquete npm](#npm-package).

Por ejemplo, [BrowserBuilder](https://github.com/angular/angular-cli/tree/master/packages/angular_devkit/build_angular/src/browser) ejecuta una compilación de [webpack](https://webpack.js.org/) para un destino de navegador y [KarmaBuilder](https://github.com/angular/angular-cli/tree/master/packages/angular_devkit/build_angular/src/karma) inicia el servidor Karma y ejecuta una compilación de webpack para pruebas unitarias.

El [comando CLI `ng run`](cli/run) invoca un constructor con una [configuración de destino específica](#target). El [archivo de configuración del espacio de trabajo](guide/workspace-config), `angular.json`, contiene configuraciones predeterminadas para los constructores integrados.

{@a C}

{@a case-conventions}
{@a dash-case}
{@a camelcase}
{@a kebab-case}

## tipos de casos

Angular usa convenciones de mayúsculas para distinguir los nombres de varios tipos, como se describe en la [sección de pautas de nomenclatura](guide/styleguide#02-01) de la Guía de estilo. Aquí hay un resumen de los tipos de casos:

* camelCase: símbolos, propiedades, métodos, nombres de pipelines, selectores de directivas sin componentes, constantes.
El caso de camelcase estándar o inferior usa minúsculas en la primera letra del artículo. Por ejemplo, "selectedHero".

* UpperCamelCase (o PascalCase): nombres de clase, incluidas las clases que definen componentes, interfaces, NgModules, directivas y tuberías,
La mayúscula del camelcase usa mayúscula en la primera letra del artículo. Por ejemplo, "HeroListComponent".

* dash-case (o "kebab-case"): Parte descriptiva de los nombres de archivos, selectores de componentes. Por ejemplo, "app-hero-list".

* underscore_case (o "snake_case"): No se usa típicamente en Angular. El caso de serpiente utiliza palabras relacionadas con guiones bajos.
Por ejemplo, "convert_link_mode".

* UPPER_UNDERSCORE_CASE (o UPPER_SNAKE_CASE, o SCREAMING_SNAKE_CASE): Tradicional para constantes (aceptable, pero prefiere camelCase).
Las mayúsculas y minúsculas usan palabras en mayúsculas conectadas con guiones bajos. Por ejemplo, "FIX_ME".

{@a change-detection}

## detección de cambios

El mecanismo por el cual el framework de angular sincroniza el estado de la interfaz de usuario de una aplicación con el estado de los datos.

El detector de cambios verifica el estado actual del modelo de datos cada vez que se ejecuta y lo mantiene como el estado anterior para compararlo en la siguiente iteración.

A medida que la lógica de la aplicación actualiza los datos de los componentes, los valores que están vinculados a las propiedades DOM en la vista pueden cambiar.
El detector de cambios es responsable de actualizar la vista para reflejar el modelo de datos actual.
Del mismo modo, el usuario puede interactuar con la interfaz de usuario, lo que provoca eventos que cambian el estado del modelo de datos.
Estos eventos pueden desencadenar la detección de cambios.

Usando la estrategia de detección de cambio predeterminada ("CheckAlways"), el detector de cambio pasa por la [jerarquía de vista](#view-tree) en cada turno de VM para verificar cada [propiedad vinculada a datos](#data-binding) en la plantilla. en el modelo. En la primera fase, compara el estado actual de los datos dependientes con el estado anterior y recopila los cambios.
En la segunda fase, actualiza la página DOM para reflejar los nuevos valores de datos.

Si configuras la estrategia de detección de cambios `OnPush` ("CheckOnce"), el detector de cambios se ejecuta solo cuando [invocado explícitamente](api/core/ChangeDetectorRef), o cuando se activa mediante un cambio de referencia en mediante un `Input` o un controlador de eventos. Esto generalmente mejora el rendimiento. Para obtener más información, consulta [Optimizar la detección de cambios de Angular](https://web.dev/faster-angular-change-detection/).

{@a class-decorator}

## decorador de clase

Un [decorador](#decorator) que aparece inmediatamente antes de una definición de clase, que declara que la clase es del tipo dado y proporciona metadatos adecuados para el tipo.

Los siguientes decoradores pueden declarar tipos de clase angular:
* `@Component()`
* `@Directive()`
* `@Pipe()`
* `@Injectable()`
* `@NgModule()`


{@a class-field-decorator}

## decorador de campo de clase

Un [decorador](#decorator) declarado inmediatamente antes de un campo en una definición de clase que declara el tipo de ese campo. Algunos ejemplos son `@Input` y `@Output`.

{@a collection}

## colección

En Angular, un conjunto de [esquemas](#schematic) relacionados recogidos en un [paquete npm](#npm-package).

{@a cli}

## interfaz de línea de comandos (CLI)

[Angular CLI](cli) es una herramienta de línea de comandos para administrar el ciclo de desarrollo angular. Úsalo para crear la estructura inicial del sistema de archivos para un [espacio de trabajo](#workspace) o [proyecto](#project), y para ejecutar [esquemas](#schematic) que agregue y modifique código para versiones genéricas iniciales de varios elementos. La CLI admite todas las etapas del ciclo de desarrollo, incluidas la construcción, las pruebas, la agrupación y la implementación.

* Para comenzar a usar la CLI para un nuevo proyecto, consulte [Configuración del entorno local](guide/setup-local "Configuración para el desarrollo local").
* Para obtener más información sobre las capacidades completas de la CLI, consulta la [Referencia del comando CLI](cli).

Ver también [Esquemas CLI](#schematics-cli).

{@a component}

## componente

Una clase con el `@Component()` [decorador](#decorator) tque lo asocia con un compañero [plantilla](#template). Juntos, la clase de componente y la plantilla definen una [vista](#view).
Un componente es un tipo especial de [directiva](#directive).
El decorador `@Component()` extiende el decorador `@Directive()` con características orientadas a plantillas.

Una clase de componente angular es responsable de exponer los datos y manejar la mayor parte de la visualización de la vista y la lógica de interacción del usuario a través de [enlace de datos](#data-binding).

Lee más sobre clases de componentes, plantillas y vistas en [Introducción a los conceptos de Angular](guide/architecture).


## configuración

Ver [configuración del espacio de trabajo](#cli-config)

{@a content-projection}

## proyección de contenido

Una forma de insertar contenido DOM desde fuera de un componente en la vista del componente en un lugar designado.

Para obtener más información, consulta [Respuesta a cambios en el contenido](guide/lifecycle-hooks#content-projection).

{@a custom-element}

## elemento personalizado

Una función de plataforma web, actualmente compatible con la mayoría de los navegadores y disponible en otros navegadores a través de polyfills (consulta [Soporte del navegador](guide/browser-support)).

La característica de elemento personalizado extiende HTML al permitirle definir una etiqueta cuyo contenido es creado y controlado por código JavaScript. Un elemento personalizado (también llamado *componente web*) es reconocido por un navegador cuando se agrega a [CustomElementRegistry](https://developer.mozilla.org/en-US/docs/Web/API/CustomElementRegistry).

Puedes usar la API para transformar un componente Angular para que pueda registrarse con el navegador y usarse en cualquier HTML que agregue directamente al DOM dentro de una aplicación Angular. La etiqueta de elemento personalizado inserta la vista del componente, con la funcionalidad de detección de cambios y enlace de datos, en contenido que de otro modo se mostraría sin procesamiento angular.

Ver [Elemento angular](#angular-element).

Consulta también [carga de componentes dinámicos](#dynamic-components).

{@a D}

{@a data-binding}

## enlace de datos

Un proceso que permite a las aplicaciones mostrar valores de datos a un usuario y responder al usuario acciones (como clics, toques y pulsaciones de teclas).

En el enlace de datos, declara la relación entre un widget HTML y una fuente de datos
y deja que el framework maneje los detalles.

El enlace de datos es una alternativa para insertar manualmente los valores de datos de la aplicación en HTML, adjuntando oyentes de eventos, extrayendo valores modificados de la pantalla y actualizar los valores de los datos de la aplicación.

Lee sobre las siguientes formas de enlace en la [Sintaxis de plantilla](guide/template-syntax) de Angular:

 * [Interpolación](guide/interpolation)
 * [Enlace de propiedad](guide/property-binding)
 * [Enlace de evento](guide/event-binding)
 * [Enlace de atributo](guide/attribute-binding)
 * [Enlace de clase](guide/attribute-binding#class-binding)
 * [Enlace de estilo](guide/attribute-binding#style-binding)
 * [Enlace de datos bidireccional con ngModel](guide/built-in-directives#ngModel)

{@a declarable}

## declarable

Un tipo de clase que puede agregar a la lista de 'declaraciones' de un [NgModule](#ngmodule).
Puedes declarar [componentes](#component), [directivas](#directive), y [pipes](#pipe).

No declares lo siguiente:
* Una clase que ya está declarada en otro NgModule
* Un conjunto de directivas importadas de otro paquete. Por ejemplo, no declarar `FORMS_DIRECTIVES` de `@angular/forms`
* Clases de NgModule
* Clases de servicio
* Clases y objetos no de Angular, como cadenas de texto, números, funciones, modelos de entidad, configuraciones, lógica de negocios y clases auxiliares.


{@a decorator}

{@a decoration}

## decorador | decoración

Una función que modifica una definición de clase o propiedad. Los decoradores (también llamados *anotaciones*) son experimentales (etapa 2)[característica del lenguaje JavaScript](https://github.com/wycats/javascript-decorators).
TypeScript agrega soporte para decoradores.

Angular define decoradores que adjuntan metadatos a clases o propiedades para que sepas qué significan esas clases o propiedades y cómo deberían funcionar.

Consulta [decorador de clase](#class-decorator), [decorador de campo de clase](#class-field-decorator).

{@a di}

{@a dependency-injection}

## inyección de dependencia (DI)

Un patrón de diseño y un mecanismo para crear y entregar algunas partes de una aplicación (dependencias) a otras partes de una aplicación que las requieran.

En Angular, las dependencias suelen ser servicios, pero también pueden ser valores, como cadenas o funciones.

Un [inyector](#injector) fpara una aplicación (creado automáticamente durante el arranque) crea instancias de dependencias cuando sea necesario, utilizando un [proveedor](#provider) configurado del servicio o valor.

Más información en [Inyección de dependencia en Angular](guide/dependency-injection).

{@a di-token}

## token DI

Un token de búsqueda asociado con una dependencia [proveedor](#provider), para usar con el sistema [inyección de dependencia](#di).

{@a differential-loading}

## carga diferencial

Una técnica de compilación que crea dos paquetes para una aplicación. Un paquete más pequeño es para navegadores modernos. Un segundo paquete más grande permite que la aplicación se ejecute correctamente en navegadores más antiguos (como IE11) que no son compatibles con todas las API de navegador modernas.

Para obtener más información, consulta la guía de [Implementación](guide/deployment#differential-loading).


{@a directive}
{@a directives}

## directiva

Una clase que puede modificar la estructura del DOM o modificar atributos en el DOM y el modelo de datos de componentes. Una definición de clase directiva está precedida inmediatamente por un `@Directive ()` [decorador](#decorator) que proporciona metadatos.

Una clase de directiva generalmente está asociada con un elemento o atributo HTML, y ese elemento o atributo a menudo se conoce como la directiva misma. Cuando Angular encuentra una directiva en una [plantilla](#template) HTML, crea la instancia de clase de directiva coincidente y le da a la instancia control sobre esa parte del DOM del navegador.

Hay tres categorías de directivas:
* [Componentes](#component) usa `@Component()` (una extensión de `@Directive()`) para asociar una plantilla con una clase.

* [Directivas de atributo](#attribute-directive) modifica el comportamiento y la apariencia de los elementos de la página.

* [Directivas estructurales](#structural-directive) modifican la estructura del DOM.

Angular proporciona una serie de directivas integradas que comienzan con el prefijo `ng`.
También puede crear nuevas directivas para implementar su propia funcionalidad.
Asocia un *selector* (una etiqueta HTML como `<my-directive>`) con una directiva personalizada, extendiendo así la [sintaxis de plantilla](guide/template-syntax) que puede usar en sus aplicaciones.

{@a dom}

## lenguaje específico de dominio (DSL)

Una biblioteca o API de propósito especial; consulte [Idioma específico del dominio](https://en.wikipedia.org/wiki/Domain-specific_language).
Angular extiende TypeScript con lenguajes específicos de dominio para varios dominios relevantes para aplicaciones Angular, definidas en NgModules como [animaciones](guide/animations), [formularios](guide/forms), y [enrutamiento y navegación](guide/router).

{@a dynamic-components}

## carga dinámica de componentes

Una técnica para agregar un componente al DOM en tiempo de ejecución. Requiere que excluya el componente de la compilación y luego lo conecte al marco de detección de cambios y manejo de eventos de Angular cuando lo agrega al DOM.

Consulta también [elemento personalizado](#custom-element), que proporciona una ruta más fácil con el mismo resultado.


{@a E}

{@a eager-loading}

## carga eager

Los NgModules o componentes que se cargan en el lanzamiento se llaman eager-loaded, para distinguirlos de aquellos que se cargan en tiempo de ejecución (carga diferida).

Ver [carga diferida](#lazy-load).


{@a ecma}

## ECMAScript

La [especificación oficial del lenguaje JavaScript](https://en.wikipedia.org/wiki/ECMAScript).

No todos los navegadores son compatibles con el último estándar ECMAScript, pero puede usar un [transpiler](#transpile) (como [TypeScript](#typescript)) para escribir código utilizando las últimas funciones, que luego se transpilarán al código que se ejecuta en las versiones que son compatibles con los navegadores.

Para obtener más información, consulta [Browser Support](guide/browser-support).


{@a element}

## elemento

Angular define una clase `ElementRef` para envolver elementos de interfaz de usuario nativos específicos del render.
En la mayoría de los casos, esto le permite usar plantillas de angular y enlace de datos para acceder a elementos DOM sin referencia al elemento nativo.

La documentación generalmente se refiere a *elementos* (instancias `ElementRef`), a diferencia de *elementos DOM* (que se puede acceder directamente si es necesario).

Comparado con [elemento personalizado](#custom-element).

{@a entry-point}

## punto de entrada

Un [módulo de JavaScript](#module) que está destinado a ser importado por un usuario de [un
paquete npm](guide/npm-packages). Un módulo de punto de entrada suele reexportarse símbolos de otros módulos internos. Un paquete puede contener múltiples puntos de entrada. Por ejemplo, el paquete `@angular/core` Un módulo de punto de entrada suele reexportarse
símbolos de otros módulos internos. Un paquete puede contener múltiples puntos de entrada. Por ejemplo, el paquete `@angular/core` y
`@angular/core/testing`.

{@a F}

{@a form-control}

## control de formulario

Una instancia de `FormControl`, que es un elemento fundamental para los formularios angular.
Junto con `FormGroup` y `FormArray`, rastrea el valor, la validación y el estado de un elemento de entrada de formulario.

Lee más sobre formularios en [Introducción a los formularios en angular](guide/forms-overview).

{@a form-model}

## form model

modelo de formulario

La "fuente de verdad" para el valor y el estado de validación de un elemento de entrada de formulario en un momento dado. Cuando se usan [formularios reactivos](#reactive-forms), tel modelo de formulario se crea explícitamente en la clase de componente. Al utilizar [formularios controlados por plantilla](#template-driven-forms), el modelo de formulario se crea implícitamente mediante directivas.

Obtiene más información sobre los formularios reactivos y basados en plantillas en [Introducción a los formularios en angular](guide/forms-overview).

{@a form-validation}

## validación de formulario

Una comprobación que se ejecuta cuando cambian los valores del formulario e informa si los valores dados son correctos y completos, de acuerdo con las restricciones definidas. Se aplican formas reactivas [funciones de validación](guide/form-validation#adding-to-reactive-forms). Los formularios basados en plantillas usan [directivas de validación](guide/form-validation#adding-to-template-driven-forms).

Para obtener más información, consulta [Validación de formularios](guide/form-validation).


{@a G}


{@a H}

{@a I}


{@a immutability}

## inmutabilidad

La capacidad de alterar el estado de un valor después de su creación. [Formularios reactivos](#reactive-forms) realizan cambios inmutables en ese cada cambio en el modelo de datos produce un nuevo modelo de datos en lugar de modificar el existente. [Formas controladas por plantilla](#template-driven-forms) realizan cambios mutables con `NgModel` y [enlace de datos bidireccional](#data-binding) para modificar el modelo de datos existente en su lugar.

{@a injectable}

## inyectable

Una clase angular u otra definición que proporciona una dependencia utilizando el mecanismo de [inyección de dependencia](#di). Una clase inyectable [servicio](#service) debe estar marcada por el `@Injectable ()` [decorador](#decorator). Otros elementos, como valores constantes, también pueden ser inyectables.

{@a injector}

## inyector

Un objeto en el sistema angular [inyección de dependencia](#dependency-injection) que puede encontrar una dependencia con nombre en su caché o crear una dependencia utilizando un [proveedor](#provider) configurado.
Los inyectores se crean para NgModules automáticamente como parte del proceso de arranque
y se heredan a través de la jerarquía de componentes.

* Un inyector proporciona una instancia singleton de una dependencia, y puede inyectar esta misma instancia en múltiples componentes.

* Una jerarquía de inyectores a nivel de NgModule y componente puede proporcionar diferentes instancias de dependencia a sus propios componentes y componentes secundarios.

* Puede configurar inyectores con diferentes proveedores que pueden proporcionar diferentes implementaciones de la misma dependencia.

Obtenga más información sobre la jerarquía de inyectores en [Inyectores de dependencia jerárquica](guide/hierarchical-dependency-injection).

{@a input}

## input

When defining a [directive](#directive), the `@Input()` decorator on a directive property
makes that property available as a *target* of a [property binding](guide/property-binding).
Data values flow into an input property from the data source identified
in the [template expression](#template-expression) to the right of the equal sign.

To learn more, see [input and output properties](guide/inputs-outputs).

{@a interpolation}

## interpolation

A form of property [data binding](#data-binding) in which a [template expression](#template-expression) between double-curly braces renders as text.
That text can be concatenated with neighboring text before it is assigned to an element property
or displayed between element tags, as in this example.

```html
<label>My current hero is {{hero.name}}</label>
```


Read more in the [Interpolation](guide/interpolation) guide.

{@a ivy}

## Ivy

Ivy is the code name for Angular's [next-generation compilation and rendering pipeline](https://blog.angular.io/a-plan-for-version-8-0-and-ivy-b3318dfc19f7).
With the version 9 release of Angular, the new compiler and runtime instructions are used by default instead of the older compiler and runtime, known as [View Engine](#ve).

See [Angular Ivy](guide/ivy).


{@a J}

{@a javascript}

## JavaScript

See [ECMAScript](#ecma), [TypeScript](#typescript).


{@a jit}


## compilación just-in-time (JIT)

The Angular just-in-time (JIT) compiler converts your Angular HTML and TypeScript code into
efficient JavaScript code at run time, as part of bootstrapping.

JIT compilation is the default (as opposed to AOT compilation) when you run Angular's `ng build` and `ng serve` CLI commands, and is a good choice during development.
JIT mode is strongly discouraged for production use
because it results in large application payloads that hinder the bootstrap performance.

Compare to [compilación ahead-of-time (AOT)](#aot).


{@a K}


{@a L}

{@a lazy-load}

## lazy loading

A process that speeds up application load time by splitting the application into multiple bundles and loading them on demand.
For example, dependencies can be lazy loaded as needed&mdash;as opposed to [eager-loaded](#eager-loading) modules that are required by the root module and are thus loaded on launch.

The [router](#router) makes use of lazy loading to load child views only when the parent view is activated.
Similarly, you can build custom elements that can be loaded into an Angular app when needed.

{@a library}

## library

In Angular, a [project](#project) that provides functionality that can be included in other Angular apps.
A library isn't a complete Angular app and can't run independently.
(To add re-usable Angular functionality to non-Angular web apps, you can use Angular [custom elements](#angular-element).)

* Library developers can use the [Angular CLI](#cli) to `generate` scaffolding for a new library in an existing [workspace](#workspace), and can publish a library as an `npm` package.

* Application developers can use the [Angular CLI](#cli) to `add` a published library for use with an application in the same [workspace](#workspace).

See also [schematic](#schematic).

{@a lifecycle-hook}

## lifecycle hook

An interface that allows you to tap into the lifecycle of [directives](#directive) and [components](#component) as they are created, updated, and destroyed.

Each interface has a single hook method whose name is the interface name prefixed with `ng`.
For example, the `OnInit` interface has a hook method named `ngOnInit`.

Angular calls these hook methods in the following order:

* `ngOnChanges`: When an [input](#input)/[output](#output) binding value changes.
* `ngOnInit`: After the first `ngOnChanges`.
* `ngDoCheck`: Developer's custom change detection.
* `ngAfterContentInit`: After component content initialized.
* `ngAfterContentChecked`: After every check of component content.
* `ngAfterViewInit`: After a component's views are initialized.
* `ngAfterViewChecked`: After every check of a component's views.
* `ngOnDestroy`: Just before the directive is destroyed.

To learn more, see [Lifecycle Hooks](guide/lifecycle-hooks).

{@a M}

{@a module}

## module

In general, a module collects a block of code dedicated to a single purpose. Angular uses standard JavaScript modules and also defines an Angular module, `NgModule`.

In JavaScript (ECMAScript), each file is a module and all objects defined in the file belong to that module. Objects can exported, making them public, and public objects can be imported for use by other modules.

Angular ships as a collection of JavaScript modules (also called libraries). Each Angular library name begins with the `@angular` prefix. Install Angular libraries with the [npm package manager](https://docs.npmjs.com/getting-started/what-is-npm) and import parts of them with JavaScript `import` declarations.

Compare to [NgModule](#ngmodule).


{@a N}

{@a ngcc}

## ngcc

Angular compatibility compiler.
If you build your app using [Ivy](#ivy), but it depends on libraries that have not been compiled with Ivy, the CLI uses `ngcc` to automatically update the dependent libraries to use Ivy.


{@a ngmodule}

## NgModule

A class definition preceded by the `@NgModule()` [decorator](#decorator), which declares and serves as a manifest for a block of code dedicated to an application domain, a workflow, or a closely related set of capabilities.

Like a [JavaScript module](#module), an NgModule can export functionality for use by other NgModules and import public functionality from other NgModules.
The metadata for an NgModule class collects components, directives, and pipes that the application uses along with the list of imports and exports. See also [declarable](#declarable).

NgModules are typically named after the file in which the exported thing is defined. For example, the Angular [DatePipe](api/common/DatePipe) class belongs to a feature module named `date_pipe` in the file `date_pipe.ts`. You import them from an Angular [scoped package](#scoped-package) such as `@angular/core`.

Every Angular application has a root module. By convention, the class is called `AppModule` and resides in a file named `app.module.ts`.

To learn more, see [NgModules](guide/ngmodules).

{@a npm-package}

## npm package

The [npm package manager](https://docs.npmjs.com/getting-started/what-is-npm) is used to distribute and load Angular modules and libraries.

Learn more about how Angular uses [Npm Packages](guide/npm-packages).

{@a O}

{@a observable}

## observable

A producer of multiple values, which it pushes to [subscribers](#subscriber). Used for asynchronous event handling throughout Angular. You execute an observable by subscribing to it with its `subscribe()` method, passing callbacks for notifications of new values, errors, or completion.

Observables can deliver single or multiple values of any type to subscribers, either synchronously (as a function delivers a value to its caller) or on a schedule. A subscriber receives notification of new values as they are produced and notification of either normal completion or error completion.

Angular uses a third-party library called [Reactive Extensions (RxJS)](http://reactivex.io/rxjs/).

To learn more, see [Observables](guide/observables).


{@a observer}

## observer

An object passed to the `subscribe()` method for an [observable](#observable). The object defines the callbacks for the [subscriber](#subscriber).

{@a output}

## output

When defining a [directive](#directive), the `@Output{}` decorator on a directive property
makes that property available as a *target* of [event binding](guide/event-binding).
Events stream *out* of this property to the receiver identified
in the [template expression](#template-expression) to the right of the equal sign.

To learn more, see [Input and Output Properties](guide/inputs-outputs).


{@a P}

{@a pipe}

## pipe

A class which is preceded by the `@Pipe{}` decorator and which defines a function that transforms input values to output values for display in a [view](#view). Angular defines various pipes, and you can define new pipes.

To learn more, see [Pipes](guide/pipes).

{@a platform}

## platform

In Angular terminology, a platform is the context in which an Angular application runs.
The most common platform for Angular applications is a web browser, but it can also be an operating system for a mobile device, or a web server.

Support for the various Angular run-time platforms is provided by the `@angular/platform-*` packages. These packages allow applications that make use of `@angular/core` and `@angular/common` to execute in different environments by providing implementation for gathering user input and rendering UIs for the given platform. Isolating platform-specific functionality allows the developer to make platform-independent use of the rest of the framework.

* When running in a web browser, [`BrowserModule`](api/platform-browser/BrowserModule) is imported from the `platform-browser` package, and supports services that simplify security and event processing, and allows applications to access browser-specific features, such as interpreting keyboard input and controlling the title of the document being displayed. All applications running in the browser use the same platform service.

* When [server-side rendering](#server-side-rendering) (SSR) is used, the [`platform-server`](api/platform-server) package provides web server implementations of the `DOM`, `XMLHttpRequest`, and other low-level features that don't rely on a browser.

{@a polyfill}

## polyfill

An [npm package](guide/npm-packages) that plugs gaps in a browser's JavaScript implementation.
See [Browser Support](guide/browser-support) for polyfills that support particular functionality for particular platforms.

{@a project}

## project

In the Angular CLI, a standalone application or [library](#library) that can be created or modified by a CLI command.

A project, as generated by the [`ng new`](cli/new), contains the set of source files, resources, and configuration files that you need to develop and test the application using the CLI. Projects can also be created with the `ng generate application` and `ng generate library` commands.

For more information, see [Project File Structure](guide/file-structure).

The [`angular.json`](guide/workspace-config) file configures all projects in a [workspace](#workspace).

{@a provider}

## provider

An object that implements one of the [`Provider`](api/core/Provider) interfaces. A provider object defines how to obtain an injectable dependency associated with a [DI token](#token).
An [injector](#injector) uses the provider to create a new instance of a dependency
for a class that requires it.

Angular registers its own providers with every injector, for services that Angular defines.
You can register your own providers for services that your app needs.

See also [service](#service), [dependency injection](#di).

Learn more in [Dependency Injection](guide/dependency-injection).


{@a Q}

{@a R}

{@a reactive-forms}

## reactive forms

A framework for building Angular forms through code in a component.
The alternative is a [template-driven form](#template-driven-forms).

When using reactive forms:

* The "source of truth", the form model, is defined in the component class.
* Validation is set up through validation functions rather than validation directives.
* Each control is explicitly created in the component class by creating a `FormControl` instance manually or with `FormBuilder`.
* The template input elements do *not* use `ngModel`.
* The associated Angular directives are prefixed with `form`, such as `formControl`, `formGroup`, and `formControlName`.

The alternative is a template-driven form. For an introduction and comparison of both forms approaches, see [Introduction to Angular Forms](guide/forms-overview).

{@a resolver}

## resolver

A class that implements the [Resolve](api/router/Resolve "API reference") interface (or a function with the same signature as the [resolve() method](api/router/Resolve#resolve "API reference")) that you use to produce or retrieve data that is needed before navigation to a requested route can be completed.

Resolvers run after all [route guards](#route-guard "Definition") for a route tree have been executed and have succeeded.

See an example of using a [resolve guard](guide/router-tutorial-toh#resolve-guard "Routing techniques tutorial") to retrieve dynamic data.

{@a route-guard}

## route guard

A method that controls navigation to a requested route in a routing application.
Guards determine whether a route can be activated or deactivated, and whether a lazy-loaded module can be loaded.

Learn more in the [Routing and Navigation](guide/router#preventing-unauthorized-access "Examples") guide.


{@a router}
{@a router-module}

## router

A tool that configures and implements navigation among states and [views](#view) within an Angular app.

The `Router` module is an [NgModule](#ngmodule) that provides the necessary service providers and directives for navigating through application views. A [routing component](#routing-component) is one that imports the `Router` module and whose template contains a `RouterOutlet` element where it can display views produced by the router.

The router defines navigation among views on a single page, as opposed to navigation among pages. It interprets URL-like links to determine which views to create or destroy, and which components to load or unload. It allows you to take advantage of [lazy loading](#lazy-load) in your Angular apps.

To learn more, see [Routing and Navigation](guide/router).

{@a router-outlet}

## router outlet

A [directive](#directive) that acts as a placeholder in a routing component's template. Angular dynamically renders the template based on the current router state.

{@a router-component}

## routing component

An Angular [component](#component) with a `RouterOutlet` directive in its template that displays views based on router navigations.

For more information, see [Routing and Navigation](guide/router).

{@a rule}

## rule

In [schematics](#schematic), a function that operates on a [file tree](#file-tree) to create, delete, or modify files in a specific manner.

{@a S}

{@a schematic}

## schematic

A scaffolding library that defines how to generate or transform a programming project by creating, modifying, refactoring, or moving files and code.
A schematic defines [rules](#rule) that operate on a virtual file system called a [tree](#file-tree).

The [Angular CLI](#cli) uses schematics to generate and modify [Angular projects](#project) and parts of projects.

* Angular provides a set of schematics for use with the CLI. See the [Angular CLI command reference](cli). The [`ng add`](cli/add) command runs schematics as part of adding a library to your project. The [`ng generate`](cli/generate) command runs schematics to create apps, libraries, and Angular code constructs.

* [Library](#library) developers can create schematics that enable the Angular CLI to add and update their published libraries, and to generate artifacts the library defines.
Add these schematics to the npm package that you use to publish and share your library.

For more information, see [Schematics](guide/schematics) and [Integrating Libraries with the CLI](guide/creating-libraries#integrating-with-the-cli).

{@a schematics-cli}

## Schematics CLI

Schematics come with their own command-line tool.
Using Node 6.9 or above, install the Schematics CLI globally:

<code-example language="bash">
npm install -g @angular-devkit/schematics-cli
</code-example>

This installs the `schematics` executable, which you can use to create a new schematics [collection](#collection) with an initial named schematic. The collection folder is a workspace for schematics. You can also use the `schematics` command to add a new schematic to an existing collection, or extend an existing schematic.

{@a scoped-package}

## scoped package

A way to group related [npm packages](guide/npm-packages).
NgModules are delivered within scoped packages whose names begin with the Angular *scope name* `@angular`. For example, `@angular/core`, `@angular/common`, `@angular/forms`, and `@angular/router`.

Import a scoped package in the same way that you import a normal package.

<code-example path="architecture/src/app/app.component.ts" header="architecture/src/app/app.component.ts (import)" region="import">

</code-example>

{@a server-side-rendering}

## server-side rendering

A technique that generates static application pages on the server, and can generate and serve those pages in response to requests from browsers.
It can also pre-generate pages as HTML files that you serve later.

This technique can improve performance on mobile and low-powered devices and improve the user experience by showing a static first page quickly while the client-side app is loading.
The static version can also make your app more visible to web crawlers.

You can easily prepare an app for server-side rendering by using the [CLI](#cli) to run the [Angular Universal](#universal) tool, using the `@nguniversal/express-engine` [schematic](#schematic).


{@a service}

## service

In Angular, a class with the [@Injectable()](#injectable) decorator that encapsulates non-UI logic and code that can be reused across an application.
Angular distinguishes components from services to increase modularity and reusability.

The `@Injectable()` metadata allows the service class to be used with the [dependency injection](#di) mechanism.
The injectable class is instantiated by a [provider](#provider).
[Injectors](#injector) maintain lists of providers and use them to provide service instances when they are required by components or other services.

To learn more, see [Introduction to Services and Dependency Injection](guide/architecture-services).

{@a structural-directive}
{@a structural-directives}

## structural directives

A category of [directive](#directive) that is responsible for shaping HTML layout by modifying the DOM&mdashthat is, adding, removing, or manipulating elements and their children.

To learn more, see [Structural Directives](guide/structural-directives).

{@a subscriber}

## subscriber

A function that defines how to obtain or generate values or messages to be published. This function is executed when a consumer calls the `subscribe()` method of an [observable](#observable).

The act of subscribing to an observable triggers its execution, associates callbacks with it, and creates a `Subscription` object that lets you unsubscribe.

The `subscribe()` method takes a JavaScript object (called an [observer](#observer)) with up to three callbacks, one for each type of notification that an observable can deliver:

* The `next` notification sends a value such as a number, a string, or an object.
* The `error` notification sends a JavaScript Error or exception.
* The `complete` notification doesn't send a value, but the handler is called when the call completes. Scheduled values can continue to be returned after the call completes.

{@a T}

{@a target}

## target

A buildable or runnable subset of a [project](#project), configured as an object in the [workspace configuration file](guide/workspace-config#project-tool-configuration-options), and executed by an [Architect](#architect) [builder](#builder).

In the `angular.json` file, each project has an "architect" section that contains targets which configure builders. Some of these targets correspond to [CLI commands](#cli), such as `build`, `serve`, `test`, and `lint`.

For example, the Architect builder invoked by the `ng build` command to compile a project uses a particular build tool, and has a default configuration whose values can be overridden on the command line. The `build` target also defines an alternate configuration for a "production" build, that can be invoked with the `--prod` flag on the `build` command.

The Architect tool provides a set of builders. The [`ng new` command](cli/new) provides a set of targets for the initial application project. The [`ng generate application`](cli/generate#application) and [`ng generate library`](cli/generate#library) commands provide a set of targets for each new [project](#project). These targets, their options and configurations, can be customized to meet the needs of your project. For example, you may want to add a "staging" or "testing" configuration to a project's "build" target.

You can also define a custom builder, and add a target to the project configuration that uses your custom builder. You can then run the target using the [`ng run`](cli/run) CLI command.

{@a template}

## template

Code that defines how to render a component's [view](#view).

A template combines straight HTML with Angular [data-binding](#data-binding) syntax, [directives](#directive),
and [template expressions](#template-expression) (logical constructs).
The Angular elements insert or calculate values that modify the HTML elements before the page is displayed. Learn more about Angular template language in the [Template Syntax](guide/template-syntax) guide.

A template is associated with a [component class](#component) through the `@Component()` [decorator](#decorator). The template code can be provided inline, as the value of the `template` property, or in a separate HTML file linked through the `templateUrl` property.

Additional templates, represented by `TemplateRef` objects, can define alternative or *embedded* views, which can be referenced from multiple components.

{@a template-driven-forms}

## template-driven forms

A format for building Angular forms using HTML forms and input elements in the view.
The alternative format uses the [reactive forms](#reactive-forms) framework.

When using template-driven forms:

* The "source of truth" is the template. The validation is defined using attributes on the individual input elements.
* [Two-way binding](#data-binding) with `ngModel` keeps the component model synchronized with the user's entry into the input elements.
* Behind the scenes, Angular creates a new control for each input element, provided you have set up a `name` attribute and two-way binding for each input.
* The associated Angular directives are prefixed with `ng` such as `ngForm`, `ngModel`, and `ngModelGroup`.

The alternative is a reactive form. For an introduction and comparison of both forms approaches, see [Introduction to Angular Forms](guide/forms-overview).

{@a template-expression}

## template expression

A TypeScript-like syntax that Angular evaluates within a [data binding](#data-binding).

Read about how to write template expressions in the [template expressions](guide/interpolation#template-expressions) section of the [Interpolation](guide/interpolation) guide.

{@a token}

## token

An opaque identifier used for efficient table lookup. In Angular, a [DI token](#di-token) is used to find [providers](#provider) of dependencies in the [dependency injection](#di) system.

{@a transpile}

## transpile

The translation process that transforms one version of JavaScript to another version; for example, down-leveling ES2015 to the older ES5 version.

{@a file-tree}

## tree

In [schematics](#schematic), a virtual file system represented by the `Tree` class.
Schematic [rules](#rule) take a tree object as input, operate on them, and return a new tree object.

{@a typescript}

## TypeScript

A programming language based on JavaScript that is notable for its optional typing system.
TypeScript provides compile-time type checking and strong tooling support (such as
code completion, refactoring, inline documentation, and intelligent search).
Many code editors and IDEs support TypeScript either natively or with plug-ins.

TypeScript is the preferred language for Angular development.
Read more about TypeScript at [typescriptlang.org](http://www.typescriptlang.org/).

## TypeScript configuration file

A file specifies the root files and the compiler options required to compile a TypeScript project. For more information, see [TypeScript configuration](/guide/typescript-configuration).


{@a U}

{@a unidirectional-data-flow}

## unidirectional data flow

A data flow model where the component tree is always checked for changes in one direction (parent to child), which prevents cycles in the change detection graph.

In practice, this means that data in Angular flows downward during change detection.
A parent component can easily change values in its child components because the parent is checked first.
A failure could occur, however, if a child component tries to change a value in its parent during change detection (inverting the expected data flow), because the parent component has already been rendered.
In development mode, Angular throws the `ExpressionChangedAfterItHasBeenCheckedError` error if your app attempts to do this, rather than silently failing to render the new value.

To avoid this error, a [lifecycle hook](guide/lifecycle-hooks) method that seeks to make such a change should trigger a new change detection run. The new run follows the same direction as before, but succeeds in picking up the new value.

{@a universal}

## Universal

A tool for implementing [server-side rendering](#server-side-rendering) of an Angular application.
When integrated with an app, Universal generates and serves static pages on the server in response to requests from browsers.
The initial static page serves as a fast-loading placeholder while the full application is being prepared for normal execution in the browser.

To learn more, see [Angular Universal: server-side rendering](guide/universal).

{@a V}

{@a view}

## view

The smallest grouping of display elements that can be created and destroyed together.
Angular renders a view under the control of one or more [directives](#directive).

A [component](#component) class and its associated [template](#template) define a view.
A view is specifically represented by a `ViewRef` instance associated with a component.
A view that belongs immediately to a component is called a *host view*.
Views are typically collected into [view hierarchies](#view-tree).

Properties of elements in a view can change dynamically, in response to user actions;
the structure (number and order) of elements in a view can't.
You can change the structure of elements by inserting, moving, or removing nested views within their view containers.

View hierarchies can be loaded and unloaded dynamically as the user navigates through the application, typically under the control of a [router](#router).

{@a ve}

## View Engine

The compilation and rendering pipeline used by Angular before version 9. Compare [Ivy](#ivy).


{@a view-tree}

## view hierarchy

A tree of related views that can be acted on as a unit. The root view is a component's *host view*. A host view can be the root of a tree of *embedded views*, collected in a *view container* (`ViewContainerRef`) attached to an anchor element in the hosting component. The view hierarchy is a key part of Angular [change detection](#change-detection).

The view hierarchy doesn't imply a component hierarchy. Views that are embedded in the context of a particular hierarchy can be host views of other components. Those components can be in the same NgModule as the hosting component, or belong to other NgModules.

{@a W}
{@a web-component}

## web component

See [custom element](#custom-element).

{@a workspace}

## workspace

A collection of Angular [projects](#project) (that is, applications and libraries) powered by the [Angular CLI] (#cli) that are typically co-located in a single source-control repository (such as [git](https://git-scm.com/)).

The [CLI](#cli) [`ng new` command](cli/new) creates a file system directory (the "workspace root").
In the workspace root, it also creates the workspace [configuration file](#configuration) (`angular.json`) and, by default, an initial application project with the same name.

Commands that create or operate on apps and libraries (such as `add` and `generate`) must be executed from within a workspace folder.

For more information, see [Workspace Configuration](guide/workspace-config).

{@a cli-config}

{@a config}

## workspace configuration

A file named `angular.json` at the root level of an Angular [workspace](#workspace) provides workspace-wide and project-specific configuration defaults for build and development tools that are provided by or integrated with the [Angular CLI](#cli).

For more information, see [Workspace Configuration](guide/workspace-config).

Additional project-specific configuration files are used by tools, such as `package.json` for the [npm package manager](#npm-package), `tsconfig.json` for [TypeScript transpilation](#transpile), and `tslint.json` for [TSLint](https://palantir.github.io/tslint/).

For more information, see [Workspace and Project File Structure](guide/file-structure).

{@a X}


{@a Y}


{@a Z}
{@a zone}

## zone

An execution context for a set of asynchronous tasks. Useful for debugging, profiling, and testing apps that include asynchronous operations such as event processing, promises, and calls to remote servers.

An Angular app runs in a zone where it can respond to asynchronous events by checking for data changes and updating the information it displays by resolving [data bindings](#data-binding).

A zone client can take action before and after an async operation completes.

Learn more about zones in this
[Brian Ford video](https://www.youtube.com/watch?v=3IqtmUscE_U).
