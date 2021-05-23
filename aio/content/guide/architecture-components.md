# Introducción a componentes y plantillas

Un *componente* controla una parte en pantalla llamada [*vista*](guide/glossary#view "Definition of view").
Por ejemplo, los componentes individuales definen y controlan cada una de las siguientes vistas del [tutorial Tour of Heroes](tutorial):

* La raíz de la aplicación con los enlaces de navegación.
* La lista de héroes.
* El editor de héroe.

Tu defines la lógica de aplicación de un componente &mdash; lo que hace para apoyar la vista &mdash; dentro de una clase.
La clase interactúa con la vista a través de una API de propiedades y métodos.

Por ejemplo, `HeroListComponent` tiene una propiedad `heroes` que contiene un array de héroes.
Tu método `selectHero()` establece una propiedad `selectedHero` cuando el usuario hace clic para elegir un héroe de esa lista.
El componente adquiere los héroes desde un servicio, que es una [propiedad de parámetro](https://www.typescriptlang.org/docs/handbook/2/classes.html#parameter-properties) de TypeScript en el constructor.
El servicio es proporcionado al componente a través del sistema de inyección de dependencias.

<code-example path="architecture/src/app/hero-list.component.ts" header="src/app/hero-list.component.ts (class)" region="class"></code-example>

Angular crea, actualiza y destruye componentes a medida que el usuario se mueve por la aplicación. Tu aplicación puede tomar medidas en cada momento de este ciclo de vida a través de [enlaces de ciclo de vida](guide/lifecycle-hooks) opcionales, como `ngOnInit()`.

{@a component-metadata}

## Metadatos de componentes

<img src="generated/images/guide/architecture/metadata.png" alt="Metadatos" class="left">

El decorador `@Component` identifica la clase inmediatamente debajo de ella como una clase de componente y especifica sus metadatos. En el código de ejemplo a continuación, puedes ver que `HeroListComponent` es solo una clase, sin ninguna notación o sintaxis de angular especial. No es un componente hasta que lo marcas como uno con el decorador `@Component`.

Los metadatos de un componente le dicen a Angular dónde obtener los bloques de construcción principales que necesita para crear y presentar el componente y su vista. En particular, asocia una *plantilla* con el componente, ya sea directamente con el código en línea o por referencia. Juntos, el componente y su plantilla describen una *vista*.

Además de contener o apuntar a la plantilla, los metadatos de `@Component` configuran, por ejemplo, cómo puede ser referenciado en HTML el componente y qué servicios requiere.

Aquí hay un ejemplo de metadatos básicos para `HeroListComponent`.

<code-example path="architecture/src/app/hero-list.component.ts" header="src/app/hero-list.component.ts (metadata)" region="metadata"></code-example>

Este ejemplo muestra algunas de las opciones de configuración de `@Component` más útiles:

* `selector`: Un selector de CSS que le dice a Angular que cree e inserte una instancia de este componente siempre que encuentre la etiqueta correspondiente en la plantilla HTML. Por ejemplo, si el HTML de una aplicación contiene `<app-hero-list></app-hero-list>`, entonces
Angular inserta una instancia de la vista `HeroListComponent` entre esas etiquetas.

* `templateUrl`: La dirección relativa al módulo de la plantilla HTML de este componente. Alternativamente, puede proporcionar la plantilla HTML en línea, como el valor de la propiedad `template`. Esta plantilla define la *vista host* del componente.

* `proveedores`: Un array de [proveedores](guide/glossary#provider) para los servicios que requiere el componente. En el ejemplo, esto le dice a Angular cómo proporcionar la instancia de `HeroService` que el constructor del componente usa para obtener la lista de héroes para mostrar.

{@a templates-and-views}

## Plantillas y vistas

<img src="generated/images/guide/architecture/template.png" alt="Plantilla" class="left">

Tu defines la vista de un componente con su plantilla complementaria. Una plantilla es una forma de HTML que le dice a Angular cómo renderizar el componente.

Las vistas generalmente se organizan jerárquicamente, lo que le permite modificar o mostrar y ocultar secciones enteras de UI o páginas como una unidad. La plantilla asociada inmediatamente con un componente define la *vista host* de ese componente. El componente también puede definir una *jerarquía de vistas*, que contiene *vistas incrustadas*, alojadas por otros componentes.

<div class="lightbox">
  <img src="generated/images/guide/architecture/component-tree.png" alt="Árbol de componentes" class="left">
</div>

Una jerarquía de vistas puede incluir vistas de componentes en el mismo NgModule, pero también puede (y a menudo lo hace) incluir vistas de componentes que están definidos en diferentes NgModules.

{@a template-syntax}

## Sintaxis de plantilla

Una plantilla se ve como HTML normal, excepto que también contiene [sintaxis de plantilla](guide/template-syntax) de Angular, que altera el HTML según la lógica de su aplicación y el estado de la aplicación y datos del DOM. Tu plantilla puede usar *el enlace de datos* para coordinar la aplicación y los datos del DOM, *pipes* para transformar los datos antes de que se muestren y *directivas* para aplicar lógica de aplicación a lo que es mostrado.

Por ejemplo, aquí hay una plantilla para el `HeroListComponent` del Tutorial.

<code-example path="architecture/src/app/hero-list.component.html" header="src/app/hero-list.component.html"></code-example>

Esta plantilla utiliza elementos HTML típicos como `<h2>` y `<p>`, y también incluye elementos de sintaxis de plantilla angular, `*ngFor`, `{{hero.name}}`, `(click)`, `[heroe]` y `<app-hero-detail>`. Los elementos de sintaxis de plantilla le dicen a Angular cómo representar el HTML en la pantalla, utilizando la lógica y los datos del programa.

* La directiva `*ngFor` le dice a Angular que itere una lista.
* `{{hero.name}}`, `(click)`, y `[hero]` enlazan los datos del programa hacia y desde el DOM, respondiendo a entradas del usuario. Obtenga más información sobre [enlace de datos](#data-binding).
* La etiqueta `<app-hero-detail>` en el ejemplo es un elemento que representa un nuevo componente, `HeroDetailComponent`.
`HeroDetailComponent` (código no mostrado) define la vista del [PENDIENTE]hijo hero-detail `HeroListComponent`.
Observe cómo los componentes personalizados como este se combinan a la perfección con HTML nativo en los mismos diseños.

{@a data-binding}

### Enlace de datos

Sin un marco, sería responsable de introducir valores de datos en los controles HTML y convertir las respuestas de los usuarios en acciones y actualizaciones de valores. Escribir esa lógica de empujar y tirar a mano es tedioso, propenso a errores y una pesadilla de leer, como puede atestiguar cualquier programador de JavaScript de front-end experimentado.

Angular admite *enlace de datos bidireccional*, un mecanismo para coordinar las partes de una plantilla con las partes de un componente. Agregue marcado de enlace a la plantilla HTML para decirle a Angular cómo conectar ambos lados.

El siguiente diagrama muestra las cuatro formas de marcado de enlace de datos. Cada formulario tiene una dirección: al DOM, desde el DOM o ambos.

<div class="lightbox">
  <img src="generated/images/guide/architecture/databinding.png" alt="Data Binding" class="left">
</div>

Este ejemplo de la plantilla `HeroListComponent` utiliza tres de estos formularios.

<code-example path="architecture/src/app/hero-list.component.1.html" header="src/app/hero-list.component.html (binding)" region="binding"></code-example>

* El `{{hero.name}}` [*interpolación*](guide/displaying-data#interpolation)
muestra el valor de la propiedad `hero.name` del componente dentro del elemento`<li>`.

* El `[heroe]` [*enlace de propiedad*](guide/property-binding) pasa el valor de
`selectedHero` del padre `HeroListComponent` a la propiedad `hero` del hijo `HeroDetailComponent`.

* El `(click)` [*event binding*](guide/user-input#binding-to-user-input-events) llama al método `selectHero` del componente cuando el usuario hace clic en el nombre de un héroe.

Enlace de datos bidireccional (utilizado principalmente en [formularios basados ​​en plantillas](guide/forms))
combina la propiedad y la vinculación de eventos en una sola notación.
Aquí hay un ejemplo de la plantilla `HeroDetailComponent` que usa enlace de datos bidireccional con la directiva `ngModel`.

<code-example path="architecture/src/app/hero-detail.component.html" header="src/app/hero-detail.component.html (ngModel)" region="ngModel"></code-example>

En el enlace bidireccional, un valor de propiedad de datos fluye al cuadro de entrada desde el componente como con el enlace de propiedad.
Los cambios del usuario también regresan al componente, restableciendo la propiedad al último valor,
como con la vinculación de eventos.

Angular procesa *todos* los enlaces de datos una vez para cada ciclo de eventos de JavaScript,
desde la raíz del árbol de componentes de la aplicación a través de todos los componentes secundarios.

<div class="lightbox">
  <img src="generated/images/guide/architecture/component-databinding.png" alt="Data Binding" class="left">
</div>

El enlace de datos juega un papel importante en la comunicación entre una plantilla y su componente, y también es importante para la comunicación entre los componentes principal y secundario.

<div class="lightbox">
  <img src="generated/images/guide/architecture/parent-child-binding.png" alt="Parent/Child binding" class="left">
</div>

### Pipes

Los pipes de Angular te permiten declarar transformaciones de valor de visualización en su plantilla HTML. Una clase con el decorador `@Pipe` define una función que transforma los valores de entrada en valores de salida para mostrarlos en una vista.

Angular define varios pipes, como el pipe [date](api/common/DatePipe) y el pipe [currency](api/common/CurrencyPipe); para obtener una lista completa, consulta la [Lista de API de pipes](api?type=pipe). También puedes definir nuevos pipe.

Para especificar una transformación de valor en una plantilla HTML, usa el [operador de tubería (|)](guide/template-expression-operators#pipe).

`{{interpolated_value | pipe_name}}`

Puedes encadenar pipes, enviando la salida de una función de pipe para que sea transformada por otra función de pipe. Un pipe también puede tomar argumentos que controlen cómo realiza su transformación. Por ejemplo, puedes pasar el formato deseado al pipe `fecha`.

```
  <!-- Formato predeterminado: salida '15 de junio de 2015'-->
  <p> Hoy es {{hoy | fecha}}</p>

 <!-- formato fullDate: salida 'Lunes, 15 de junio de 2015'-->
 <p> La fecha es {{hoy | fecha:'fullDate'}}</p>

  <!-- formato de tiempo corto: salida '9:43 AM'-->
  <p> La hora es {{hoy | fecha:'shortTime'}}</p>
```

{@a directives}

### Directivas

<img src="generated/images/guide/architecture/directive.png" alt="Directives" class="left">

Las plantillas de Angular son *dinámicas*. Cuando Angular los renderiza, transforma el DOM de acuerdo con las instrucciones dadas por *directivas*. Una directiva es una clase con un decorador `@Directive()`.

Un componente es técnicamente una directiva.
Sin embargo, los componentes son tan distintivos y fundamentales para las aplicaciones de Angular que Angular
define el decorador `@Component()`, que extiende el decorador `@Directive()` con
características orientadas a plantillas.

Además de los componentes, existen otros dos tipos de directivas: *estructural* y *atributo*.
Angular define una serie de directivas de ambos tipos, y puedes definir las tuyas propias usando el decorador `@Directive()`.

Al igual que para los componentes, los metadatos de una directiva asocian la clase decorada con un elemento `selector` que usas para insertarlo en HTML. En las plantillas, las directivas suelen aparecer dentro de una etiqueta de elemento como atributos, ya sea por nombre o como el destino de una asignación o un enlace.

#### Directivas estructurales

* Directivas estructurales * modifican el diseño agregando, quitando y reemplazando elementos en el DOM.
La plantilla de ejemplo utiliza dos directivas estructurales integradas para agregar lógica de aplicación a cómo se representa la vista.

<code-example path="architecture/src/app/hero-list.component.1.html" header="src/app/hero-list.component.html (structural)" region="structural"></code-example>

* [`*ngFor`](guide/displaying-data#ngFor) es iterativo; le dice a Angular que elimine un `<li>` por héroe en la lista de `heroes`.
* [`*ngIf`](guide/displaying-data#ngIf) es un condicional; incluye el componente `HeroDetail` solo si existe un héroe seleccionado.

#### Directivas de atributos

* Las directivas de atributos * alteran la apariencia o el comportamiento de un elemento existente.
En las plantillas, se ven como atributos HTML normales, de ahí el nombre.

La directiva `ngModel`, que implementa el enlace de datos bidireccional, es un ejemplo de directiva de atributo. `ngModel` modifica el comportamiento de un elemento existente (típicamente `<input>`) estableciendo su propiedad de valor de visualización y respondiendo a eventos de cambio.

<code-example path="architecture/src/app/hero-detail.component.html" header="src/app/hero-detail.component.html (ngModel)" region="ngModel"></code-example>

Angular tiene más directivas predefinidas que alteran la estructura del diseño
(por ejemplo, [ngSwitch](guide/built-in-directives#ngSwitch))
o modificar aspectos de elementos y componentes DOM
(por ejemplo, [ngStyle](guide/built-in-directives#ngStyle) y [ngClass](guide/built-in-directives#ngClass)).

<div class="alert is-helpful">

Obtén más información en las guías [Directivas de atributos](guide/attribute-directives) y [Directivas estructurales](guide/structural-directives).

</div>
