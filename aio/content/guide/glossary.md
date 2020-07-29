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

Obtén más información en [Vista general de Elementos Angular](guide/elements).

{@a annotation}

## anotaciones

Una estructura que proporciona metadatos para una clase. Ver [decorador](#decorator).

{@a app-shell}

## shell de aplicación

El shell de aplicación es una forma de representar una parte de su aplicación a través de una ruta en el momento de la compilación.
Esto brinda a los usuarios una primera visualización significativa de su aplicación que aparece rápidamente porque el navegador puede renderizar HTML y CSS estáticos sin la necesidad de inicializar JavaScript.

Obtén más información en [Modelo Shell de Aplicación](https://developers.google.com/web/fundamentals/architecture/app-shell).

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

Obtén más información en [Directivas de atributos](guide/attribute-directives).

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

Obtén más información en [Bootstrapping](guide/bootstrapping).

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

* Para comenzar a usar la CLI para un nuevo proyecto, consultá [Configuración del entorno local](guide/setup-local "Configuración para el desarrollo local").
* Para obtener más información sobre las capacidades completas de la CLI, consultá la [Referencia del comando CLI](cli).

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

Para obtener más información, consultá [Respuesta a cambios en el contenido](guide/lifecycle-hooks#content-projection).

{@a custom-element}

## elemento personalizado

Una función de plataforma web, actualmente compatible con la mayoría de los navegadores y disponible en otros navegadores a través de polyfills (consulta [Soporte del navegador](guide/browser-support)).

La característica de elemento personalizado extiende HTML al permitirle definir una etiqueta cuyo contenido es creado y controlado por código JavaScript. Un elemento personalizado (también llamado *componente web*) es reconocido por un navegador cuando se agrega a [CustomElementRegistry](https://developer.mozilla.org/en-US/docs/Web/API/CustomElementRegistry).

Puedes usar la API para transformar un componente Angular para que pueda registrarse con el navegador y usarse en cualquier HTML que agregue directamente al DOM dentro de una aplicación Angular. La etiqueta de elemento personalizado inserta la vista del componente, con la funcionalidad de detección de cambios y enlace de datos, en contenido que de otro modo se mostraría sin procesamiento angular.

Ver [Elemento angular](#angular-element).

Consultá también [carga de componentes dinámicos](#dynamic-components).

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

Una función que modifica una definición de clase o propiedad. Los decoradores (también llamados *anotaciones*) son experimentales (etapa 2) [característica del lenguaje JavaScript](https://github.com/wycats/javascript-decorators).
TypeScript agrega soporte para decoradores.

Angular define decoradores que adjuntan metadatos a clases o propiedades para que sepas qué significan esas clases o propiedades y cómo deberían funcionar.

Consulta [decorador de clase](#class-decorator), [decorador de campo de clase](#class-field-decorator).

{@a di}

{@a dependency-injection}

## inyección de dependencia (DI)

Un patrón de diseño y un mecanismo para crear y entregar algunas partes de una aplicación (dependencias) a otras partes de una aplicación que las requieran.

En Angular, las dependencias suelen ser servicios, pero también pueden ser valores, como cadenas o funciones.

Un [inyector](#injector) fpara una aplicación (creado automáticamente durante el arranque) crea instancias de dependencias cuando sea necesario, utilizando un [proveedor](#provider) configurado del servicio o valor.

Obtén más información en [Inyección de dependencia en Angular](guide/dependency-injection).

{@a di-token}

## token DI

Un token de búsqueda asociado con una dependencia [proveedor](#provider), para usar con el sistema [inyección de dependencia](#di).

{@a differential-loading}

## carga diferencial

Una técnica de compilación que crea dos paquetes para una aplicación. Un paquete más pequeño es para navegadores modernos. Un segundo paquete más grande permite que la aplicación se ejecute correctamente en navegadores más antiguos (como IE11) que no son compatibles con todas las API de navegador modernas.

Obtén más información en la guía de [Implementación](guide/deployment#differential-loading).


{@a directive}
{@a directives}

## directiva

Una clase que puede modificar la estructura del DOM o modificar atributos en el DOM y el modelo de datos de componentes. Una definición de clase directiva está precedida inmediatamente por un `@Directive()` [decorador](#decorator) que proporciona metadatos.

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

Consultá también [elemento personalizado](#custom-element), que proporciona una ruta más fácil con el mismo resultado.


{@a E}

{@a eager-loading}

## carga eager

Los NgModules o componentes que se cargan en el lanzamiento se llaman eager-loaded, para distinguirlos de aquellos que se cargan en tiempo de ejecución (carga diferida).

Ver [carga diferida](#lazy-load).


{@a ecma}

## ECMAScript

La [especificación oficial del lenguaje JavaScript](https://en.wikipedia.org/wiki/ECMAScript).

No todos los navegadores son compatibles con el último estándar ECMAScript, pero puede usar un [transpiler](#transpile) (como [TypeScript](#typescript)) para escribir código utilizando las últimas funciones, que luego se transpilarán al código que se ejecuta en las versiones que son compatibles con los navegadores.

Para obtener más información, consultá [Browser Support](guide/browser-support).


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

Obtén más información sobre los formularios reactivos y basados en plantillas en [Introducción a los formularios en angular](guide/forms-overview).

{@a form-validation}

## validación de formulario

Una comprobación que se ejecuta cuando cambian los valores del formulario e informa si los valores dados son correctos y completos, de acuerdo con las restricciones definidas. Se aplican formas reactivas [funciones de validación](guide/form-validation#adding-to-reactive-forms). Los formularios basados en plantillas usan [directivas de validación](guide/form-validation#adding-to-template-driven-forms).

Obtén más información en [Validación de formularios](guide/form-validation).


{@a G}


{@a H}

{@a I}


{@a immutability}

## inmutabilidad

La capacidad de alterar el estado de un valor después de su creación. [Formularios reactivos](#reactive-forms) realizan cambios inmutables en ese cada cambio en el modelo de datos produce un nuevo modelo de datos en lugar de modificar el existente. [Formas controladas por plantilla](#template-driven-forms) realizan cambios mutables con `NgModel` y [enlace de datos bidireccional](#data-binding) para modificar el modelo de datos existente en su lugar.

{@a injectable}

## inyectable

Una clase angular u otra definición que proporciona una dependencia utilizando el mecanismo de [inyección de dependencia](#di). Una clase inyectable [servicio](#service) debe estar marcada por el `@Injectable()` [decorador](#decorator). Otros elementos, como valores constantes, también pueden ser inyectables.

{@a injector}

## inyector

Un objeto en el sistema angular [inyección de dependencia](#dependency-injection) que puede encontrar una dependencia con nombre en su caché o crear una dependencia utilizando un [proveedor](#provider) configurado.
Los inyectores se crean para NgModules automáticamente como parte del proceso de arranque
y se heredan a través de la jerarquía de componentes.

* Un inyector proporciona una instancia singleton de una dependencia, y puede inyectar esta misma instancia en múltiples componentes.

* Una jerarquía de inyectores a nivel de NgModule y componente puede proporcionar diferentes instancias de dependencia a sus propios componentes y componentes secundarios.

* Puedes configurar inyectores con diferentes proveedores que pueden proporcionar diferentes implementaciones de la misma dependencia.

Obtén más información sobre la jerarquía de inyectores en [Inyectores de dependencia jerárquica](guide/hierarchical-dependency-injection).

{@a input}

## entrada

Al definir una [directiva](#directive), el decorador `@Input()` en una propiedad directiva
hace que esa propiedad esté disponible como *objetivo* de un [enlace de propiedad](guide/property-binding).
Los valores de datos fluyen a una propiedad de entrada desde la fuente de datos identificada en la [expresión de plantilla](#template-expression) a la derecha del signo igual.

Obtén más información en [propiedades de entrada y salida] (guide/inputs-outputs).

{@a interpolation}

## interpolación

Una forma de propiedad [enlace de datos](#data-binding)  en la que una [expresión de plantilla](#template-expression) entre llaves dobles se representa como texto.
Ese texto se puede concatenar con el texto vecino antes de asignarlo a una propiedad de elemento
o se muestra entre etiquetas de elementos, como en este ejemplo.

```html
<label>Mi héroe actual es {{hero.name}}</label>
```

Lee más en la guía [Interpolación](guide/interpolation).

{@a ivy}

## Ivy

Ivy es el nombre en clave del [canal de compilación y renderización de próxima generación](https://blog.angular.io/a-plan-for-version-8-0-and-ivy-b3318dfc19f7) de Angulae.
Con la versión 9 de Angular, el nuevo compilador y las instrucciones de tiempo de ejecución se utilizan de forma predeterminada en lugar del compilador y el tiempo de ejecución más antiguos, conocidos como [View Engine](#ve).

Lee más en [Angular Ivy](guide/ivy).

{@a J}

{@a javascript}

## JavaScript

Ver [ECMAScript](#ecma), [TypeScript](#typescript).


{@a jit}


## compilación just-in-time (JIT)

El compilador Angular just-in-time (JIT) convierte su código Angular HTML y TypeScript en código JavaScript eficiente en tiempo de ejecución, como parte del arranque.

La compilación JIT es el valor predeterminado (a diferencia de la compilación AOT) cuando ejecuta los comandos CLI `ng build` y`ng serve` de Angular, y es una buena opción durante el desarrollo.
El modo JIT no se recomienda para el uso en producción porque da como resultado grandes cargas útiles de aplicaciones que dificultan el rendimiento de arranque.

Comparar con [compilación ahead-of-time (AOT)](#aot).


{@a K}


{@a L}

{@a lazy-load}

## carga diferida

Un proceso que acelera el tiempo de carga de la aplicación al dividir la aplicación en múltiples paquetes y cargarlos bajo demanda.
Por ejemplo, las dependencias pueden cargarse de forma diferida según sea necesario en lugar de los módulos [eager-loaded](#eager-loading) que requiere el módulo raíz y, por lo tanto, se cargan en el inicio.

El [enrutador](#router) utiliza la carga diferida para cargar vistas secundarias solo cuando la vista principal está activada.
Del mismo modo, puede crear elementos personalizados que se pueden cargar en una aplicación Angular cuando sea necesario.

{@a library}

## biblioteca

En Angular, un [proyecto](#project) que proporciona funcionalidad que se puede incluir en otras aplicaciones de Angular.
Una biblioteca no es una aplicación angular completa y no puede ejecutarse de forma independiente.
Para agregar funcionalidad angular reutilizable a aplicaciones web no angulares, puede usar Angular [elementos personalizados](#angular-element).)

* Los desarrolladores de bibliotecas pueden usar la [CLI angular](#cli) para `generar` andamios para una nueva biblioteca en un [espacio de trabajo](#workspace) existente, y pueden publicar una biblioteca como un paquete `npm`.

* Los desarrolladores de aplicaciones pueden usar la [CLI angular](#cli) para `agregar` una biblioteca publicada para usar con una aplicación en el mismo [espacio de trabajo](#workspace).

Ver también [esquema](#schematic).

{@a lifecycle-hook}

## lifecycle hook

Una interfaz que le permite aprovechar el ciclo de vida de [directivas](#directive) and [componentes](#component) a medida que se crean, actualizan y destruyen.

Cada interfaz tiene un método de enlace único cuyo nombre es el nombre de la interfaz con el prefijo `ng`.
Por ejemplo, la interfaz `OnInit` tiene un método de enlace llamado `ngOnInit`.

Angular llama a estos métodos de enlace en el siguiente orden:

* `ngOnChanges`: Cuando cambia un valor de enlace [entrada](#input)/[salida](#output).
* `ngOnInit`: Después de los primeros `ngOnChanges`.
* `ngDoCheck`: Detección de cambios personalizados del desarrollador.
* `ngAfterContentInit`: Después del contenido del componente inicializado.
* `ngAfterContentChecked`: Después de cada verificación del contenido del componente.
* `ngAfterViewInit`: Después de que se inicializan las vistas de un componente.
* `ngAfterViewChecked`: Después de cada comprobación de las vistas de un componente.
* `ngOnDestroy`: Justo antes de que se destruya la directiva.

Obtén más información en [Lifecycle Hooks](guide/lifecycle-hooks).

{@a M}

{@a module}

## módulo

En general, un módulo recopila un bloque de código dedicado a un solo propósito. Angular utiliza módulos JavaScript estándar y también define un módulo angular, `NgModule`.

En JavaScript (ECMAScript), cada archivo es un módulo y todos los objetos definidos en el archivo pertenecen a ese módulo. Los objetos se pueden exportar, haciéndolos públicos, y los objetos públicos se pueden importar para su uso por otros módulos.

Angular se envía como una colección de módulos JavaScript (también llamados bibliotecas). Cada nombre de biblioteca angular comienza con el prefijo `@angular`. Instala bibliotecas angulares con el [administrador de paquetes npm](https://docs.npmjs.com/getting-started/what-is-npm) e importa partes de ellas con las declaraciones de JavaScript `import`.

Comparar con [NgModule](#ngmodule).


{@a N}

{@a ngcc}

## ngcc

Compilador de compatibilidad angular.
Si se crea la aplicación usando [Ivy](#ivy), pero depende de las bibliotecas que no se han compilado con Ivy, la CLI usa `ngcc` para actualizar automáticamente las bibliotecas dependientes para usar Ivy.


{@a ngmodule}

## NgModule

Una definición de clase precedida por el `@NgModule()` [decorador](#decorator),  que declara y sirve como manifiesto para un bloque de código dedicado a un dominio de aplicación, un flujo de trabajo o un conjunto de capacidades estrechamente relacionadas.

Al igual que un [módulo JavaScript](#module), un NgModule puede exportar la funcionalidad para que otros NgModules la usen e importar la funcionalidad pública de otros NgModules.
Los metadatos para una clase NgModule recopilan componentes, directivas y canalizaciones que la aplicación usa junto con la lista de importaciones y exportaciones. Ver también [declarable](#declarable).

Los NgModules generalmente llevan el nombre del archivo en el que se define lo exportado. Por ejemplo, la clase Angular [DatePipe](api/common/DatePipe) pertenece a un módulo de características llamado `date_pipe` en el archivo` date_pipe.ts`. Se importa desde un [paquete con scope] (#scoped-package) como `@angular/core`.

Cada aplicación angular tiene un módulo raíz. Por convención, la clase se llama `AppModule` y reside en un archivo llamado `app.module.ts`.

Para obtener más información, consulta [NgModules](guide/ngmodules).

{@a npm-package}

## paquete npm

El [administrador de paquetes npm](https://docs.npmjs.com/getting-started/what-is-npm) se usa para distribuir y cargar módulos y bibliotecas angular.

Obtén más información acerca de cómo Angular usa [Paquetes npm](guide/npm-packages).

{@a O}

{@a observable}

## observable

Un productor de múltiples valores, que empuja a [suscriptores](#subscriber). Se utiliza para el manejo de eventos asíncronos en todo Angular. Ejecutas un observable suscribiéndote con su método `subscribe()`, pasando devoluciones de llamada para notificaciones de nuevos valores, errores o finalización.

Los observables pueden entregar valores únicos o múltiples de cualquier tipo a los suscriptores, ya sea sincrónicamente (como una función entrega un valor a la persona que llama) o en un horario. Un suscriptor recibe una notificación de los nuevos valores a medida que se producen y una notificación de finalización normal o finalización de error.

Angular utiliza una biblioteca de terceros llamada [Extensiones reactivas (RxJS)](http://reactivex.io/rxjs/).

Obtén más información en [Observables](guide/observables).


{@a observer}

## observador

Un objeto pasado al método `subscribe()` para un [observable](#observable). El objeto define las devoluciones de llamada para el [suscriptor](#subscriber).

{@a output}

## salida

Al definir una [directiva](#directive), el decorador `@Output{}` en una propiedad directiva
hace que esa propiedad esté disponible como *objetivo* de [enlace de evento](guide/event-binding).
La secuencia de eventos *fuera* de esta propiedad al receptor identificado
en la [expresión de plantilla](#template-expression) a la derecha del signo igual.

Obtén más información en [Propiedades de entrada y salida](guide/inputs-outputs).


{@a P}

{@a pipe}

## pipe

Una clase precedida por el decorador `@Pipe{}` y que define una función que transforma los valores de entrada en valores de salida para mostrar en una [vista](#view). Angular define varias pipes y puedes definir nuevas pipes.

Obtén más información en [Pipes](guide/pipes).

{@a platform}

## plataforma

En terminología angular, una plataforma es el contexto en el que se ejecuta una aplicación angular.
La plataforma más común para aplicaciones angulares es un navegador web, pero también puede ser un sistema operativo para un dispositivo móvil o un servidor web.

Los paquetes `@angular/platform-*` proporcionan soporte para las diversas plataformas de tiempo de ejecución angular. Estos paquetes permiten que las aplicaciones que utilizan `@angular/core` y `@angular/common` se ejecuten en diferentes entornos al proporcionar la implementación para recopilar la entrada del usuario y representar las IU para la plataforma dada. El aislamiento de la funcionalidad específica de la plataforma permite al desarrollador hacer un uso independiente de la plataforma del resto del marco.

* Cuando se ejecuta en un navegador web, [`BrowserModule`](api/platform-browser/BrowserModule) se importa desde el paquete `platform-browser`, y admite servicios que simplifican la seguridad y el procesamiento de eventos, y permite que las aplicaciones accedan al navegador- características específicas, como interpretar la entrada del teclado y controlar el título del documento que se muestra. Todas las aplicaciones que se ejecutan en el navegador utilizan el mismo servicio de plataforma.

* Cuando se utiliza [renderizado del lado del servidor](#server-side-rendering) (SSR) el paquete  [`platform-server`](api/platform-server) proporciona implementaciones de servidor web de` DOM`, `XMLHttpRequest`, y otras características de bajo nivel que no dependen de un navegador.

{@a polyfill}

## polyfill

Un [paquete npm](guide/npm-packages) que cierra las brechas en la implementación de JavaScript de un navegador.
Consulta [Soporte del navegador](guide/browser-support) para ver polyfills que admiten funcionalidades particulares para plataformas particulares.

{@a project}

## proyecto

En la angular CLI, una aplicación independiente o [biblioteca](#library) que se puede crear o modificar mediante un comando de la CLI.

Un proyecto, generado por [`ng new`](cli/new), contiene el conjunto de archivos de origen, recursos y archivos de configuración que necesita para desarrollar y probar la aplicación utilizando la CLI. Los proyectos también se pueden crear con los comandos `ng generate application` y `ng generate library`.

Obtén más información en [Estructura del archivo del proyecto](guide/file-structure).

El archivo [`angular.json`](guide/workspace-config) configura todos los proyectos en un [espacio de trabajo](#workspace).

{@a provider}

## proveedor

Un objeto que implementa una de las interfaces [`Provider`](api/core/Provider). Un objeto proveedor define cómo obtener una dependencia inyectable asociada con un [token DI](#token).
Un [inyector](#injector) usa el proveedor para crear una nueva instancia de una dependencia para una clase que lo requiera.

Angular registra sus propios proveedores con cada inyector, para los servicios que Angular define.
Puede registrar sus propios proveedores para los servicios que su aplicación necesita.

Consulta también [servicio](#service), [inyección de dependencia](#di).

Obtén más información en [Inyección de dependencia](guide/dependency-injection).


{@a Q}

{@a R}

{@a reactive-forms}

## formularios reactivos

Un framework para construir forularios Angular a través del código en un componente.
La alternativa es un [formulario controlado por plantilla](#template-driven-forms).

Cuando se usan formularios reactivos:

* La "fuente de verdad", el modelo de formulario, se define en la clase de componente.
* La validación se configura mediante funciones de validación en lugar de directivas de validación.
* Cada control se crea explícitamente en la clase de componente creando una instancia de `FormControl` manualmente o con `FormBuilder`.
* Los elementos de entrada de la plantilla *no* usan `ngModel`.
* as directivas Angular asociadas tienen el prefijo `form`, como `formControl`, `formGroup` y `formControlName`.

La alternativa es un formulario basado en plantillas. Para una introducción y comparación de ambos enfoques de formularios, consulte [Introducción a los formularios angular](guide/forms-overview).

{@a resolver}

## resolver

Una clase que implementa la interfaz [Resolve](api/router/Resolve "Referencia API") (o una función con la misma firma que el método [resolve()](api/router/Resolve#resolve "Referencia API")) que utiliza para producir o recuperar los datos necesarios antes de poder completar la navegación a una ruta solicitada.

Los resolvers se ejecutan después de que todos los [guards de ruta](#route-guard "Definición") para un árbol de ruta se hayan ejecutado y hayan tenido éxito.

Ver un ejemplo de uso de [resolve guard](guide/router-tutorial-toh#resolve-guard "Tutorial de técnicas de enrutamiento") para recuperar datos dinámicos.

{@a route-guard}

## guard de ruta

Un método que controla la navegación a una ruta solicitada en una aplicación de enrutamiento.
Los guards determinan si una ruta se puede activar o desactivar, y si se puede cargar un módulo con carga diferida.

Obtén más información en [Enrutamiento y navegación](guide/router#preventing-unauthorized-access "Ejemplos").


{@a router}
{@a router-module}

## enrutador

Una herramienta que configura e implementa la navegación entre estados y [vistas](#view) dentro de una aplicación angular.

El módulo `Router` es un [NgModule](#ngmodule) que proporciona los proveedores de servicios y las directivas necesarias para navegar por las vistas de la aplicación. Un [componente de enrutamiento](#routing-component) es aquel que importa el módulo `Router` y cuya plantilla contiene un elemento `RouterOutlet` donde puede mostrar vistas producidas por el enrutador.

El enrutador define la navegación entre vistas en una sola página, a diferencia de la navegación entre páginas. Interpreta enlaces de tipo URL para determinar qué vistas crear o destruir, y qué componentes cargar o descargar. Le permite aprovechar [carga diferida](#lazy-load) en las aplicaciones angular.

Obtén más información en [Enrutamiento y navegación](guide/router).

{@a router-outlet}

## router outlet

Una [directiva](#directive) que actúa como marcador de posición en la plantilla de un componente de enrutamiento. Angular renderiza dinámicamente la plantilla en función del estado actual del enrutador.

{@a router-component}

## componente de enrutamiento

Un [componente](#component) con una directiva `RouterOutlet` en su plantilla que muestra vistas basadas en la navegación del enrutador.

Obtén más información en [Enrutamiento y navegación](guide/router).

{@a rule}

## regla

En [esquemas](#schematic), una función que opera en un [árbol de archivos](#file-tree) para crear, eliminar o modificar archivos de una manera específica.

{@a S}

{@a schematic}

## esquema

Una biblioteca de andamios que define cómo generar o transformar un proyecto de programación creando, modificando, refactorizando o moviendo archivos y códigos.
Un esquema define [reglas](#rule) que operan en un sistema de archivos virtual llamado [árbol](#file-tree).

La [angular CLI](#cli) utiliza esquemas para generar y modificar [proyectos Angular](#project) y partes de proyectos.

* Angular proporciona un conjunto de esquemas para usar con la CLI. Consulta la [Referencia de comando de angular CLI](cli). El comando [`ng add`](cli/add)  ejecuta esquemas como parte de agregar una biblioteca a su proyecto. El comando [`ng generate`](cli/generate) ejecuta esquemas para crear aplicaciones, bibliotecas y construcciones de código angular.

* Los desarrolladores de [Bibliotecas](#library) pueden crear esquemas que permitan a la CLI angular agregar y actualizar sus bibliotecas publicadas y generar artefactos que la biblioteca defina.
Agregue estos esquemas al paquete npm que usa para publicar y compartir su biblioteca.

Obtén más información en [Esquemas](guide/schematics) e [Integración de bibliotecas con la CLI](guide/creating-libraries#integrating-with-the-cli).

{@a schematics-cli}

## Esquema CLI

Los esquemas vienen con su propia herramienta de línea de comandos.
Usando Node 6.9 o superior, instala la CLI de esquemas globalmente:

<code-example language="bash">
npm install -g @angular-devkit/schematics-cli
</code-example>

Esto instala el ejecutable `schematics`, que puede usar para crear un nuevo esquema [colección](#collection) con un esquema inicial llamado. La carpeta de colección es un espacio de trabajo para esquemas. También puede usar el comando `schematics` para agregar un nuevo esquema a una colección existente, o extender un esquema existente.

{@a scoped-package}

## paquetes con scope

Una forma de agrupar [paquetes npm](guide/npm-packages) relacionados.
Los NgModules se entregan dentro de paquetes con ámbito cuyos nombres comienzan con Angular *nombre del scope* `@angular`. Por ejemplo, `@angular/core`, `@angular/common`, `@angular/forms`, y `@angular/router`.

Importa un paquete con alcance de la misma manera que se importa un paquete normal.

<code-example path="architecture/src/app/app.component.ts" header="architecture/src/app/app.component.ts (import)" region="import">

</code-example>

{@a server-side-rendering}

## renderizado del lado del servidor

Una técnica que genera páginas de aplicaciones estáticas en el servidor, y puede generar y servir esas páginas en respuesta a las solicitudes de los navegadores.
También puede pregenerar páginas como archivos HTML que servirá más adelante.

Esta técnica puede mejorar el rendimiento en dispositivos móviles y de baja potencia y mejorar la experiencia del usuario al mostrar una primera página estática rápidamente mientras se carga la aplicación del lado del cliente.
La versión estática también puede hacer que su aplicación sea más visible para los rastreadores web.

Puede preparar fácilmente una aplicación para la representación del lado del servidor utilizando la [CLI](#cli) para ejecutar la herramienta [Angular Universal](#universal) utilizando el [esquema](#schematic) `@nguniversal/express-engine`.


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
