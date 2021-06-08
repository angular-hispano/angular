# NgZone

Una zona es un contexto de ejecución que persiste en las tareas asíncronas. Puede considerarlo como  [almacenamiento local de hilos o subprocesos](http://en.wikipedia.org/wiki/Thread-local_storage) para máquinas virtuales JavaScript (VMs).
Esta guía describe cómo usar NgZone de Angular para detectar automáticamente cambios en el componente que actualiza el HTML.

## Fundamentos de la detección de cambios

Para entender los beneficios de `NgZone`, es importante tener una idea clara de qué es la detección de cambios y cómo funciona.

### Visualización y actualización de datos en Angular

En Angular, usted puede [mostrar datos](guide/displaying-data) enlanzando controles en una plantilla HTML a las propiedades de un componente Angular.

<code-example path="displaying-data/src/app/app.component.1.ts" header="src/app/app.component.ts"></code-example>

Además, puede vincular eventos del DOM a un método de un componente angular. En esos métodos, también puede actualizar una propiedad del componente Angular, que actualiza la data correspondiente que se muestra en la plantilla.

<code-example path="user-input/src/app/click-me.component.ts" region="click-me-component" header="src/app/click-me.component.ts"></code-example>

En los dos ejemplos arriba indicados, el código del componente actualiza solo la propiedad del componente. Sin embargo, el HTML también se actualiza automáticamente. Esta guía describe cómo y cuándo Angular renderiza el HTML en función de los datos del componente Angular.


### Detectar cambios con JavaScript puro 

Para aclarar cómo se detectan los cambios y se actualizan los valores, considere el siguiente código escrito en JavaScript puro.

```javascript
<html>
  <div id="dataDiv"></div>
  <button id="btn">actualizar Data</button>
  <canvas id="canvas"></canvas>
  <script>
    let value = 'Valor Inicial';
    // Renderizado inicial
    detectChange();

    function renderHTML() {
      document.getElementById('dataDiv').innerText = value;
    }

    function detectChange() {
      const currentValue = document.getElementById('dataDiv').innerText;
      if (currentValue !== value) {
        renderHTML();
      }
    }

    // Ejemplo 1: Actualizar el valor dentro del evento click  del handler del botón

    document.getElementById('btn').addEventListener('click', () => {
      // actualizar el valor
      value = 'actualizar el valor del botón';
      // llamada manual a la función detectChange 
      detectChange();
    });

    // Ejemplo 2: HTTP Petición
    const xhr = new XMLHttpRequest();
    xhr.addEventListener('load', function() {
      // obteniendo respuesta desde el servidor
      value = this.responseText;
      // llamada a la función detectChange manualmente
      detectChange();
    });
    xhr.open('GET', serverUrl);
    xhr.send();

    // Example 3: setTimeout
    setTimeout(() => {
      // actualizar el valor dentro del setTimeout callback
      value = 'timeout actualiza el valor';
      // llamada a la función detectChange manualmente
      detectChange();
    }, 100);

      // Ejemplo 4: Promise.then
    Promise.resolve('promesa devuelve un valor').then(v => {
      // Actualizar el valor dentro de thenCallback de la Promesa
      value = v;
      // llamada a la función detectChange manualmente
      detectChange();
    }, 100);

    // Ejemplo 5: algunas otras APIs asíncronas
    document.getElementById('canvas').toBlob(blob => {
      // actualizar el valor cuando blob data es creado desde el canvas
      value = `valor actualizado por el canvas, el tamaño es ${blob.size}`;
      // llamada a la función detectChange manualmente
      detectChange();
    });
  </script>
</html>
```

Después de actualizar los datos, debe llamar a la función `detectChange()` manualmente para validar si los datos cambiaron. Si los datos cambiaron, renderiza el HTML para reflejar los datos actualizados.

En Angular, este paso es innecesario. Cada vez que actualiza los datos, su HTML se actualiza automáticamente.

### Cuando las aplicaciones actualizan HTML

Para entender cómo funciona la detección de cambios, primero considere cuándo la aplicación necesita actualizar el HTML. Generalmente, las actualizaciones se producen por una de las siguientes razones:

1. Inicialización de componentes. Por ejemplo, cuando cargamos una aplicación Angular, Angular carga el componente de inicio y activa [ApplicationRef.tick()](api/core/ApplicationRef#tick) para llamar a la detección de cambios y visualizar el renderizado. Justo como en el ejemplo de [mostrando datos](guide/displaying-data), el `AppComponent` es el componente de carga. Este componente tiene las propiedades `title` y `myHero`, las cuales la aplicación renderiza en el HTML.

2. Detector de eventos. El detector de eventos del DOM puede actualizar los datos en un componente angular y también activar la detección de cambios, como en el siguiente ejemplo.

<code-example path="user-input/src/app/click-me.component.ts" region="click-me-component" header="src/app/click-me.component.ts"></code-example>

3. Solicitud de datos HTTP. También puede obtener datos de un servidor a través de una solicitud HTTP. Por ejemplo:

```typescript
@Component({
  selector: 'app-root',
  template: '<div>{{data}}</div>';
})
export class AppComponent implements OnInit {
  data = 'valor inicial';
  serverUrl = 'SERVER_URL';
  constructor(private httpClient: HttpClient) {}

  ngOnInit() {
    this.httpClient.get(this.serverUrl).subscribe(response => {
      // El usuario no necesita activar la detección de cambios manualmente
      this.data = response.data;
    });
  }
}
```

4. MacroTasks, como setTimeout () o setInterval (). También puede actualizar los datos en la función `callback` de una macroTask como setTimeout (). Por ejemplo:

```typescript
@Component({
  selector: 'app-root',
  template: '<div>{{data}}</div>';
})
export class AppComponent implements OnInit {
  data = 'valor inicial';

  ngOnInit() {
    setTimeout(() => {
      // El usuario no necesita activar la detección de cambios manualmente
      this.data = 'valor actualizado';
    });
  }
}
```

5. MicroTasks, como Promise.then (). Otras APIs asíncronas devuelven una promesa como objeto ( tal como `fetch`), por lo que la función callback `then()` también puede actualizar los datos. Por ejemplo:

```typescript
@Component({
  selector: 'app-root',
  template: '<div>{{data}}</div>';
})
export class AppComponent implements OnInit {
  data = 'valor inicial';

  ngOnInit() {
    Promise.resolve(1).then(v => {
      // El usuario no necesita activar la detección de cambios manualmente
      this.data = v;
    });
  }
}
```

6. Otras operaciones asíncronas. Además de `addEventListener()`, `setTimeout()` y `Promise.then()`, existen otras operaciones que pueden actualizar los datos de manera asíncrona. Algunos ejemplos incluyen a `WebSocket.onmessage()` y `Canvas.toBlob()`.

La lista previa contiene los escenarios más comunes en los que la aplicación podría cambiar los datos. Angular ejecuta la detección de cambios siempre que detecta que los datos podrían haber cambiado. El resultado de la detección de cambios es que el DOM se actualiza con nuevos datos. Angular detecta los cambios de diferentes maneras. En la inicialización de componentes, Angular llama la detección de cambios explícitamente. Para [operaciones asíncronas](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous), Angular usa una zona para detectar cambios en lugares dónde los datos podrían haber mutado y acciona la detección de cambios automáticamente.


## Zonas y contextos de ejecución

Una zona proporciona un contexto de ejecución que persiste a través de las tareas asíncronas. [Contexto de Ejecución](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) es un concepto abstracto que contiene información sobre el entorno en el cual se está ejecutando el código actual. Considere el siguiente ejemplo:

```javascript
const callback = function() {
  console.log('Contexto de setTimeout callback', this);
}

const ctx1 = { name: 'ctx1' };
const ctx2 = { name: 'ctx2' };

const func = function() {
  console.log('caller del contexto es', this);
  setTimeout(callback);
}

func.apply(ctx1);
func.apply(ctx2);
```

El valor de `this` en el callback de `setTimeout()` podría diferir dependiendo de cuándo se llame a `setTimeout()`. 

Por lo tanto, puede perder el contexto en operaciones asíncronas.

Una zona brinda un nuevo contexto de zona distinto de `this`, el contexto de zona que persiste en las operaciones asíncronas.

En el siguiente ejemplo, el nuevo contexto de zona es llamado `zoneThis`.

```javascript
zone.run(() => {
  // ahora tu estas en una zona
  expect(zoneThis).toBe(zone);
  setTimeout(function() {
    // El contexto zoneThis será la misma zona
    // Cuando el setTimeout está agendado o programado
    expect(zoneThis).toBe(zone);
  });
});
```

Este nuevo contexto, `zoneThis`, puede ser recuperado desde el callback de la función `setTimeout()`, y este contexto es el mismo programado en `setTimeout()`. Para obtener el contexto, puede llamar a [`Zone.current`](https://github.com/angular/angular/blob/master/packages/zone.js/lib/zone.ts)


## Zonas y hooks del ciclo de vida asíncronos 


Zone.js puede crear contextos que persisten en las operaciones asíncronas, así como proporcionar los hooks del ciclo de vida para operaciones asíncronas.

```javascript
const zone = Zone.current.fork({
  name: 'zone',
  onScheduleTask: function(delegate, curr, target, task) {
    console.log('nueva tarea es programada:', task.type, task.source);
    return delegate.scheduleTask(target, task);
  },
  onInvokeTask: function(delegate, curr, target, task, applyThis, applyArgs) {
    console.log('la tarea será invocada:', task.type, task.source);
    return delegate.invokeTask(target, task, applyThis, applyArgs);
  },
  onHasTask: function(delegate, curr, target, hasTaskState) {
    console.log('El estado de la tarea cambió en la zona:', hasTaskState);
    return delegate.hasTask(target, hasTaskState);
  },
  onInvoke: function(delegate, curr, target, callback, applyThis, applyArgs) {
    console.log('el callback será invocado:', callback);
    return delegate.invoke(target, callback, applyThis, applyArgs);
  }
});
zone.run(() => {
  setTimeout(() => {
    console.log('timeout callback es invocado.');
  });
});
```

El ejemplo anterior crea una zona con varios hooks.

Los `onXXXTask` hooks se activan cuando cambia el estado de la tarea. 
El concepto de una *Tarea de Zona* es muy similar al concepto de tarea de máquina virtual (VM) de JavaScript:
- `macroTask`: tal como `setTimeout()`
- `microTask`: tal como `Promise.then()`
- `eventTask`: tal como `element.addEventListener()`

Estos hooks se activan bajo las siguientes circunstancias:

- `onScheduleTask`: se activa cuando una nueva tarea asíncrona es programada, como cuando se llama a `setTimeout()`.
- `onInvokeTask`: se activa cuando una tarea asíncrona está cerca de ejecutarse, como cuando el callback de `setTimeout()` está a punto de ejecutarse.
- `onHasTask`: se activa cuando el estado de un tipo de tarea dentro de una zona cambia de estable a inestable o de inestable a estable. Un estado de "estable" significa que no hay tareas dentro de la zona, mientras que "inestable" significa que hay una nueva tarea programada en la zona.
- `onInvoke`: se activa cuando se va a ejecutar una función síncrona en la zona.

Con estos hooks, `Zone` puede monitorear el estado de todas las operaciones sincrónicas y asíncronas dentro de una zona.

El ejemplo anterior devuelve el siguiente resultado:

```
the callback will be invoked: () => {
  setTimeout(() => {
    console.log('timeout callback es invocado.');
  });
}
new task is scheduled: macroTask setTimeout
task state changed in the zone: { microTask: false,
  macroTask: true,
  eventTask: false,
  change: 'macroTask' }
task will be invoked macroTask: setTimeout
timeout callback is invoked.
task state changed in the zone: { microTask: false,
  macroTask: false,
  eventTask: false,
  change: 'macroTask' }
```

Todas las funciones de `Zone` son proporcionadas por una librería llamada [Zone.js](https://github.com/angular/angular/tree/master/packages/zone.js/README.md). Esta librería implementa esas funcionalidades interceptando API asíncronas a través de `monkey patching`. El `monkey patching` es una técnica para agregar o modificar el comportamiento predeterminado de una función en tiempo de ejecución sin cambiar el código fuente.

## NgZone

Si bien Zone.js puede monitorear todos los estados de las operaciones sincrónicas y asíncronas, Angular también proporciona un servicio llamado NgZone. 

Este servicio crea una zona denominada `angular` para activar automáticamente la detección de cambios cuando se cumplen las siguientes condiciones:

1. Cuando es ejecutada una función sincrona o asíncrona
1. Cuando no hay una  `microTask` programada.

### NgZone `run()` y `runOutsideOfAngular()`

`Zone` maneja la mayoría de las APIs asíncronas, como `setTimeout()`, `Promise.then()` y `addEventListener()`. Para obtener la lista completa, consulte el documento [Documento del Módulo Zone](https://github.com/angular/angular/blob/master/packages/zone.js/MODULE.md). Por lo tanto, en esas API asíncronas, no necesitas activar la detección de cambios manualmente.

Todavía hay algunas APIs de terceros que Zone no maneja. En esos casos, el servicio `NgZone` proporciona un método [`run()`](api/core/NgZone#run) que le permite ejecutar una función dentro de la zona angular. Esta función, y todas las operaciones asíncronas en esa función, activan la detección de cambios automáticamente en el momento correcto.

```typescript
export class AppComponent implements OnInit {
  constructor(private ngZone: NgZone) {}
  ngOnInit() {
    // Zone no maneja la nueva API asíncrona, por lo que necesitas
    // usar ngZone.run() para realizar las operaciones asíncronas en la zona angular
    // y activar la detección de cambios automáticamente.
    this.ngZone.run(() => {
      someNewAsyncAPI(() => {
        // actualizar los datos del componente
      });
    });
  }
}
```

Por defecto, todas las operaciones asíncronas están dentro de la zona angular, lo que activa la detección de cambios automáticamente. Otro caso común es cuando no quiere activar la detección de cambios. En esa situación, puede usar otro método `NgZone`: [`runOutsideAngular()`](api/core/NgZone#runoutsideangular).

```typescript
export class AppComponent implements OnInit {
  constructor(private ngZone: NgZone) {}
  ngOnInit() {
    // Sabes que no se actualizarán datos,
    // por lo que tu no quieres activar la detección de cambios en este
    // operación específica. En vez de eso, llama a ngZone.runOutsideAngular()
    this.ngZone.runOutsideAngular(() => {
      setTimeout(() => {
        // actualizar los datos del componente
        // pero no activa la detección de cambios.
      });
    });
  }
}
```

### Configurar Zone.js

Para que Zone.js esté disponible en Angular, necesitas importar el paquete `zone.js`. 
Si está utilizando la CLI de Angular, este paso se realiza automáticamente y verá la siguiente línea en `src/polyfills.ts`:

```typescript
/***************************************************************************************************
 * Zone JS es necesario de forma predeterminada para Angular.
 */
import 'zone.js/dist/zone';  // Incluida con Angular CLI.
```

Antes de importar el paquete `zone.js`, puede establecer las siguientes configuraciones:

- Puede deshabilitar algunos API monkey patching asincrónicos para un mejor rendimiento. Por ejemplo, puede deshabilitar el `requestAnimationFrame()` monkey patch, por lo que el callback de `requestAnimationFrame()` no activará la detección de cambios. Esto es útil si, en su aplicación, el callback de `requestAnimationFrame()` no actualizará ningún dato.

- Puede especificar que ciertos eventos DOM no se ejecuten dentro de la zona angular; por ejemplo, para evitar que un evento del mouse `mousemove` o `scroll` active la detección de cambios.

Existen algunas otras configuraciones que puede cambiar. Para hacer estos cambios, necesita crear un archivo `zone-flags.ts`, como el siguiente.

```typescript
// Deshabilitar patching requestAnimationFrame
(window as any).__Zone_disable_requestAnimationFrame = true;

// Deshabilitar patching para eventos específicos
(window as any).__zone_symbol__UNPATCHED_EVENTS = ['scroll', 'mousemove'];
```

Luego, importe `zone-flags` antes de importar  `zone.js` en el `polyfills.ts`:

```typescript
/***************************************************************************************************
 * Zone JS es requerido de forma predeterminada para Angular.
 */
import `./zone-flags`;
import 'zone.js/dist/zone';  // Incluida con Angular CLI.
```

Para más información sobre lo que puede configurar, consulte el documento [Zone.js](https://github.com/angular/angular/tree/master/packages/zone.js).

### NoopZone

`Zone` ayuda a Angular a saber cuándo activar la detección de cambios y permite que los desarrolladores se enfoquen en el desarrollo de la aplicación. 
De forma predeterminada, `Zone` se carga y funciona sin configuración adicional. Sin embargo, no es necesario que use `Zone` para hacer que Angular funcione. En su lugar, puede optar por activar la detección de cambios por su cuenta.

<div class="alert is-helpful">

<h4>Deshabilitando <code>Zone</code></h4>

**Si deshabilita `Zone`, necesitará activar todas las detecciones de cambios en el momento correcto usted mismo, lo que requiere un conocimiento importante de la detección de cambios.**

</div>

Para eliminar Zone.js, realice los siguientes cambios.

1. Elimine el archivo `zone.js` importado en `polyfills.ts`:

  ```typescript
  /***************************************************************************************************
   * Zone JS es requerido de forma predeterminada para Angular.
   */
  // import 'zone.js/dist/zone';  // Incluida con Angular CLI.
  ```

2. Carga de Angular con la zona `noop` en `src/main.ts`:

  ```typescript
  platformBrowserDynamic().bootstrapModule(AppModule, { ngZone: 'noop' })
    .catch(err => console.error(err));
  ```
