# Service worker en producción

Esta página es una referencia para el despliegue y soporte de aplicaciones en producción que utilizan el service worker de Angular. Se explica como el service worker de Angular se ajusta en un entorno de producción muy grande, como se comporta bajo diversas condiciones y recursos disponibles, y su sistema a prueba de fallos (fail-safes).

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

Cada vez que el usuario abre o refresca la aplicación, el service worker de Angular verifica si existen actualizaciones en la aplicación buscando cambios en el manifiesto `ngsw.json`. Si se encuentra alguna actualización, esta se descarga y se almacena en caché automaticamente, y se servirá la próxima vez que se cargue la aplicación. 

### Integridad de recursos

Uno de los posibles efectos secundarios de mantener archivos en caché por periodos largos, es la conservación involuntaria de un recurso inválido. En un caché HTTP normal, un hard refresh o la caducidad de la caché limitan los efectos negativos de mantener un archivo inválido. Un service worker ignora esta limitaciones y mantiene efectivamente la aplicación por largo tiempo en la memoria caché. Por consiguiente es esecial que el service worker obtenga el contenido correcto.

Para garantizar la integridad de los recursos, el service worker de Angular valida los hashes de todos los recursos para los que tiene un hash. Típicamente para una aplicación creada mediante el [CLI de Angular](cli), esto incluye todo el directorio `dist` cubierto por el archivo de configuración de usuarios `src/ngsw-config.json`.

Si un archivo en particular falla en la validación, el service worker de Angular intenta recuperar el contenido utilizando el parámetro "cache-busting" (romper la caché) en la URL para eliminar los efectos del navegador o del caché intermedio. Si la validación del contenido también falla, el service worker considera que la versión completa de la aplicación no es válida y deja de servirla. Si es necesario, el service worker entra en un modo seguro en el que las solicitudes recaen en la red, optando por no utilizar la caché si el riesgo de servir contenido inválido, roto o desactualizado es alto. 

Los desajustes en los hashes pueden ocurrir por diversas razones:

* Las capas de almacenamiento en caché entre el servidor y el usuario final podría servir contenido obsoleto.

* Un despliegue no atómico podría dar lugar a que el service worker de Angular tenga visibilidad parcial del contenido actualizado.

* Los errores durante el proceso de compilación pueden generar recursos actualizados sin que el archivo `ngsw.json` se actualice. Lo contrario también podría suceder, es decir que se actualice el `ngsw.json` pero los recursos no.

#### Contenido sin hash

Los únicos recursos que tienen hashes en el manifesto `ngsw.json` son los que estaban presentes en el directorio `dist` en el momento en que se creó el manifiesto. Otros recursos, especialmente los cargados desde un CDN, tienen contenido desconocido en el momento de la compilación o se actualizan con más frecuencia que la aplicación implementada.

Si el service worker de Angular no tiene un hash para validar un recurso dado, aún así almacena en caché su contenido pero bajo el principio de cabeceras HTTP en caché usando una política de "obsoleto mientras se revalida". Es decir, cuando las cabeceras HTTP en caché para un recurso en caché indican que el este ha caducado, el service worker de Angular sigue sirviendo el contenido e intenta refrescar el recurso en segundo plano. De esta manera,los recursos rotos que no han caducado no permanecen en caché más allá de su vida útil configurada.

{@a tabs}

### App tabs

It can be problematic for an app if the version of resources it's receiving changes suddenly or without warning. See the [Versions](guide/service-worker-devops#versions) section above for a description of such issues.

The Angular service worker provides a guarantee: a running app will continue to run the same version of the app. If another
instance of the app is opened in a new web browser tab, then the most current version of the app is served. As a result, that new tab can be running a different version of the app than the original tab.

It's important to note that this guarantee is **stronger** than that provided by the normal web deployment model. Without a service worker, there is no guarantee that code lazily loaded later in a running app is from the same version as the initial code for the app.

There are a few limited reasons why the Angular service worker might change the version of a running app. Some of them are error conditions:

* The current version becomes invalid due to a failed hash.
* An unrelated error causes the service worker to enter safe mode; that is, temporary deactivation.

The Angular service worker is aware of which versions are in use at any given moment and it cleans up versions when no tab is using them.

Other reasons the Angular service worker might change the version of a running app are normal events:

* The page is reloaded/refreshed.
* The page requests an update be immediately activated via the `SwUpdate` service.

### Service worker updates

The Angular service worker is a small script that runs in web browsers.
From time to time, the service worker will be updated with bug fixes and feature improvements.

The Angular service worker is downloaded when the app is first opened and when the app is accessed after a period of inactivity. If the service worker has changed, the service worker will be updated in the background.

Most updates to the Angular service worker are transparent to the app&mdash;the old caches are still valid and content is still served
normally. However, occasionally a bugfix or feature in the Angular service worker requires the invalidation of old caches. In this case, the app will be refreshed transparently from the network.

### Bypassing the service worker

In some cases, you may want to bypass the service worker entirely and let the browser handle the request instead. An example is when you rely on a feature that is currently not supported in service workers (e.g. [reporting progress on uploaded files](https://github.com/w3c/ServiceWorker/issues/1141)).

To bypass the service worker you can set `ngsw-bypass` as a request header, or as a query parameter. (The value of the header or query parameter is ignored and can be empty or omitted.)

## Debugging the Angular service worker

Occasionally, it may be necessary to examine the Angular service worker in a running state to investigate issues or to ensure that
it is operating as designed. Browsers provide built-in tools for debugging service workers and the Angular service worker itself
includes useful debugging features.

### Locating and analyzing debugging information

The Angular service worker exposes debugging information under the `ngsw/` virtual directory. Currently, the single exposed URL is `ngsw/state`. Here is an example of this debug page's contents:

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

#### Driver state

The first line indicates the driver state:

```
Driver state: NORMAL ((nominal))
```

`NORMAL` indicates that the service worker is operating normally and is not in a degraded state.

There are two possible degraded states:

* `EXISTING_CLIENTS_ONLY`: the service worker does not have a
clean copy of the latest known version of the app. Older cached
versions are safe to use, so existing tabs continue to run from
cache, but new loads of the app will be served from the network.
The service worker will try to recover from this state when a new
version of the application is detected and installed (that is,
when a new `ngsw.json` is available).

* `SAFE_MODE`: the service worker cannot guarantee the safety of
using cached data. Either an unexpected error occurred or all
cached versions are invalid. All traffic will be served from the
network, running as little service worker code as possible.

In both cases, the parenthetical annotation provides the
error that caused the service worker to enter the degraded state.

Both states are temporary; they are saved only for the lifetime of the [ServiceWorker
instance](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope).
The browser sometimes terminates an idle service worker to conserve memory and
processor power, and creates a new service worker instance in response to
network events. The new instance starts in the `NORMAL` mode, regardless of the
state of the previous instance.

#### Latest manifest hash

```
Latest manifest hash: eea7f5f464f90789b621170af5a569d6be077e5c
```

This is the SHA1 hash of the most up-to-date version of the app that the service worker knows about.


#### Last update check

```
Last update check: never
```

This indicates the last time the service worker checked for a new version, or update, of the app. `never` indicates that the service worker has never checked for an update.

In this example debug file, the update check is currently scheduled, as explained the next section.

#### Version

```
=== Version eea7f5f464f90789b621170af5a569d6be077e5c ===

Clients: 7b79a015-69af-4d3d-9ae6-95ba90c79486, 5bc08295-aaf2-42f3-a4cc-9e4ef9100f65
```

In this example, the service worker has one version of the app cached and
being used to serve two different tabs. Note that this version hash
is the "latest manifest hash" listed above. Both clients are on the
latest version. Each client is listed by its ID from the `Clients`
API in the browser.


#### Idle task queue

```
=== Idle Task Queue ===
Last update tick: 1s496u
Last update run: never
Task queue:
 * init post-load (update, cleanup)
```

The Idle Task Queue is the queue of all pending tasks that happen
in the background in the service worker. If there are any tasks
in the queue, they are listed with a description. In this example,
the service worker has one such task scheduled, a post-initialization
operation involving an update check and cleanup of stale caches.

The last update tick/run counters give the time since specific
events happened related to the idle queue. The "Last update run"
counter shows the last time idle tasks were actually executed.
"Last update tick" shows the time since the last event after
which the queue might be processed.


#### Debug log

```
Debug log:
```

Errors that occur within the service worker will be logged here.


### Developer Tools

Browsers such as Chrome provide developer tools for interacting
with service workers. Such tools can be powerful when used properly,
but there are a few things to keep in mind.

* When using developer tools, the service worker is kept running
in the background and never restarts. This can cause behavior with Dev
Tools open to differ from behavior a user might experience.

* If you look in the Cache Storage viewer, the cache is frequently
out of date. Right click the Cache Storage title and refresh the caches.

Stopping and starting the service worker in the Service Worker
pane triggers a check for updates.

## Service Worker Safety

Like any complex system, bugs or broken configurations can cause
the Angular service worker to act in unforeseen ways. While its
design attempts to minimize the impact of such problems, the
Angular service worker contains several failsafe mechanisms in case
an administrator ever needs to deactivate the service worker quickly.

### Fail-safe

To deactivate the service worker, remove or rename the
`ngsw.json` file. When the service worker's request
for `ngsw.json` returns a `404`, then the service worker
removes all of its caches and de-registers itself,
essentially self-destructing.

### Safety Worker

Also included in the `@angular/service-worker` NPM package is a small
script `safety-worker.js`, which when loaded will unregister itself
from the browser. This script can be used as a last resort to get rid
of unwanted service workers already installed on client pages.

It's important to note that you cannot register this worker directly,
as old clients with cached state may not see a new `index.html` which
installs the different worker script. Instead, you must serve the
contents of `safety-worker.js` at the URL of the Service Worker script
you are trying to unregister, and must continue to do so until you are
certain all users have successfully unregistered the old worker. For
most sites, this means that you should serve the safety worker at the
old Service Worker URL forever.

This script can be used both to deactivate `@angular/service-worker`
as well as any other Service Workers which might have been served in
the past on your site.

### Changing your app's location

It is important to note that service workers don't work behind redirect. You 
may have already encountered the error `The script resource is behind a redirect, which is disallowed`.

This can be a problem if you have to change your app's location. If you setup 
a redirect from the old location (for example `example.com`) to the new 
location (for example `www.example.com`) the worker will stop working. 
Also, the redirect won't even trigger for users who are loading the site 
entirely from Service Worker. The old worker (registered at `example.com`)
 tries to update and sends requests to the old location `example.com` which 
 get redirected to the new location `www.example.com` and create the error 
`The script resource is behind a redirect, which is disallowed`.

To remedy this, you may need to kill the old worker using one of the above
techniques ([Fail-safe](#fail-safe) or [Safety Worker](#safety-worker)).


## More on Angular service workers

You may also be interested in the following:
* [Service Worker Configuration](guide/service-worker-config).

