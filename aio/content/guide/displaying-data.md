# Mostrando Datos en vistas

Los [componentes](guide/glossary#component) de Angular forman la estructura de los datos de su aplicación.
La [plantilla](guide/glossary#template) asociado con un componente provee los medios para mostrar esos datos en el contexto de una página web.
Juntos, una clase y un template de un componente forman una [vista](guide/glossary#view) de los datos de su aplicación.

El proceso de combinar valores con su representación en la página es llamado [Enlace de Datos](guide/glossary#data-binding).
Muestra tus datos a un usuario (y obtener datos del usuario) por *enlace* de controles en la plantilla HTML a las propiedades de datos de la clase del componente. 

En adición, puedes incluir lógica a la plantilla incluyendo [directivas](guide/glossary#directive), las cuales le dicen a Angular como modificar la página cuando es renderizada.

Angular define un *lenguaje de plantilla* que expande la notación HTML con sintáxis que permite definir varios tipos de enlaces de datos y directivas lógicas.
Cuando una página es renderizada, Angular interpreta la sintáxis de la plantilla para actualizar el HTML de acuerdo a su lógica y el estado actual de sus datos.
Antes de leer la completa [guía de sintaxis de plantilla](guide/template-syntax), los ejercicios en esta página te dan una rápida demostración de como funciona la sintaxis de plantilla.

En este demo, podrás crear un componente con una lista de héroes.
Podrás mostrar la lista de nombres de los héroes y condicionalmente mostrar un mensaje debajo de la lista.
La interfaz de usuario final se verá así:

<div class="lightbox">
  <img src="generated/images/guide/displaying-data/final.png" alt="Final UI">
</div>

<div class="alert is-helpful">

El <live-example></live-example> demuestra toda la sintaxis y los fragmentos de código descritos en esta página.

</div>

{@a interpolation}

## Mostrando propiedades del componente con interpolación
La manera mas sencilla de mostrar la propiedad de un componente es realizar el enlace del nombre de la propiedad por medio de interpolación.
Con interpolación, pones el nombre de la propiedad en la plantilla de vista, encerrado en llaves dobles: `{{myHero}}`.

Usa el comando CLI [`ng new displaying-data`](cli/new) para crear un nuevo espacio de trabajo y una aplicacón llamada `displaying-data`.

Borra el archivo <code>app.component.html</code>. Este no es necesario para este ejemplo.

Entonces modifica el archivo <code>app.component.ts</code> 
cambiando la plantilla y el cuerpo del componente.

Cuando hayas terminado, debería verse de la siguiente manera:

<code-example path="displaying-data/src/app/app.component.1.ts" header="src/app/app.component.ts"></code-example>

Agregaste dos propiedades al componente anteriormente vacío: `title` y `myHero`.

La plantilla muestra las propiedades de los dos componentes usando la interpolación de dobles llaves:

<code-example path="displaying-data/src/app/app.component.1.ts" header="src/app/app.component.ts (template)" region="template"></code-example>

<div class="alert is-helpful">

La plantilla es una cadena de texto multi-línea dentro de las comillas invertidas de ECMAScript 2015 (<code>\`</code>).
La comilla invertida (<code>\`</code>)&mdash;que *no* es la misma que una comilla sencilla (`'`)&mdash;te permite componer una cadena de texto de muchas líneas, 
lo que hace que el HTML sea mas legible.

</div>

Angular automaticamente pone el valor de las propiedades `title` y `myHero` del componente
 e inserta dichos valores en el navegador. Angular actualiza la vista cuando esas propiedades cambian.

<div class="alert is-helpful">

Mas precisamente, la revisualización ocurre despues de algun tipo de evento asíncrono relacionado a 
la vista, como pulsaciones de teclas, finalización de un temporizador, o la respuesta de una solicitud HTTP.

</div>

Observa que no se usa **new** para crear una instancia de una clase de `AppComponent`.
Angular está creando la instancia por tí, ¿Como?

El `selector` CSS en el decorador `@Component` especifica un elemento llamado `<app-root>`.
Ese elemento es un marcador de posición en el cuerpo de su archivo `index.html`:

<code-example path="displaying-data/src/index.html" header="src/index.html (body)" region="body"></code-example>

Cuando arranca con la clase `AppComponent` (en <code>main.ts</code>), Angular busca un `<app-root>`
en el `index.html`, lo encuentra, crea una instancia de `AppComponent`, y lo representa 
dentro de la etiqueta `<app-root>`.

Ahora corre la app. Debería mostrar el título y el nombre del héroe:

<div class="lightbox">
  <img src="generated/images/guide/displaying-data/title-and-hero.png" alt="Title and Hero">
</div>

Las siguientes secciones revisan algunas de las opciones de codificación de la aplicación.

## Escogiendo el origen de la plantilla

Los metadatos del `@Component` le dicen a Angular donde encontrar la plantilla del componente.
Puedes guardar la plantilla de tu componente en uno de dos lugares.

* Puedes definir la plantilla *inline* usando la propiedad `template` del decorador `@Component`. Una plantilla en línea es útil para un pequeño demo o prueba.
* Alternativamente, puedes definir la plantilla en un archivo HTML separado y enlazar ese archivo en la propiedad `templateUrl` del decorador `@Component`. Esta es una configuración típica para cualquier cosa mas compleja que un pequeño test o demo, y esto pasa por defecto cuando se genera un nuevo componente.

En cualquier estilo, el enlace de datos de la plantilla tiene el mismo acceso a las propiedades del componente.
Aquí la app usa HTML en línea por que la plantilla es pequeña y el demo es mas simple sin el archivo HTML adicional.

<div class="alert is-helpful">

  Por defecto, el comando de la CLI de Angular [`ng generate component`](cli/generate) genera componentes con un archivo de plantilla.
  Puede anular esto adicionando la opción "-t" (abreviación para `inlineTemplate=true`)

  <code-example hideCopy language="sh" class="code-shell">
    ng generate component hero -t
  </code-example>

</div>


## Inicialización

El siguiente ejemplo usa asignación de variables para inicializar los componentes.

<code-example path="displaying-data/src/app/app-ctor.component.1.ts" region="class"></code-example>

 En su lugar, podrías declarar e inicializar las propiedades utilizando un constructor.
 Esta aplicación utiliza un estilo de "asignación de variables" más conciso simplemente por brevedad.

{@a ngFor}

## Agregue lógica para recorrer los datos

La directiva `*ngFor` (predefinida por Anglar) permite recorrer los datos. El siguiente ejemplo usa la directiva para mostrar todos los valores de las propiedades de un arreglo.

Para mostrar una lista de héroes, comienza agregando un arreglo de nombres de héroes al componente y redefinir `myHero` para ser el primer nombre en el arreglo.

<code-example path="displaying-data/src/app/app.component.2.ts" header="src/app/app.component.ts (class)" region="class"></code-example>


Ahora usa la directiva de Angular  `ngFor` en la plantilla para mostrar cada item en la lista de `heroes`.


<code-example path="displaying-data/src/app/app.component.2.ts" header="src/app/app.component.ts (template)" region="template"></code-example>


Esta interfaz de usuario usa una lista desordenada de HTML con las etiquetas `<ul>` y `<li>`. El `*ngFor`
en el elemento `<li>` es la directiva "repetidora" de Angular.
Esto marca el elemento `<li>` (y sus hijos) como la "plantilla repetidora":


<code-example path="displaying-data/src/app/app.component.2.ts" header="src/app/app.component.ts (li)" region="li"></code-example>

<div class="alert is-important">

No olvides el asterisco inicial (\*) en `*ngFor`. Esto es parte escencial de la sintaxis.
Lee más acerca de `ngFor` y `*` en la [sección ngFor](guide/built-in-directives#ngfor) de la página de [directivas integradas](guide/built-in-directives).

</div>

Observa el `hero` en la instrucción` ngFor` entre comillas dobles;
este es un ejemplo de una plantilla de variable de entrada. 
Lee más acerca de plantillas de variables de entrada en la sección [microsintaxis](guide/built-in-directives#microsyntax) de
la página de [directivas integradas](guide/built-in-directives).

Angular duplica el `<li>` por cada elemento en la lista, configurando la variable `hero`
para el elemento (el héroe) en la iteración actual. Angular usa esta variable como el 
contexto para la interpolación dentro de las llaves dobles.

<div class="alert is-helpful">

En este caso, `ngFor` está mostrando un arreglo, pero `ngFor` puede
repetir elementos para cualquier objeto [iterable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols).

</div>

Ahora los héroes aparecen en una lista desordenada.

<div class="lightbox">
  <img src="generated/images/guide/displaying-data/hero-names-list.png" alt="After ngfor">
</div>


## Creando una clase para los datos

El código de la aplicación define los datos directamente dentro del componente, lo cual no es la mejor practica.
Sin embargo en un demo simple, está bien.

Hasta el momento, el enlace es a un arreglo de cadenas de texto.
En aplicaciones reales, muchos enlaces son a objetos mas especializados.

Para convertir este enlace para usar objetos especializados, convierta el arreglo
de nombres de héroes en un arreglo de objetos `Hero`. Para esto necesitarás una clase `Hero`:

<code-example language="sh" class="code-shell">
  ng generate class hero
</code-example>

Este comando crea el siguiente código.


<code-example path="displaying-data/src/app/hero.ts" header="src/app/hero.ts"></code-example>

Has definido una clase con un constructor y dos propiedades: `id` y `name`.

Puede que parezca que la clase no tiene propiedades, pero las tiene.
La declaración de los parámetros del constructor aprovecha un atajo de TypeScript.

Considera el primer parámetro;


<code-example path="displaying-data/src/app/hero.ts" header="src/app/hero.ts (id)" region="id"></code-example>

Esa breve sintaxis hace mucho:

* Declara un parametro del constructor y su tipo.
* Declara una propiedad pública del mismo nombre.
* Inicializa esa propiedad con el argumento correspondiente cuando se crea una instancia de la clase.

### Usando la clase Hero

Después de importar la clase `Hero`, la propiedad `AppComponent.heroes` puede retornar un arreglo _tipeado_ 
de objetos `Hero`.


<code-example path="displaying-data/src/app/app.component.3.ts" header="src/app/app.component.ts (heroes)" region="heroes"></code-example>



Ahora, actualiza la plantilla.
Hasta el momento esto muestra los `id` y `name` de héroe.
Cámbialo para mostrar solo la propiedad `name` de héroe.

<code-example path="displaying-data/src/app/app.component.3.ts" header="src/app/app.component.ts (template)" region="template"></code-example>

La visualización parece la misma, pero el código es mas claro.

{@a ngIf}

## Visualización condicional con NgIf

A veces una app necesita mostrar una vista o una porción de la vista solo bajo circunstancias específicas.

Cambiemos el ejemplo para mostrar un mensaje si hay más de tres héroes.

La directiva de Angular `ngIf` inserta o remueve un elemento basado en una condición _verdadera/falsa_.
Para verlo en acción, agrega el siguiente párrafo al final de la plantilla:


<code-example path="displaying-data/src/app/app.component.ts" header="src/app/app.component.ts (message)" region="message"></code-example>


<div class="alert is-important">

No olvides el asterisco inicial (\*) en `*ngIf`. Esto es parte escencial de la sintaxis.
Lee mas acerca de `ngIf` y `*` en la [sección ngIf](guide/built-in-directives#ngIf) de la página [Directivas integradas](guide/built-in-directives).

</div>


La expresión de plantilla dentro de las comillas dobles,
`*ngIf="heroes.length > 3"`, parece y se comporta como TypeScript.
Cuando la lista de héroes del componente tiene mas de tres elementos, Angular agrega el párrafo
al DOM y el mensaje aparece.
Si hay tres o ménos items, Angular omite el párrafo, entonces el mensaje no aparece.

Para mas información, visita [operadores de expresión de plantilla](guide/interpolation#template-expressions).


<div class="alert is-helpful">

Angular no está mostrando y escondiendo el mensaje. El elemento párrafo es agregado o removido del DOM. Esto mejora el desempeño, especialmente en grandes proyectos cuando
condicionalmente se incluyen o excluyen 
grandes porciones de HTML con muchos enlaces de datos.

</div>

Probemoslo. Como el arreglo tiene cuatro elementos, el mensaje debe aparecer.
Vuelve al <code>app.component.ts</code> y elimina o comenta uno de los elementos del arreglo de héroes.
El navegador debería actualizarse automáticamente y el mensaje debería desaparecer.

## En Resumen
Ahora sabes cómo usar:

* **Interpolación** con dobles llaves para mostrar la propiedad de un componente.
* **ngFor** para mostrar un arreglo de elementos.
* Una clase de TypeScript para dar forma a los **datos del modelo** para su componente y mostrar las propiedades de ese modelo.
* **ngIf** para mostrar condicionalmente un trozo de HTML basado en una expresión booleana.

Este es el código final:

<code-tabs>

  <code-pane header="src/app/app.component.ts" path="displaying-data/src/app/app.component.ts" region="final">

  </code-pane>

  <code-pane header="src/app/hero.ts" path="displaying-data/src/app/hero.ts">

  </code-pane>

  <code-pane header="src/app/app.module.ts" path="displaying-data/src/app/app.module.ts">

  </code-pane>

  <code-pane header="main.ts" path="displaying-data/src/main.ts">

  </code-pane>

</code-tabs>
