# Módulos de funciones de carga diferida

Por defecto, los NgModules son cargados de manera _eagerly_, esto significa que tan pronto como la aplicación cargue, también cargarán todos los NgModules, sean o no inmediatamente necesarios. Para aplicaciones grandes con muchas rutas, considera la carga diferida&mdash;un patrón de diseño que carga los NgModules como se vayan necesitando. La carga diferida ayuda a mantener más pequeño el tamaño de los bundles iniciales, lo que a su vez ayuda a disminuir los tiempos de carga.

<div class="alert is-helpful">
  Para la aplicación final de muestra con los dos módulos de carga diferida que se describen en esta pagina, ve el <live-example></live-example>.
</div>

{@a lazy-loading}

## Conceptos básicos de carga diferida

Esta sección introduce el procedimiento básico para configurar rutas con carga diferida. Para un ejemplo paso a paso, ve la sección [paso a paso](#step-by-step) en esta página.
Para aplicar la carga diferida en los módulos de angular usa `loadchildren` (en lugar de `component`) en tu configuración de `routes` en `AppRoutingModule` como se describe en el siguiente ejemplo.

<code-example header="AppRoutingModule (extracto)">

const routes: Routes = [
  {
    path: 'items',
    loadChildren: () => import('./items/items.module').then(m => m.ItemsModule)
  }
];

</code-example>

En el módulo de enrutamiento del módulo de carga diferida, agregue una ruta para el componente.

<code-example header="Módulo de enrutamiento para el módulo de carga diferida (extracto)">

const routes: Routes = [
  {
    path: '',
    component: ItemsComponent
  }
];

</code-example>

También asegurate de remover el `ItemsModule` de el `AppModule`.
Para obtener instrucciones paso a paso sobre los módulos de carga diferida, continúa con las siguientes seciones de esta página.

{@a step-by-step}

## Preparación del paso a paso

Hay dos pasos principales para configurar la carga diferida en el módulo de funciones:

1. Crear un módulo de funciones con el CLI, usando la bandera `--route`.
2. Configurar las rutas.

### Configurar una aplicación

Si no tienes una aplicación, puedes seguir los pasos que están abajo para crear una con el CLI. Si ya tienes una aplicación, ve a [Configurar las rutas](#config-routes). Ejecuta el siguiente comando en el cuál `customer-app` es el nombre de tu aplicación:

<code-example language="bash">
ng new customer-app --routing
</code-example>

Este comando crea una aplicación llamada `customer-app` y la bandera `--routing` genera un archivo llamado `app-routing.module.ts`, el cuál es uno de los archivos que se necesitan para configurar la funcionalidad de carga diferida en tu módulo.
Entra a tu proyecto ejecutando el comando `cd customer-app`.

<div class="alert is-helpful">

La opcion `--routing` requiere Angular/CLI versión 8.1 o superior.
Ve [Mantenerse actualizado](guide/updating).

</div

### Crea un módulo de funciones con routing

Para continuar, necesitarás un módulo de funciones con un componente a enrutar.
Para hacer uno, ejecuta el siguiente comando en la terminal donde `customers` es el nombre de tu módulo de funciones. La ruta para cargar el módulo de `customers` es también `customers` porque está especificado con la opción `--route`.

<code-example language="bash">
ng generate module customers --route customers --module app.module
</code-example>

Este comando crea un directorio llamado `customers` con el nuevo módulo de carga diferida llamado `CustomersModule` definido en el archivo de `customers.module.ts`. El comando automaticamente declara el componente `CustomersComponent` dentro del nuevo módulo de funciones.

Debido a que se entiende que el nuevo módulo será con carga diferida, el comando no agrega una referencia al nuevo módulo de funciones en el archivo raíz del módulo de la aplicación, `app.module.ts`. En lugar de eso, se agrega la ruta declarada, `customers` al array `routes` declarado en el módulo proporcionado con la opción `---module`.

<code-example
  header="src/app/app-routing.module.ts"
  path="lazy-loading-ngmodules/src/app/app-routing.module.ts"
  region="routes-customers">
</code-example>

Nota que en la sintaxis de la carga diferida se usa `loadChildren` seguido de una función que usa la sintaxis `import('...')` incorporada en el navegador para importaciones dinámicas.
La ruta de importación es relativa a la ruta del módulo.

#### Agregar otro módulo de funciones

Usa el mismo comando para crear un segundo módulo de carga diferida con enrutador, junto con su componente auxiliar.

<code-example language="bash">
ng generate module orders --route orders --module app.module
</code-example>

Esto crea un nuevo directorio llamado `orders`, contiene los archivos `OrdersModule` y `OrdersRoutingModule`, junto con el nuevo componente `OrdersComponent`.
La ruta de `orders`, especificada con la opción `--route`, es agregada al array de `routes` dentro del archivo `app-routing.module.ts`, usando la sintaxis de carga diferida.

<code-example
  header="src/app/app-routing.module.ts"
  path="lazy-loading-ngmodules/src/app/app-routing.module.ts"
  region="routes-customers-orders">
</code-example>

### Preparar la UI

Aunque puedes escribir la URL en la barra de dirección, una interfaz de usuario de navegación es más fácil para el usuario y más común.
Reemplaza el contenido que viene por defecto en `app.component.html` por una barra de navegación personalizada para que puedas navegar de manera sencilla por los módulos en el navegador.

<code-example path="lazy-loading-ngmodules/src/app/app.component.html" header="app.component.html" region="app-component-template" header="src/app/app.component.html"></code-example>

Para ver tu aplicación hasta el momento en el navegador, ingresa el siguiente comando en la terminal:

<code-example language="bash">
ng serve
</code-example>

Ahora puedes ir a `localhost:4200` donde deberías ver “customer-app” y tres botones.

<div class="lightbox">
  <img src="generated/images/guide/lazy-loading-ngmodules/three-buttons.png" width="300" alt="tres botones en el navegador">
</div>

Estos botones funcionan porque el CLI agregó automáticamente las rutas de los módulos de funionalidades en el array `routes` dentro del `app.module.ts`.

{@a config-routes}

### Importaciones y configuración del enrutador

El CLI automáticament agregó cada módulo de funciones al mapa de rutas a nivel de la aplicación.
Para terminar esto agrega una ruta predeterminada. En el archivo `app-routing.module.ts`, actualiza el array de `routes` con lo siguiente:

<code-example path="lazy-loading-ngmodules/src/app/app-routing.module.ts" id="app-routing.module.ts" region="const-routes" header="src/app/app-routing.module.ts"></code-example>

Las primeras dos rutas son las rutas de `CustomerModule` y de `OrdersModule`.
La entrada final define la ruta predeterminada. La ruta vacía coincide con todo lo que no haya coincidido con las rutas anteriores.

### Dentro del módulo de funciones

Para continuar, echa un vistazo al archivo `customers.module.ts`. Si estás usando el CLI y siguiendo los pasos resumidos en esta página, no tienes que hacer nada aquí.

<code-example path="lazy-loading-ngmodules/src/app/customers/customers.module.ts" id="customers.module.ts" region="customers-module" header="src/app/customers/customers.module.ts"></code-example>

El archivo `customers.module.ts` importa los archivos `customers-routing.module.ts` y `customers.component.ts`. `CustomerRoutingModule` está listado en el array de `imports` en `@NgModule` dando acceso a `CustomersModule` a su propio módulo de enrutamiento. `CustomersComponent` está en el array de `declarations`, esto significa que `CustomersComponent` pertenece a `CustomersModule`.

El archivo `app-routing.module.ts` importa el módulo de funciones, `customers.module.ts` usando importación dinámica.

El archivo específico de definición de ruta `customers-routing.module.ts` importa su propio componente de funcionalidades definido en el archivo `customers.component.ts`, junto con las otras declaraciones de importaciones de JavaScript. A continuación asigna la ruta vacía al `CustomerComponent`,

<code-example path="lazy-loading-ngmodules/src/app/customers/customers-routing.module.ts" id="customers-routing.module.ts" region="customers-routing-module" header="src/app/customers/customers-routing.module.ts"></code-example>

El `path` aquí está como una string vacía porque la ruta en `AppRoutingModule` ya está configurada como `customers`, por lo tanto esta ruta en `CustomersRoutingModule`, ya está dentro del contexto de `customers`. Toda ruta en este módulo de enrutamiento es una ruta hija.

El módulo de enrutamiento del otro módulo de funciones está configurado de una manera similar.

<code-example path="lazy-loading-ngmodules/src/app/orders/orders-routing.module.ts" id="orders-routing.module.ts" region="orders-routing-module-detail" header="src/app/orders/orders-routing.module.ts (extracto)"></code-example>

### Verifica la carga diferida

Ahora puedes revisar que un módulo está siendo cargado de manera diferida con las herramientas de desarrollador de Chrome. En Chrome, abre las herramientas de desarrollador presionando `Cmd+Option+i` en Mac o `Ctrl+Shift+j` en una PC y ve a la pestaña de Red o Network.

<div class="lightbox">
  <img src="generated/images/guide/lazy-loading-ngmodules/network-tab.png" width="600" alt="diagrama de módulos con carga diferida ">
</div>

Haz click en el botón de de Orders o Customers. Si ves que aparece un _chunk_, todo está configurado de manera adecuada y el módulo de funciones está siendo cargado de manera diferida. Un _chunk_ debería aparecer para Orders y para Customers pero sólo aparecerá una vez para cada uno.

<div class="lightbox">
  <img src="generated/images/guide/lazy-loading-ngmodules/chunk-arrow.png" width="600" alt="diagrama de carga diferida">
</div>

Para verlo de nuevo, o probar después de trabajar en el proyecto, limpia todo dando click en el circulo que tiene una línea atravezada situado en la parte superior izquierda en la pestaña de Network.

<div class="lightbox">
  <img src="generated/images/guide/lazy-loading-ngmodules/clear.gif" width="200" alt="lazy loaded modules diagram">
</div>

Después recarga con `Cmd+r` o `Ctrl+r`, dependiend el sistema operativo que estés utilizando.

## `forRoot()` y `forChild()`

Como ya habrás notado, el CLI agrega `RouterModule.forRoot(routes)` al array de `imports` en `AppRoutingModule`.
Esto permite a Angular saber que `AppRoutingModule` es un módulo de enrutamiento y `forRoot()` especifica que que es el módulo raiz de rutas.
Esto configura todas las rutas que le pasas, dando acceso a las directivas del enrutador, y registra el servicio de `Router`.
Usa `forRoot()` solamente una vez en la aplicación, dentro de `AppRoutingModule`.

El CLI también agrega `RouterModule.forChild(routes)` al módulo de enrutamiento.
De esta manera, Angular sabe que la lista de rutas solo es responsable de proporcionar rutas adicionales y está destinado a módulos de funciones.
También puedes usar `forChild()` en múltiples módulos.

El método `forRoot()` se encarga de la configuración del inyector _global_ para el enrutador.
El método `forChild()` no tiene configuración de inyector. Se usan directivas como `RouterOutlet` y `RouterLink`.
Para más información, revisa la sección de [patrón `forRoot()`](guide/singleton-services#forRoot) de la guía [Servicios Singleton](guide/singleton-services)

{@a preloading}

## Precarga

La precarga mejora la experiencia de usuario cargando partes de tu aplicación en segundo plano.
Puedes precargar módulos o datos de componentes.

### Precarga de módulos

La precarga de módulos mejora la experiencia de usuario cargando partes de tu aplicación en segundo plano para que los usuarios no tengan que esperar hasta que se descarguen estos elementos cuando activen una ruta.

Para habilitar la precarga de todos los módulos con carga diferida, importa el token `PreloadAllModules` desde Angular `router`.

<code-example header="AppRoutingModule (extracto)">

import { PreloadAllModules } from '@angular/router';

</code-example>

En el archivo `AppRoutingModule`, especifica tu estrategia de precarga en `forRoot()`.

<code-example header="AppRoutingModule (extracto)">

RouterModule.forRoot(
  appRoutes,
  {
    preloadingStrategy: PreloadAllModules
  }
)

</code-example>

### Precargando datos de componentes

Para precargar datos de componentes puedes usar un `resolver`.
Los _resolvers_ mejoran la experiencia de usuario al bloquear la carga de la página hasta que todos los datos necesarios estén disponibles para mostrar la página por completo.

#### Resolvers

Crea un servicio de _resolvers_.
Con el CLI, el comando para generar un servicio es el siguiente:

<code-example language="none" class="code-shell">
  ng generate service <service-name>
</code-example>

En tu servicio, importa los siguientes miembros del enrutador, implementa `Resolve`, e injecta el servicio de `Router`:

<code-example header="Resolver service (extracto)">

import { Resolve } from '@angular/router';

...

export class CrisisDetailResolverService implements Resolve<> {
  resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<> {
    // tu lógica aquí
  }
}

</code-example>

Importa este _resolver_ al módulo de enrutamiento de tu módulo.

<code-example header="Feature module's routing module (extracto)">

import { YourResolverService } from './your-resolver.service';

</code-example>

Agrega un objeto `resolver` a la configuración del `route` del componente.

<code-example header="Feature module's routing module (extracto)">
{
  path: '/your-path',
  component: YourComponent,
  resolve: {
    crisis: YourResolverService
  }
}
</code-example>

En el componente, usa un `Observable` para obtener datos desde el `ActivatedRoute`.

<code-example header="Component (extracto)">
ngOnInit() {
  this.route.data
    .subscribe((your-parameters) => {
      // el código de tus datos específicos va aquí
    });
}
</code-example>

Para más información con un ejemplo práctico, ve el [tutorial de enrutamiento sobre prearga](guide/router-tutorial-toh#preloading-background-loading-of-feature-areas).

<hr>

## Más sobre NgModules y routing

También te podría interesar lo siguiente:

- [Enrutamiento y navegación](guide/router).
- [Providers](guide/providers).
- [Tipos en los módulos de funciones](guide/module-types).
- [División de código a nivel de ruta en Angular](https://web.dev/route-level-code-splitting-in-angular/)
- [Estrategias de precarga en Angular](https://web.dev/route-preloading-in-angular/)
