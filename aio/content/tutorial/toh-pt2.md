# Mostrar una lista de selección

En esta página, ampliaremos la aplicación Tour de Héroes para mostrar una lista de héroes,
Permite al usuario seleccionar un héroe y ver los detalles del héroe.

<div class="alert is-helpful">

Para ver la aplicación de ejemplo que describe esta página, consulte el  <live-example></live-example>.

</div>


## Crea un simulacro de héroe

Primero, necesitarás algunos héroes para mostrar.
Eventualmente los obtendrá de un servidor de datos remoto. Por ahora, creará algunos _héroes simulados_ y pretenderá que provienen del servidor.

Cree un archivo llamado `mock-heroes.ts` en la carpeta `src/app/`.
Defina la constante `HEROES` como un conjunto de 10 héroes y expórtela.
El archivo se verá así:

<code-example path="toh-pt2/src/app/mock-heroes.ts" header="src/app/mock-heroes.ts"></code-example>

## Mostrar Héroe

Abra el archivo de clase `HeroesComponent` e importe el mock `HEROES`.

<code-example path="toh-pt2/src/app/heroes/heroes.component.ts" region="import-heroes" header="src/app/heroes/heroes.component.ts (import HEROES)">
</code-example>

En el mismo archivo (clase `HeroesComponent`), defina una propiedad de componente llamada `heroes` para exponer el array HEROES para la vinculación.

<code-example path="toh-pt2/src/app/heroes/heroes.component.ts" header="src/app/heroes/heroes.component.ts" region="component">
</code-example>

### Enumerar héroes con `*ngFor`

Abra las Plantillas `HeroesComponent` y realice los siguientes cambios:

* Agregue `<h2>` al principio
* Agregue una lista HTML desordenada (`<ul>`) debajo de ella
* Inserte `<li>` dentro del `<ul>` que muestra la propiedad `hero`
* Espolvorea algunas clases CSS al estilo (agregaremos estilos CSS en breve)

Se parece a esto:

<code-example path="toh-pt2/src/app/heroes/heroes.component.1.html" region="list" header="heroes.component.html (heroes template)"></code-example>

Esto muestra un héroe. Para enumerarlos a todos, agregue `*ngFor*` a `<li>` para iterar sobre la lista de héroes.

<code-example path="toh-pt2/src/app/heroes/heroes.component.1.html" region="li">
</code-example>

[`*ngFor`](guide/template-syntax#ngFor) es la directiva de _repetición_ de Angular.

Esto repite el elemento host para cada elemento de la lista.

La sintaxis para este ejemplo es:

* `<li>` es un elemento host
* `heroes` es una lista de clases `HeroesComponent` que contiene la lista de héroes simulados
* `hero` contiene el objeto héroe actual en una lista para cada ciclo

<div class="alert is-important">

No olvide el asterisco (*) antes del `ngFor`. Esta es una parte importante de la sintaxis.

</div>

Actualiza tu navegador para ver la lista de héroes.

{@a styles}

### Agregar estilo a los héroes

La lista de héroes debe ser atractiva y visualmente prominente cuando el usuario coloca el cursor y selecciona un héroe de la lista.

En [Primer tutorial](tutorial/toh-pt0#app-wide-styles), configuro el estilo básico de toda la aplicación en `styles.css`.

No incluí el estilo de la lista de héroes en esta hoja de estilo.

Puede agregar más estilos a `styles.css` y seguir expandiendo esa hoja de estilo a medida que agrega componentes

Es posible que prefiera definir un estilo privado para un componente en particular y mantener todo lo que el componente necesita &mdash;
código, HTML, CSS&mdash; en un solo lugar.

Este enfoque facilita la reutilización del componente en otro lugar y aún así proporciona al componente la apariencia deseada, incluso cuando los estilos aplicados globalmente son diferentes.

Los estilos privados se definen en línea dentro de la matriz `@Component.styles` o como un archivo de hoja de estilo identificado en una matriz particular `@Component.styleUrls` como un archivo de hoja de estilo.

Cuando la CLI crea un `HeroesComponent`, se crea un `heroes.component.css` vacío para el `HeroesComponent`.
`@Component.styleUrls` se señala de esta manera.

<code-example path="toh-pt2/src/app/heroes/heroes.component.ts" region="metadata"
 header="src/app/heroes/heroes.component.ts (@Component)">
</code-example>

Abra `heroes.component.css` y pegue el estilo privado para `HeroesComponent`.
Puede encontrarlos en la [Revisión del código final](#final-code-review) al final de esta guía.

<div class="alert is-important">

`@Component` Los estilos y las hojas de estilo identificados en los metadatos se definen en un componente en particular.
El estilo `heroes.component.css` solo se aplica a `HeroesComponent` y no afecta a otros HTML o HTML dentro de ningún otro componente.
</div>

## Maestro/Detalle

Cuando haces clic en un héroe en la lista **maestro**, el componente debe mostrar los **detalles** del héroe seleccionado en la parte inferior de la página.

En este capítulo, esperemos a que se haga clic en el elemento del héroe y luego actualice los detalles del héroe.

### Agregar enlace de evento de clic

Agregue el enlace de evento click a su `<li>` así:

<code-example path="toh-pt2/src/app/heroes/heroes.component.1.html" region="selectedHero-click" header="heroes.component.html (template excerpt)"></code-example>

Este es un ejemplo de la sintaxis de Angular [enlace de eventos](guide/template-syntax#event-binding) .

Los paréntesis alrededor del `clic` le dicen a Angular que es un evento `clic` para el elemento `<li>`.
Cuando el usuario hace clic en `<li>`, Angular ejecuta la expresión `onSelect(hero)`.

En la siguiente sección, definiremos el método `onSelect()` en el `HeroesComponent` para mostrar los héroes definidos por las expresiones `*ngFor`.

### Agregar un controlador de eventos de clic

Cambie el nombre de la propiedad `hero` del componente a `selectedHero`, pero no la asigne todavía.
No hay _héroe seleccionado_ cuando se inicia la aplicación.

Agregue el método `onSelect()` de la siguiente manera y asigne el héroe en el que se hizo clic desde Plantillas al componente 'seleccionadoHero`.

<code-example path="toh-pt2/src/app/heroes/heroes.component.ts" region="on-select" header="src/app/heroes/heroes.component.ts (onSelect)"></code-example>

### Agregar una sección de detalles

Actualmente, el componente Plantillas tiene una lista.
Para hacer clic en un héroe en la lista para ver los detalles de ese héroe, necesita una sección de detalles para representarlo en Plantillas.
Agregue lo siguiente debajo de la sección de la lista de `heroes.component.html`.

<code-example path="toh-pt2/src/app/heroes/heroes.component.html" region="selectedHero-details" header="heroes.component.html (selected hero details)"></code-example>

Cuando actualizo el navegador, la aplicación está rota.

Abra las herramientas de desarrollador de su navegador y busque un mensaje de error como este en la consola:

<code-example language="sh" class="code-shell">
  HeroesComponent.html:3 ERROR TypeError: no se puede leer la propiedad 'nombre' de undefined
</code-example>

### ¿Que pasó?

Cuando inicia la aplicación, `selectedHero` es _intencionalmente_ indefinido`.

Los enlaces de expresión en Plantillas que se refieren a las propiedades de `selectedHero` - expresiones como` {{{selectedHero.name}} `deben _fallar_ porque el héroe seleccionado no existe, no.

### Reparemos-use  _*ngIf_ para ocultar detalles vacíos

El componente solo debe mostrar detalles para el héroe seleccionado si `selectedHero` está presente.

Adjunte los detalles del héroe en HTML `<div>`.
Agregue la directiva angular `*ngIf` a su `<div>` y configúrelo en `selectedHero`.

<div class="alert is-important">

No olvide el asterisco (*) antes del `ngIf`. Esta es una parte importante de la sintaxis.

</div>

<code-example path="toh-pt2/src/app/heroes/heroes.component.html" region="ng-if" header="src/app/heroes/heroes.component.html (*ngIf)"></code-example>

Actualice su navegador y verá la lista de nombres nuevamente.
El área de detalles está en blanco.
Haga clic en un héroe en la lista de héroes para ver más detalles.
La aplicación comenzó a funcionar nuevamente.
Los héroes se muestran en la lista, y los detalles del héroe seleccionado se muestran en la parte inferior de la página.

### Por qué esto funciona

Cuando `selectedHero` no está definido, `ngIf` elimina los detalles del héroe del DOM. No hay obligación de preocuparse por `selectedHero`.

Cuando el usuario selecciona un héroe, `selectedHero` tiene un valor y `ngIf` inserta los detalles del héroe en el DOM.

### Dar estilo a el héroe seleccionado

Si todos los elementos `<li>` se parecen, es difícil identificar al _héroe seleccionado_ en la lista.

Si el usuario hace clic en "Magneta", el héroe debe dibujarse con un color de fondo prominente como este:

<div class="lightbox">
  <img src='generated/images/guide/toh/heroes-list-selected.png' alt="Selected hero">
</div>

El color del _héroe seleccionado_ es el trabajo de la clase CSS `.selected` en [el estilo que acaba de agregar](#styles).
Simplemente aplica la clase `.selected` a `<li>` cuando el usuario hace clic.

El [enlace de clase](guide/template-syntax#class-binding)de Angular facilita la adición y eliminación de clases CSS condicionales.
Simplemente agregue `[class.some-css-class] =" some-condition"` al elemento que desea decorar.

Agregue el enlace `[class.selected]` al `<li>` en `HeroesComponent` Plantillas:

<code-example path="toh-pt2/src/app/heroes/heroes.component.1.html" region="class-selected" header="heroes.component.html (toggle the 'selected' CSS class)"></code-example>

Si el héroe de la fila actual es el mismo que de `selectedHero`, Angular agrega una clase CSS `selected`.Cuando los dos héroes son diferentes, Angular eliminará la clase.

El `<li>` completado se ve así:

<code-example path = "toh-pt2/src/app/heroes/heroes.component.html" region = "li" header = "heroes.component.html (elemento de lista hero)"> </code-example>

{@a final-code-review}

## Revisión final del código

Aquí está el archivo de código en esta página que incluye el estilo `HeroesComponent`.

<code-tabs>

  <code-pane header="src/app/mock-heroes.ts" path="toh-pt2/src/app/mock-heroes.ts">
  </code-pane>
  
  <code-pane header="src/app/heroes/heroes.component.ts" path="toh-pt2/src/app/heroes/heroes.component.ts">
  </code-pane>

  <code-pane header="src/app/heroes/heroes.component.html" path="toh-pt2/src/app/heroes/heroes.component.html">
  </code-pane>

  <code-pane header="src/app/heroes/heroes.component.css" path="toh-pt2/src/app/heroes/heroes.component.css">
  </code-pane>
</code-tabs>

## Resumen

* La aplicación "Tour de Héroes" muestra una lista de héroes en la pantalla Maestro / Detalle
* El usuario puede seleccionar un héroe y ver los detalles de ese héroe
* Utilizó `*ngFor` para mostrar la lista
* Utilizó `*ngIf` para incluir o excluir condicionalmente bloques de HTML
* La clase de estilo CSS se puede cambiar con el enlace `class`
