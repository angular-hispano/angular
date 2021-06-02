# Observables en Angular

Angular utiliza observables para manejar operaciones asincronas. Por ejemplo:

- Puedes definir [custom events](guide/event-binding#custom-events-with-eventemitter) que envían la informacion de un componente hijo a un componente padre.
- El módulo HTTP usa observables para manejar la peticiones y respuestas AJAX.
- Los módulos Router (Enrutador) y Forms usa observables para escuchar y responder a los eventos del usuario.

## Trasmitir informacion entre componentes

Angular provee una clase llamada `EventEmitter` la cual se utiliza para emitir valores desde un componente a través del [`@Output()` decorator](guide/inputs-outputs#how-to-use-output).
`EventEmitter` extiende el [RxJS `Subject`](https://rxjs.dev/api/index/class/Subject), agregandole el metodo `emit()` para que pueda enviar valores arbitrarios.
Cuando llamamos a `emit()`, se pasa el valor emitido al metodo `next()` de cualquier observable subscrito.

Un buen ejemplo de uso puede encontrarse en la documentación de [EventEmitter](api/core/EventEmitter). Aqui podemos encontrar un ejemplo de componente que escucha eventos de open y close:

`<app-zippy (open)="onOpen($event)" (close)="onClose($event)"></app-zippy>`

La definicion del componente es:

<code-example path="observables-in-angular/src/main.ts" header="EventEmitter" region="eventemitter"></code-example>

## HTTP

Angular `HttpClient` retorna un observable cuando un metodo HTTP es llamado. Por ejemplo `http.get(‘/api’)` retorna un observable. Esto proporciona varias ventajas sobre las promesas basadas en HTTP APIs:

- Los Observables no mutan la respuesta del servidor (como puede ocurrir encadenandos llamadas a `.then()` en las promesas). En su lugar, se usaran una serie de operadores para transformar la respuesta segun necesitemos.
- Las peticiones HTTP pueden ser cancelables mediante el metodo `unsubscribe()`.
- Se puede obtener informacion acerca del progreso de la peticion.
- Las peticiones fallidas se pueden reintentar fácilmente.

## Async pipe

El [AsyncPipe](api/common/AsyncPipe) se subscribe a un observable o promesa y devuelve el ultimo valor que ha sido emitido. Cuando se emite un nuevo valor, el pipe marca el componente, para verificar si hay cambios.

El siguiente ejemplo combina un observable de tiempo con la vista del componente. El observable actualiza continuamente la vista con la hora actual.

<code-example path="observables-in-angular/src/main.ts" header="Using async pipe" region="pipe"></code-example>

## Router

[`Router.events`](api/router/Router#events) proporciona eventos como observables. Puedes usar el operador `filter()` desde RxJS y obtener los eventos que sean de tu interes, y subscribirte a ellos con el fin de tomar decisiones basadas en la secuencia de eventos en el proceso de navegación. Aquí hay un ejemplo:

<code-example path="observables-in-angular/src/main.ts" header="Router events" region="router"></code-example>

El [ActivatedRoute](api/router/ActivatedRoute) es un servicio de router inyectado que utiliza observables para obtener información acerca de una ruta y parámetros. Por ejemplo, "ActivatedRoute.url" contiene un observable que informa la ruta o las rutas. He aquí un ejemplo:

<code-example path="observables-in-angular/src/main.ts" header="ActivatedRoute" region="activated_route"></code-example>

## Formularios Reactivos

Los formularios reactivos tienen propiedades que usan observables para monitorear los valores de los FormControl. El [`FormControl`](api/forms/FormControl) tiene las propiedaded `valueChanges` y `statusChanges` que contienen observables los cuales generan eventos de cambio. Suscribirse a una propiedad observable de form-control es una forma de activar la lógica de la aplicación dentro de la clase del componente. Por ejemplo:

<code-example path="observables-in-angular/src/main.ts" header="Reactive forms" region="forms"></code-example>
