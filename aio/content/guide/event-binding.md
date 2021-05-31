# Enlace de eventos `(event)`

El enlace de eventos le permite escuchar ciertos eventos, como
pulsaciones de teclas, movimientos del ratón, clics y toques.

<div class="alert is-helpful">

Consulte el <live-example> </live-example> para ver un ejemplo funcional que contiene los fragmentos de código de esta guía.
</div>

La sintaxis de enlace de eventos angular consiste en un **evento de destino** nombre
entre paréntesis a la izquierda de un signo igual y una cita
declaración de plantilla a la derecha.
El siguiente enlace de eventos escucha los eventos de clic del botón, llamando
el método `onSave()` del componente cada vez que se produce un clic:

<div class="lightbox">
  <img src='generated/images/guide/template-syntax/syntax-diagram.svg' alt="Syntax diagram">
</div>

## Evento objetivo

Como arriba, el objetivo es el evento de clic del botón.

<code-example path="event-binding/src/app/app.component.html" region="event-binding-1" header="src/app/app.component.html"></code-example>

Alternativamente, use el prefijo `on-`, conocido como la forma canónica:

<code-example path="event-binding/src/app/app.component.html" region="event-binding-2" header="src/app/app.component.html"></code-example>

Los eventos de elementos pueden ser los objetivos más comunes, pero Angular busca primero para ver si el nombre coincide con una propiedad de evento
de una directiva conocida, como en el siguiente ejemplo:

<code-example path="event-binding/src/app/app.component.html" region="custom-directive" header="src/app/app.component.html"></code-example>

Si el nombre no coincide con un evento de elemento o una propiedad de salida de una directiva conocida,
Angular informa de un error de "directiva desconocida".

## *$event* y declaraciones de manejo de eventos

En un enlace de eventos, Angular configura un controlador de eventos para el evento de destino.

Cuando se genera el evento, el controlador ejecuta la declaración de plantilla.
La declaración de plantilla generalmente involucra a un receptor, que realiza una acción
en respuesta al evento, como almacenar un valor del control HTML
en un modelo.

El enlace transmite información sobre el evento. Esta información puede incluir valores de datos como un objeto de evento, una cadena o un número llamado `$event`.

El evento de destino determina la forma del objeto `$event`.
Si el evento de destino es un evento de elemento DOM nativo, entonces `$event` es un
[Objeto de evento DOM](https://developer.mozilla.org/en-US/docs/Web/Events),
con propiedades como `target` y` target.value`.

Considere este ejemplo:

<code-example path="event-binding/src/app/app.component.html" region="event-binding-3" header="src/app/app.component.html"></code-example>

Este código establece la propiedad `<input>` `value` uniéndose a la propiedad `name`.
Para escuchar cambios en el valor, el código se enlaza con el `input`
evento del elemento `<input>`.
Cuando el usuario realiza cambios, se genera el evento `input` y el enlace se ejecuta
la declaración dentro de un contexto que incluye el objeto de evento DOM, `$event`.

Para actualizar la propiedad `name`, el texto modificado se recupera siguiendo la ruta` $event.target.value`.

Si el evento pertenece a una directiva&mdash;recuerda que los componentes
son directivas&mdash;`$event` tiene cualquier forma que produzca la directiva.

## Eventos personalizados con `EventEmitter`

Las directivas suelen generar eventos personalizados con un Angular [EventEmitter](api/core/EventEmitter).
La directiva crea un `EventEmitter` y lo expone como una propiedad.
La directiva llama `EventEmitter.emit(payload)` para disparar un evento, pasando una carga útil de mensaje, que puede ser cualquier cosa.
Las directivas principales escuchan el evento al vincularse a esta propiedad y acceder a la carga útil a través del objeto `$event`.
Considere una`ItemDetailComponent` que presenta información sobre el artículo y responde a las acciones del usuario.
Aunque el `ItemDetailComponent` tiene un botón de borrar, no sabe cómo borrar al héroe. Solo puede generar un evento que informe la solicitud de eliminación del usuario.

Aquí están los extractos pertinentes de ese `ItemDetailComponent`:

<code-example path="event-binding/src/app/item-detail/item-detail.component.html" header="src/app/item-detail/item-detail.component.html (template)" region="line-through"></code-example>

<code-example path="event-binding/src/app/item-detail/item-detail.component.ts" header="src/app/item-detail/item-detail.component.ts (deleteRequest)" region="deleteRequest"></code-example>

El componente define una propiedad `deleteRequest` que devuelve un `EventEmitter`.
Cuando el usuario hace clic en *eliminar*, el componente invoca el método `delete()`,
decirle al `EventEmitter` que emita un objeto `Item`.

Ahora imagine un componente principal de alojamiento que se une al evento `deleteRequest`
del `ItemDetailComponent`.

<code-example path="event-binding/src/app/app.component.html" header="src/app/app.component.html (event-binding-to-component)" region="event-binding-to-component"></code-example>

Cuando se activa el evento `deleteRequest`, Angular llama al componente principal
Método `deleteItem()`, pasando el *elemento a borrar* (emitido por `ItemDetail`)
en la variable `$event`.

## Las declaraciones de plantilla tienen efectos secundarios

Aunque [expresiones de plantilla](guide/interpolation#template-expressions) no debería tener [efectos secundarios](guide/property-binding#avoid-side-effects), la plantilla
las declaraciones suelen hacer. El método `deleteItem()` tiene
un efecto secundario: elimina un elemento.

Eliminar un elemento actualiza el modelo y, según su código, desencadena
otros cambios, incluidas las consultas y el guardado en un servidor remoto.
Estos cambios se propagan a través del sistema y finalmente se muestran en esta y otras vistas.
