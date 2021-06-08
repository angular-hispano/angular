# Entrada del usuario

Acciones del usuario como hacer clic a un enlace, presionando un botón, e ingresando
texto genera eventos al DOM.
Esta página explica cómo vincular esos eventos a los controladores de eventos de componentes utilizando
la sintaxis de enlace de eventos de Angular.

Run the <live-example></live-example>.

{@a binding-to-user-input-events}
## Enlace a los eventos de entrada del usuario

Usted puede usar [Enlace de eventos Angular](guide/event-binding)
y responder a algún [Evento del DOM](https://developer.mozilla.org/en-US/docs/Web/Events).
Muchos eventos del DOM son activados por la entrada de usuario. El enlace a estos eventos proporciona una forma de 
obtener información del usuario.

Para enlazar el evento del DOM, envuelva el nombre del evento del DOM entre paréntesis y asígnele una
[Declaración de plantilla](guide/template-statements) entre comillas.

El siguiente ejemplo muestra un enlace de eventos que implementa el controlador de clic:

<code-example path="user-input/src/app/click-me.component.ts" region="click-me-button" header="src/app/click-me.component.ts"></code-example>

{@a click}

El `(click)` a la izquierda del signo igual identifica el evento de clic del botón como el **objetivo del enlace**.
El texto entre comillas a la derecha del signo igual
es la **declaración de plantilla**, que responde
al evento de clic llamando al método del componente `onClickMe`.

Cuando escriba un enlace, tenga en cuenta las declaraciones de platillas **contexto de ejecución**.
Los identificadores en una declaración de plantilla pertenecen a un objeto de contexto específico,
generalmente el componente de Angular es quien controla la plantilla.
El ejemplo anterior muestra una sola línea de HTML, pero ese HTML pertenece a un componente más grande:


<code-example path="user-input/src/app/click-me.component.ts" region="click-me-component" header="src/app/click-me.component.ts"></code-example>



Cuando el usuario hace clic en el botón, Angular llama al método `onClickMe` desde `ClickMeComponent`.



## Obtener la entrada del usuario del objeto de $evento
Los eventos del DOM llevan una carga de información que puede ser útil para el componente.
Esta sección muestra cómo enlaza al evento `keyup` de un cuadro de entrada para obtener la entrada del usuario después de cada pulsación de tecla.

El siguiente código escucha el evento `keyup` y pasa toda la carga del evento (`$event`) al controlador de eventos del componente.

<code-example path="user-input/src/app/keyup.components.ts" region="key-up-component-1-template" header="src/app/keyup.components.ts (template v.1)"></code-example>



Cuando un usuario presiona y suelta una tecla, se produce el evento `keyup` y Angular proporciona el correspondiente
objeto de evento del DOM en la variable `$event` este código pasa como parámetro al método `onKey()` del componente.

<code-example path="user-input/src/app/keyup.components.ts" region="key-up-component-1-class-no-type" header="src/app/keyup.components.ts (class v.1)"></code-example>



Las propiedades de un objeto `$event` varían según el tipo de evento del DOM. Por ejemplo,
un evento de mouse incluye información diferente a la de un evento de edición de cuadro de entrada.

Todos los [objetos de eventos del DOM estándar](https://developer.mozilla.org/en-US/docs/Web/API/Event)
tienen una propiedad `target`, una referencia al elemento que generó el evento.
En este caso, `target` se refiere a [`<input>` element](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement) and
`event.target.value` devuelve el contenido actual de ese elemento.

Después de cada llamada, el método `onKey()` agrega el contenido del valor del cuadro de entrada a la lista
en la propiedad `values` del componente, seguida de un carácter separador (|).
La [interpolación](guide/interpolation)
muestra los cambios acumulados en el cuadro de entrada de la propiedad `values`.

Suponga que el usuario ingresa las letras "abc" y luego retrocede para eliminarlas una por una.
Esto es lo que muestra la interfaz de usuario:

<code-example>
  a | ab | abc | ab | a | |
</code-example>



<div class="lightbox">
  <img src='generated/images/guide/user-input/keyup1-anim.gif' alt="key up 1">
</div>



<div class="alert is-helpful">



Alternativamente, puede acumular las keys individuales sustituyendo `event.key`
por `event.target.value` en cuyo caso la misma entrada del usuario produciría:

<code-example>
  a | b | c | backspace | backspace | backspace |

</code-example>



</div>



{@a keyup1}


### Escribir el _$evento_

El ejemplo anterior arroja el `$event` como un tipo `any`.
Eso simplifica el código con un costo.
No hay información de ese tipo
que podría revelar las propiedades del objeto del evento y evitar errores tontos.

El siguiente ejemplo reescribe el método con tipos:

<code-example path="user-input/src/app/keyup.components.ts" region="key-up-component-1-class" header="src/app/keyup.components.ts (class v.1 - typed )"></code-example>



El `$evento` es ahora un específico `KeyboardEvent`.
No todos los elementos tienen una propiedad `value` por lo que lanza `target` a un elemento de entrada.
El método `OnKey` expresa más claramente lo que espera de la plantilla y cómo interpreta el evento.

### Pasando el _$evento_ es una práctica dudosa
Escribir el objeto de evento revela una objeción significativa a pasar todo el evento del DOM al método:
el componente tiene demasiado conocimiento de los detalles de la plantilla.
No puede extraer información sin saber más de lo que debería sobre la implementación de HTML.
Que rompe la separación de importancia entre la plantilla (_lo que ve el usuario_)
y el componente (_cómo la aplicación procesa los datos del usuario_).

La siguiente sección muestra cómo usar las variables de referencia de la plantilla para abordar este problema.



## Obtener la entrada del usuario de una variable de referencia de plantilla
Hay otra forma de obtener los datos del usuario: use Angular
[**variables de referencia de plantilla**](guide/template-reference-variables).
Estas variables proporcionan acceso directo a un elemento desde dentro de la plantilla.
Para declarar una variable de referencia de plantilla, preceda el identificador con un carácter de almohadilla (#).

El siguiente ejemplo usa una variable de referencia de plantilla.
Para implementar un loopback de pulsaciones de teclas en una plantilla simple.

<code-example path="user-input/src/app/loop-back.component.ts" region="loop-back-component" header="src/app/loop-back.component.ts"></code-example>



La variable de referencia de plantilla llamada `box`, declarada en el elemento `<input>`,
se refiere al elemento `<input>` elemento en si.
El código usa la variable `box` para obtener el `value` del elemento de entrada y mostrarlo
con interpolación entre etiquetas `<p>`.

La plantilla es completamente autónoma. No se une al componente,
y el componente no hace nada.

Escriba algo en el cuadro de entrada y observe cómo se actualiza la pantalla con cada pulsación de tecla.


<div class="lightbox">
  <img src='generated/images/guide/user-input/keyup-loop-back-anim.gif' alt="loop back">
</div>



<div class="alert is-helpful">



**Esto no funcionará en absoluto a menos que se enlace a un evento**.

Angular actualiza los enlaces (y por lo tanto la pantalla)
solo si la aplicación hace algo en respuesta a eventos asincrónicos, como pulsaciones de teclas.
Este código de ejemplo enlaza el evento `keyup`
al número 0, la declaración de plantilla más corta posible.
Si bien la declaración no hace nada útil,
satisface el requisito de Angular para que Angular actualice la pantalla.

</div>



Es más fácil llegar al cuadro de entrada con la referencia de la plantilla
variable que pasa por el objeto `$evento`. Aquí hay una reescritura de la anterior.
Ejemplo de `keyup` que usa una variable de referencia de plantilla para obtener la entrada del usuario.

<code-example path="user-input/src/app/keyup.components.ts" region="key-up-component-2" header="src/app/keyup.components.ts (v2)"></code-example>



Un buen aspecto de este enfoque es que el componente obtiene valores de datos limpios de la vista.
Ya no requiere conocimiento del `$evento` y su estructura.
{@a key-event}


## Filtrado de eventos clave (con `key.enter`)
El controlador de eventos `(keyup)` escucha *cada pulsación de tecla*.
A veces, solo importa la tecla _Enter_, porque indica que el usuario ha terminado de escribir.
Una forma de reducir el ruido sería examinar cada `$event.keyCode` y actuar solo cuando la tecla sea _Enter_.

Hay una manera más fácil: enlazarse con el pseudo-evento `keyup.enter` de Angular.
Entonces Angular llama al controlador de eventos solo cuando el usuario presiona _Enter_.

<code-example path="user-input/src/app/keyup.components.ts" region="key-up-component-3" header="src/app/keyup.components.ts (v3)"></code-example>



Así es como funciona.

<div class="lightbox">
  <img src='generated/images/guide/user-input/keyup3-anim.gif' alt="key up 3">
</div>




## En desenfoque

En el ejemplo anterior, el estado actual del cuadro de entrada
se pierde si el usuario aleja el mouse y hace clic en otra parte de la página
sin presionar primero _Enter_.
La propiedad `value` del componente se actualiza solo cuando el usuario presiona _Enter_.

Para solucionar este problema, escuche ambas teclas _Enter_ y el evento _blur_.


<code-example path="user-input/src/app/keyup.components.ts" region="key-up-component-4" header="src/app/keyup.components.ts (v4)"></code-example>




## Ponlo todo junto
La página anterior mostró cómo [mostrar datos](guide/displaying-data).
Esta página demostró técnicas de enlace de eventos.

Ahora, ponlo todo junto en una micro-aplicación
que puede mostrar una lista de héroes y agregar nuevos héroes a la lista.
El usuario puede agregar un héroe escribiendo el nombre del héroe en el cuadro de entrada y
haciendo clic en **Add**.


<div class="lightbox">
  <img src='generated/images/guide/user-input/little-tour-anim.gif' alt="Little Tour of Heroes">
</div>



A continuación se muestra el componente "Pequeño Tour de Heroes".


<code-example path="user-input/src/app/little-tour.component.ts" region="little-tour" header="src/app/little-tour.component.ts"></code-example>



### Observaciones

* **Utilice variables de plantilla para hacer referencia a elementos** &mdash;
la variable de plantilla `newHero` se refiere al elemento `<input>`.
Puede hacer referencia a `newHero` desde cualquier hermano o hijo del elemento `<input>`.

* **Pasar valores, no elementos** &mdash;
En lugar de pasar el `newHero` al método `addHero` del componente,
obtenga el valor del cuadro de entrada y pase *ese* a `addHero`.

* **Mantenga las declaraciones de plantilla simples** &mdash;
El evento `(blur)` está vinculado a dos declaraciones de JavaScript.
La primera declaración llama a `addHero`. La segunda declaración, `newHero.value=''`,
borra el cuadro de entrada después de que se agregue un nuevo héroe a la lista.



## Código fuente

A continuación se muestra todo el código discutido en esta página.

<code-tabs>

  <code-pane header="click-me.component.ts" path="user-input/src/app/click-me.component.ts">

  </code-pane>

  <code-pane header="keyup.components.ts" path="user-input/src/app/keyup.components.ts">

  </code-pane>

  <code-pane header="loop-back.component.ts" path="user-input/src/app/loop-back.component.ts">

  </code-pane>

  <code-pane header="little-tour.component.ts" path="user-input/src/app/little-tour.component.ts">

  </code-pane>

</code-tabs>


Angular también admite detectores de eventos pasivos. Por ejemplo, puede utilizar los siguientes pasos para convertir el evento de scroll en pasivo.

1. Cree un archivo `zone-flags.ts` en el directorio `src`.
2. Agregue la siguiente línea a este archivo.

```
(window as any)['__zone_symbol__PASSIVE_EVENTS'] = ['scroll'];
```

3. En el archivo `src/polyfills.ts`, antes de importar zone.js, importe los `zone-flags` recién creados.

```
import './zone-flags';
import 'zone.js/dist/zone';  // Included with Angular CLI.
```

Después de esos pasos, si agrega detectores de eventos para el evento `scroll`, los detectores serán `passive`.

## Resumen

Ha dominado las primitivas básicas para responder a las entradas y los gestos del usuario..

Estas técnicas son útiles para demostraciones a pequeña escala, pero
rápidamente se vuelven verbosos y torpes cuando manejan grandes cantidades de información de usuario.
El enlace de datos bidireccional es una forma más elegante y compacta de moverse
valores entre los campos de entrada de datos y las propiedades del modelo.
La página siguiente, `Formularios`, explica cómo escribir
enlaces bidireccionales con `NgModel`.
