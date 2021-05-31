# Usando observables para pasar valores

Los observables proporcionan soporte al pase de mensajes entre partes de su aplicación.
Se usan frecuentemente en Angular y son la técnica recomendada para el manejo de eventos, programación asíncrona y manejar valores múltiples.

El patrón de observador es un patrón de diseño en el cual un objeto, llamado el *sujeto*, mantiene una lista de sus dependientes, llamados *observadores* y los notifica automáticamente de cambios de estado.
Este patrón es similar (pero no idéntico) al patrón de diseño editor/suscriptor.

Los observables son declarativos&mdash;esto es, se define una función para publicar valores, pero no se ejecuta hasta que un consumidor se suscribe a ésta.
El consumidor suscrito ahora recibe notificaciones hasta que la función termina, o hasta que anule su suscripción.

Un observable puede entregar múltiples valores de cualquier tipo&mdash;literales, mensajes o eventos dependiendo del contexto. La API para recibir valores es la misma sin importar que los valores sean entregados sincrónicamente o asincrónicamente. Ya que el observable se encarga de la lógica de inicio y desmontaje, su codigo en la aplicación sólo debe preocuparse sobre suscribirse para consumir valores, y al terminal, anular la suscripción. Sin importar que el flujo fuera presiones de teclas, una respuesta HTTP o un intervalo del temporizador, la interface para escuchar a los valores y dejar de escuchar es la misma.

Debido a estas ventajas, los observables son usados extensamente en Angular y se recomiendan para el desarrollo de aplicaciones también.

## Uso y términos básicos

Como un emisor, puede crear una instancia `Observable` que define una función *suscriber*. Esta es la función queque es ejecutada cuando un consumidor llama al método `subscribe()`. La función suscriptora define cómo obtener o generar valores o mensajes a ser publicados.

Para ejecutar el observable que ha creado y empezar a recibir notificaciones, llame al método `subscribe()`, pasando un *observer*. Este es un objeto JavaScript que define los manejadores para las notificaciones que recibe. La llamada `subscribe()` retorna un objeto`Subscription` que tiene un método `unsubscribe()`, que puede llamar para dejar de recibir notificaciones.

Encuentre al continuar un ejemplo que demuestra el modelo de uso básico demostrando cómo un observable puede ser usado para emitir actualizaciones de geolocalización.

<code-example class="no-auto-link" path="observables/src/geolocation.ts" header="Observar actualizaciones de geolocalización"></code-example>

## Definiendo observadores

Un manejador para recibir notificaciones de un observable implementa la interface `Observer`. Es un objeto que defina los métodos de retrollamada (callback) para manejar los tres tipos de notificaciones que un observador puede enviar:

| Tipo de notificación | Descripción                                                  |
| :------------------- | :----------------------------------------------------------- |
| `next`               | Requerido. Un manejador por cada valor entregado. Llamado cero o más veces después de que la ejecución empieza. |
| `error`              | Opcional. Un manejador para una notificación de error. Un error detiene la ejecución de la instancia observable. |
| `complete`           | Opcional. Un manejador de la notificación "ejecución completa". Los valores retrasados pueden seguir siendo entregados al siguiente manejador después de que la ejecución se completa. |

Un objeto observador puede definir cualquier combinación de estos manejadores, Si no provee un método para un tipo de notificación, el observador ignora notificaciones de ese tipo.

## Suscribiéndose

Una instancia `Observable` empieza publicando valores sólo cuando alguien se suscribe a ésta llamando al método `subscribe()` de la instancia, pasando un objeto observer para recibir las notificaciones.

<div class="alert is-helpful">

En orden para mostrar cómo funcionan las suscripciones, necesitamos crear un observable. Hay un constructor que se usa para crear nuevas instancias, pero a manera ilustrativa, podemos usar algunos médotos de la librería RxJS que crea observables simples de tipos usados frecuentemente:

  * `of(...items)`&mdash;Retorna una instancia `Observable` que entrega sincrónicamente los valores envíados como argumentos.
  * `from(iterable)`&mdash;Convierte sus argumentos a una instancia `Observable`. Éste método es usado comúnmente para convertir un array en un observable.

</div>

Acá hay un ejemplo creando y suscribiéndose a un observable simple, con son observador que muestra los mensajes recibidos en la consola:

<code-example
  path="observables/src/subscribing.ts"
  region="observer"
  header="Subscribirse usand observador"></code-example>

Alternativamente, el método `subscribe()` puede aceptar funciónes retrollamada en la línea para los manejadores de `next`, `error`, y `complete`. Por ejemplo, la siguiente llamada `subscribe()` es la misma a la que especifica el observador predefinido:

<code-example path="observables/src/subscribing.ts" region="sub_fn" header="Subscribirse con argumentos posicionales"></code-example>

En cualquier caso, un manejador `next`es necesario. Los manejadores `error` y `complete` son opcionales.

Tome en cuenta que la función `next()` puede recibir, por ejemplo, cadenas de mensajes, eventos de objetos, valores numéricos o estructuras, dependiendo del contexto. Como un término general, nos referimos a la data publicada por un observable como un *stream*. Cualquier tipo de valor puede representarse con un observable y los valores publicados como un stream.

## Creando observables

Use el constructor `Observable` para crear un stream observable de cualquier tipo. El constructor toma como su argumento la función de suscriptor para ejecutarse cuando el método `subscribe()` se ejecuta. Una función suscriptora recibe un objeto `Observer` y puede publicar valores al método `next()`.

Por ejemplo, para crear un observable equivalente al `of(1, 2, 3)` encontrado anteriormente, debería hacer algo como esto:

<code-example path="observables/src/creating.ts" region="subscriber" header="Crear observable con un constructor"></code-example>

Para tomar este ejemplo más lejos, podemos crear un observable que publica eventos. En este ejemplo, la función suscriptora se define en la línea.

<code-example path="observables/src/creating.ts" region="fromevent" header="Crear con función fromEvent personalizada"></code-example>

Ahora puede usar esta función para crear un observable que publica eventos de presiones de teclas:

<code-example path="observables/src/creating.ts" region="fromevent_use" header="Usando función fromEvent personalizada"></code-example>

## Multidifusión (Multicasting)

Un observable típico crea una ejecución nueva e independiente para cada observador suscrito. Cuando un observador se suscribe, el observable conecta un manejador de evento y entrega valores a ese observador. Cuando un segundo observador se suscribe, el observable conecta a un nuevo manejador de eventos y entrega valores a el segundo observador en una ejecución separada.

Algunas veces, en vez de comenzar una ejecución independiente ara cada suscriptor, quiere que cada suscripción obtenga los mismos valores&mdash;incluso si los valores ya empezaron e emitirse. Este sería el caso con algo como un observable de click en el objeto de documento.

*Multidifusión* es la práctica de difundir a una lista de múltiples suscriptores en una sola ejecución. Con un observable en multidifusión, no necesita registrar varios oyentes en el documento, prefiriendo reutilizar el primer oyente y mandar valores a cada suscriptor.

Cuando cree un observable debería determinar cómo quiere que ese observable se use y si quiere o no que se multidifusión en sus valores.

Veamos un ejemplo que cuenta de 1 a 3, con un retraso de un segundo después de que cada número es emitido.

<code-example path="observables/src/multicasting.ts" region="delay_sequence" header="Crer una secuencia retrasada"></code-example>

Tome en cuenta que si se suscribe dos veces, habrán dos streams separados, cada una emitiendo valores cada segundo. Se ve parecido a esto:

<code-example path="observables/src/multicasting.ts" region="subscribe_twice" header="Dos suscripciones"></code-example>

Cambiando al observable a multidifusión se vería similar a esta manera:

<code-example path="observables/src/multicasting.ts" region="multicast_sequence" header="Crear suscriptor multidifusión"></code-example>

<div class="alert is-helpful">
   Los observables multidifusión toman más tiempo empezando, pero pueden ser más útiles para cieras aplicaciones. Más adelante veremos herramientas que simiplifican el proceso de multidifusión, permitiéndole tomar cualquier observable y volverlo multidifusión.
</div>


## Manejo de errores

Como los observable producen valores de manera asíncrona, try/catch no va a detectar errores efectivamente. Los errores se manejan especificando una retrollamada `error` en el observador. Produciendo un error también causa que el observable limpie sus suscripciones y deje de producir valores. Un observable puede producir (haciendo retrollamada a `next`), o puede completar, con la retrollamda `complete` o `error`.

<code-example>
myObservable.subscribe({
  next(num) { console.log('Siguiente número: ' + num)},
  error(err) { console.log('Se recibió el error: ' + err)}
});
</code-example>

El manejo de errores (y especialmente la recuperación de un error) se cubre en mayor detalle  más adelante.
