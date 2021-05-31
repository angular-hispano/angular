# Service worker en producción

Esta página es una referencia para el despliegue y soporte de aplicaciones en producción que utilizan el service worker de Angular. Se explica como el service worker de Angular se ajusta a un entorno de producción muy grande, como se comporta bajo diversas condiciones y recursos disponibles, y su sistema a prueba de fallos.

#### Prerequisitos

Conocimiento básico acerca de lo siguiente: 
* [Comunicación en Service Worker](guide/service-worker-communications).

<hr />

## Service worker y el almacenamiento de recursos de aplicaciones en caché

Conceptualmente, puedes imaginar el service worker de Angular como un forward caché o un CDN perimetral que está instalado en el navegador del usuario final. El service worker funciona para satisfacer las peticiones de recursos o datos hechas por la aplicación en Angular desde un caché local, sin necesidad de esperar a la red. Como cualquier caché, tiene reglas sobre como caduca y se actualiza el contenido.

{@a versions}

### Versiones de aplicación

En el contexto de un service worker de Angular, una "versión" es una colección de recursos que representan una compilación (build) específica de la aplicación. Cada vez que se despliega una nueva compilación, el service worker trata esa compilación como una nueva versión de la aplicación. Esto pasa incluso si solo se actualiza un único archivo. En cualquier momento, el service worker puede tener múltiples versiones de la aplicación en caché y servirlas simultáneamente. Para obtener más información, consulta [Pestañas de aplicación](guide/service-worker-devops#tabs) que se encuentra más adelante. 

Para preservar la integridad de una aplicación, el service worker de Angular agrupa todos los archivos en una versión. Los archivos agrupados en una versión generalmente incluyen archivos HTML, JS y CSS. Dicha agrupación es esencial para preservar la integridad porque con frecuencia los archivos HTML, JS y CSS se vinculan entre sí y dependen de un contenido específico. Por ejemplo, un archivo `index.html` puede incluir una etiqueta `<script>` que hace referencia a `bundle.js`  y este intenta llamar a una función `startApp()` que se encuentra dentro del mismo script. Cada vez que se sirve `index.html` también se debe servir el `bundle.js` correspondiente. Por ejemplo, imagina que la función `startApp()` es renombrada a `runApp()` en ambos archivos. En este escenario no es válido servir el archivo `index.html` antiguo, que llama a la función `startApp()` junto con el nuevo paquete, donde se definió `runApp()`.

Esta integridad de archivos es especialmente importante cuando la carga de los módulos se realiza mediante lazy loading.
Un paquete de JS puede hacer referencia a muchos trozos pequeños de la aplicación (chunk), y los nombres de archivos chunk son exclusivos de cada compilación de la aplicación. Si una aplicación que se está ejecutando en la versión `X` intenta cargar un chunk, pero el servidor ya ha actualizado a la versión  `X + 1` la operación de lazy loading podría fallar.

El identificador de versión de la aplicación está determinado por el contenido de todos los recursos, y cambia si alguno de ellos cambia. En la práctica, la versión está determinada por el contenido del archivo `ngsw.json`, que incluye hashes para todo el contenido conocido. Si alguno de los archivos en caché cambia, el hash del archivo cambiará en `ngsw.json`, haciendo que el service worker de Angular trate el conjunto activo de archivos como una nueva versión.

Con este comportamiento de control de versiones para el service worker de Angular, un servidor de aplicaciones puede asegurarse de que la aplicación siempre tenga un conjunto de archivos consistentes. 

#### Búsqueda de actualizaciones

Cada vez que el usuario abre o refresca la aplicación, el service worker de Angular verifica si existen actualizaciones en la aplicación buscando cambios en el manifiesto `ngsw.json`. Si se encuentra alguna actualización, esta se descarga y se almacena en caché automáticamente, y se servirá la próxima vez que se cargue la aplicación.

### Integridad de recursos

Uno de los posibles efectos secundarios de mantener archivos en caché por periodos largos, es la conservación involuntaria de un recurso inválido. En un caché HTTP normal, un hard refresh o la caducidad de la caché limitan los efectos negativos de mantener un archivo inválido. Un service worker ignora esta limitaciones y mantiene efectivamente la aplicación por largo tiempo en la memoria caché. Por consiguiente es esencial que el service worker obtenga el contenido correcto.

Para garantizar la integridad de los recursos, el service worker de Angular valida los hashes de todos los recursos para los que tiene un hash. Típicamente para una aplicación creada mediante el [CLI de Angular](cli), esto incluye todo el directorio `dist` cubierto por el archivo de configuración de usuarios `src/ngsw-config.json`.

Si un archivo en particular falla en la validación, el service worker de Angular intenta recuperar el contenido utilizando el parámetro "cache-busting" (romper la caché) en la URL para eliminar los efectos del navegador o del caché intermedio. Si la validación del contenido también falla, el service worker considera que la versión completa de la aplicación no es válida y deja de servirla. Si es necesario, el service worker entra en un modo seguro en el que las solicitudes recaen en la red, optando por no utilizar la caché si el riesgo de servir contenido inválido, roto o desactualizado es alto. 

Los desajustes en los hashes pueden ocurrir por diversas razones:

* Las capas de almacenamiento en caché entre el servidor y el usuario final podría servir contenido obsoleto.

* Un despliegue no atómico podría dar lugar a que el service worker de Angular tenga visibilidad parcial del contenido actualizado.

* Los errores durante el proceso de compilación pueden generar recursos actualizados sin que el archivo `ngsw.json` se actualice. Lo contrario también podría suceder, es decir que se actualice el `ngsw.json` pero los recursos no.

#### Contenido sin hash

Los únicos recursos que tienen hashes en el manifiesto `ngsw.json` son los que estaban presentes en el directorio `dist` en el momento en que se creó el manifiesto. Otros recursos, especialmente los cargados desde un CDN, tienen contenido desconocido en el momento de la compilación o se actualizan con más frecuencia que la aplicación implementada.

Si el service worker de Angular no tiene un hash para validar un recurso dado, aún así almacena en caché su contenido pero bajo el principio de cabeceras HTTP en caché usando una política de "obsoleto mientras se revalida". Es decir, cuando las cabeceras HTTP en caché para un recurso en caché indican que el este ha caducado, el service worker de Angular sigue sirviendo el contenido e intenta refrescar el recurso en segundo plano. De esta manera,los recursos rotos que no han caducado no permanecen en caché más allá de su vida útil configurada.

{@a tabs}

### Pestañas de la aplicación

Que la versión cambie repentinamente o sin previo aviso, puede ser un problema para la aplicación. Consulte la sección anterior de [Versiones]](guide/service-worker-devops#versions) para obtener información acerca de dichos problemas. 

El service worker de Angular garantiza lo siguiente: un aplicación en ejecución seguirá ejecutando la misma versión de la aplicación. Si se abre otra instancia de la aplicación en una nueva pestaña del navegador, entonces se sirve la versión más reciente. Como resultado, esa nueva pestaña puede estar ejecutando una versión diferente de la aplicación que la pestaña original.

Es importante tener en cuenta que esta garantía es **más solida** que la proporcionada por el modelo de implementación web normal. Sin un service worker, no hay garantía de que el código cargado mediante lazy loading en una aplicación en ejecución sea de la misma versión que el código inicial de la aplicación.  

Existen muy pocas razones por las que el service worker de Angular podría cambiar la versión de una aplicación en funcionamiento. Estas razones podrían ser las siguientes condiciones de error: 

* La versión actual deja de ser válida debido a un fallo en los hashes. 
* Un error externo hace que el service worker entre en modo seguro, es decir se desactiva temporalmente.

El service worker de Angular siempre está informado sobre las versiones que se utilizan en cada momento y limpia las versiones cuando no hay ninguna pestaña que las utilice. 

Otras razones por las que el service worker de Angular podría cambiar la versión de una aplicación en ejecución son los eventos normales:
* La página se recarga/actualiza.
* La página solicita que se active una actualización de inmediato a través del servicio `SwUpdate`. 
 
### Actualizaciones del service worker

El service worker de Angular es un pequeño script que se ejecuta en los navegadores. De vez en cuando, el service worker puede actualizarse para corregir errores y mejorar funcionalidades.  

El service worker de Angular se descarga cuando se abre la aplicación por primera vez y cuando se accede a la aplicación después de un periodo de inactividad. Cuando existe algún cambio en el service worker, este se puede actualizar en segundo plano.

La mayoría de actualizaciones del service worker de Angular son transparentes para la aplicación&mdash; el contenido en caché sigue siendo válido y todavía se sirve de forma normal. Sin embargo, ocasionalmente, una corrección de errores o una nueva funcionalidad del service worker de Angular requiere la invalidación del contenido antiguo que se encuentra en caché. En este caso, la aplicación se actualizará de forma transparente desde la red.  

### Deshabilitando el service worker

En algunos casos, es posible que desees deshabilitar completamente el service worker y dejar que en su lugar el navegador maneje las solicitudes. Un ejemplo es cuando se utiliza alguna función que no es compatible con los service workers, (por ejemplo [informar sobre el progreso de carga de archivos](https://github.com/w3c/ServiceWorker/issues/1141)).

Para deshabilitar el service worker puedes establecer `ngsw-bypass` en el header de la petición o enviarlo como un parámetro por query. (El valor del header o del parámetro por query se ignora y puede estar vacío u omitirse.)

## Depurando el service worker de Angular

Ocasionalmente, puede ser necesario examinar el service worker de Angular mientras se está ejecutando para revisar issues o para asegurarse está funcionando de acuerdo al diseño. Los navegadores cuentan con herramientas incorporadas para depurar service workers y el propio service worker de Angular incluye funcionalidades de depuración útiles.

### Localizando y analizando la información de depuración

El service worker de Angular expone la información de depuración en el directorio virtual `ngsw/`. Actualmente, la única URL expuesta es `ngsw/state`. Aquí un ejemplo del contenido de dicha página de depuración:

```
NGSW Debug Info:

Driver state: NORMAL ((nominal))
Latest manifest hash: eea7f5f464f90789b621170af5a569d6be077e5c
Last update check: never

=== Version eea7f5f464f90789b621170af5a569d6be077e5c ===

Clients: 7b79a015-69af-4d3d-9ae6-95ba90c79486, 5bc08295-aaf2-42f3-a4cc-9e4ef9100f65

=== Idle Task Queue ===
Last update tick: 1s496u
Last update run: never
Task queue:
 * init post-load (update, cleanup)

Debug log:
```

#### Estado del driver

La primera línea indica el estado del driver:

```
Driver state: NORMAL ((nominal))
```

`NORMAL` indica que el service worker funciona correctamente y no se encuentra en estado degradado.

Hay dos posibles estados degradados:

* `EXISTING_CLIENTS_ONLY`: el service worker no tiene una copia limpia de la última versión conocida de la aplicación. Las versiones anteriores en caché son seguras de usar, por lo que las pestañas existentes continúan ejecutándose desde la memoria caché, pero las nuevas cargas de la aplicación serán servidas desde la red. El service worker intentará recuperarse de este estado cuando se detecte e instale una nueva versión de la aplicación (es decir, cuando haya un nuevo `ngsw.json` disponible).

* `SAFE_MODE`: el service worker no puede garantizar la seguridad del uso de datos en caché. Esto, debido a que ocurrió un error inesperado o ninguna de las versiones en caché es válida. Todo el tráfico será servido desde la red, ejecutando la menor cantidad de código posible desde el service worker. 

En ambos casos, la notación entre paréntesis indica cual fue el error que causó que el service worker pasara a estado degradado.

Ambos estados son temporales; se guardan únicamente durante el tiempo de vida útil de la [instancia del service worker (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope). Algunas veces el navegador termina una instancia inactiva del service worker para conservar memoria y potencia del procesador, y crea una nueva instancia del service worker en respuesta a eventos de la red. La nueva instancia incia en modo `NORMAL` independientemente del estado de la instancia anterior.


#### Último hash del manifiesto

```
Latest manifest hash: eea7f5f464f90789b621170af5a569d6be077e5c
```

Este es el hash SHA1 de la versión más actualizada de la aplicación que el service worker conoce. 


#### Última comprobación de actualización

```
Last update check: never
```

Indica la última vez que el service buscó una nueva versión, o una actualización de la aplicación. `never` indica que el service worker nunca ha buscado una actualización.

En este archivo de depuración de ejemplo, la comprobación de la actualización ha sido programada, como se explica en la siguiente sección.

#### Versión

```
=== Version eea7f5f464f90789b621170af5a569d6be077e5c ===

Clients: 7b79a015-69af-4d3d-9ae6-95ba90c79486, 5bc08295-aaf2-42f3-a4cc-9e4ef9100f65
```

En este ejemplo, el service worker tiene una versión de la aplicación en caché y esta se utiliza para servir dos pestañas diferentes. Tenga en cuenta que este hash de la versión es el "hash del manifiesto más reciente" indicado anteriormente. Ambos clientes están en la última versión. Cada cliente está listado por su ID en `Clients` API del navegador.

#### Cola de tareas inactivas

```
=== Idle Task Queue ===
Last update tick: 1s496u
Last update run: never
Task queue:
 * init post-load (update, cleanup)
```

La cola de tareas inactivas es la cola de todas las tareas pendientes que ocurren en segundo plano en el service worker. Si hay una tarea, esta en muestra en la lista junto con su descripción. En este ejemplo, el service worker tiene una tarea programada, una operación de post-inicialización que implica comprobar actualizaciones y limpiar la memoria caché obsoleta. 

Los parámetros "Last update tick" y "Last update run" son contadores que muestran el tiempo en que ocurrieron eventos específicos relacionados con la cola  de tareas. "Last update run" muestra la última hora en la que se ejecutaron las tareas inactivas. "Last update tick" muestra el tiempo transcurrido desde que se pudo procesar la última cola de tareas.

#### Logs del depurador

```
Debug log:
```

Aquí se registrarán todos los errores que se produzcan en el service worker.

### Herramientas de desarrollador

Algunos navegadores como Chrome proporcionan herramientas para que los desarrolladores puedan interactuar con los service workers. Dichas herramientas pueden ser muy potentes cuando se utilizan correctamente, pero hay que tener en cuenta algunas consideraciones. 

* Cuando se utilizan las herramientas de desarrollador, el service worker se mantiene en ejecución en segundo plano y nunca se reinicia. Esto puede causar que el comportamiento al usar las herramientas de desarrollador difiera del comportamiento que podría experimentar un usuario.

* Si examinas el visor de almacenamiento en caché (Cache Storage), notarás que frecuentemente la caché esta desactualizada. Haz click derecho en Cache Storage y selecciona la opción de actualizar caché (Refresh cache).

Detener e iniciar el service worker en el panel Service Worker activa la búsqueda de actualizaciones.

## Seguridad del Service Worker

Como cualquier sistema complejo, los errores o una mala configuración pueden ocasionar que el service worker de Angular opere de forma imprevista. Aunque su diseño intenta minimizar el impacto de estos problemas, el service worker de Angular contiene varios mecanismos de seguridad en caso de que un administrador necesite desactivar rápidamente el service worker. 

### Fail-safe

Para desactivar el service worker, elimina o renombra el archivo `ngsw.json`. Cuando el service worker hace petición del archivo `ngsw.json`, se devuelve un `404`, elimina toda su memoria caché y se desregistra, básicamente se autodestruye. 

### Safety Worker

También se incluye en el paquete de npm `@angular/service-worker` un pequeño script llamado `safety-worker.js`, que cuando se cargue desregistrará el service worker del navegador. Este script puede utilizarse como último recurso para deshacerse de los service workers no deseados que ya se encuentren instalados en las páginas de los clientes.

Es importante tener en cuenta que no se puede registrar este safety worker directamente, ya que los clientes antiguos que se ejecutan desde caché podrían no detectar un nuevo `index.html` que instala el script del nuevo worker. En su lugar, debes servir el contenido de `safety-worker.js` en la URL del script del service worker que intentas desregistrar, y debes continuar haciéndolo hasta que estés seguro de que todos los usuarios han desregistrado con éxito el antiguo worker. Para la mayoría de los sitios, esto significa que debes servir el contenido de `safety-worker.js` en la antigua URL del service worker que deseas desregistrar para siempre. 

Este script puede usarse tanto para desactivar `@angular/service-worker` como para cualquier otro service worker que pudiera haberse servido en el pasado en tu sitio. 

### Cambiando la dirección de tu aplicación


Es importante tener en cuenta que los service workers no trabajan cuando hay por detrás una redirección. Es posible que ya te hayas encontrado con el error `The script resource is behind a redirect, which is disallowed` (El script del recurso está detrás de una redirección, que no está permitida).
Esto puede ser un problema si tienes que cambiar la dirección de tu aplicación. Si configuras una redirección desde la antigua dirección (por ejemplo `ejemplo.com`) a la nueva (por ejemplo `www.example.com`) el worker dejará de funcionar. 
Además, la redirección ni siquiera se activará para los usuarios que estén cargando el sitio completamente desde el Service Worker. El antiguo worker (registrado en `ejemplo.com`) intenta actualizar y envía peticiones a la antigua dirección `ejemplo.com` que son redirigidas a la nueva dirección `www.example.com` y crean el error `The script resource is behind a redirect, which is disallowed`.

Para remediar esto, es posible que tenga que eliminar el antiguo worker utilizando una de las técnicas anteriores técnicas ([Fail-safe](#fail-safe) o [Safety Worker](#safety-worker)).

## Más acerca del service worker de Angular

También puede interesarte lo siguiente:
* [Configuración del Service Worker](guide/service-worker-config).
