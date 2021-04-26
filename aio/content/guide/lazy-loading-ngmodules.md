# Carga diferida en módulos

Por defecto, los NgModules son cargados de manera _eagerly_, esto significa que tan pronto como la aplicación cargue, también cargarán todos los NgModules, sean o no inmediatamente necesarios. Para aplicaciones grandes con muchas rutas, considera la carga diferida&mdash;un patrón de diseño que carga los NgModules como se vayan necesitando. La carga diferida ayuda a mantener más pequeño el tamaño de los bundles iniciales, lo que a su vez ayuda a disminuir los tiempos de carga.

<div class="alert is-helpful">
  Para la aplicación de muestra final con dos módulos de carga diferida que se describen en esta pagina, ve el <live-example></live-example>.
</div>

{@a lazy-loading}

## Conceptos básicos de carga diferida

Esta sección introduce el procedimiento básico para configurar rutas con carga diferida. Para un ejemplo paso a paso, ve la sección [paso a paso](#step-by-step) en esta página.
Para aplicar la carga diferida en los módulos de angular usa `loadchildren` (en lugar de `component`) en tu configuración de `AppRoutingModule` `routes` como se describe en el siguiente ejemplo.
<code-example header="AppRoutingModule (excerpt)">

const routes: Routes = [
{
path: 'items',
loadChildren: () => import('./items/items.module').then(m => m.ItemsModule)
}
];

</code-example>

En el archivo de rutas del módulo que será cargado de manera diferida, agrega una ruta para el componente.

<code-example header="Routing module for lazy loaded module (excerpt)">

const routes: Routes = [
{
path: '',
component: ItemsComponent
}
];

</code-example>

También asegurate de remover el `ItemsModule` de el `AppModule`..
Para obtener instrucciones paso a paso sobre los módulos de carga diferida, continua con las siguientes seciones de esta página.

{@a step-by-step}

## Preparación del paso a paso

Hay dos pasos principales para configurar la carga diferida en el módulo de funcionalidades:

1. Crear un módulo de funcionalidades con el CLI, usando la bandera `--route`.
2. Configurar las rutas.

### Configurar una aplicación

Si no tienes una aplicación, puedes seguir los pasos que están abajo para crear una con el CLI. Si ya tienes una aplicación, ve a [Configurar las rutas](#config-routes). Ejecuta el siguiente comando en el cuál `customer-app` es el nombre de tu aplicación:

<code-example language="bash">
ng new customer-app --routing
</code-example>

Este comando crea una aplicación llamada `customer-app` y la bandera `--routing` genera un archivo llamado `app-routing.module.ts`, el cuál es uno de los archivos que se necesitan para configurar la funcionalidad de carga diferida en tu módulo.
Entra a tu proyecto con ejecutando el comando `cd customer-app`.

<div class="alert is-helpful">

La opcion `--routing` requiere Angular/CLI versión 8.1 o superior.
Ve [Mantenerse actualizado](guide/updating).

</div

### Crea un módulo de funcionalidades con routing

Ahora, necesitarás un módulo de funcionalidades con un componente a enrutar.
Para hacer uno, ejecuta el siguiente comando en la terminal donde `customers` es el nombre de tu módulo de funcionalidades. La ruta para cargar el módulo de `customers` es también `customers` porque está especificado con la opción `--route`.

<code-example language="bash">
ng generate module customers --route customers --module app.module
</code-example>

Este comando crea un directorio llamado `customers` con el nuevo módulo de carga diferida llamado `CustomersModule` definido en el archivo de `customers.module.ts`. El comando automaticamente declara el componente `CustomersComponent` dentro del nuevo módulo de funcionalidades.

Debido a que se entiende que el nuevo módulo será con carga diferida, el comando no agrega una referencia al nuevo módulo de funcionalidades en el archivo raíz del módulo de la aplicación, `app.module.ts`. En lugar de eso, se agrega la ruta declarada, `customers` al arreglo `routes` declarado en el módulo proporcionado con la opción `---module`.

<code-example
  header="src/app/app-routing.module.ts"
  path="lazy-loading-ngmodules/src/app/app-routing.module.ts"
  region="routes-customers">
</code-example>

Nota que en la sintaxis de la carga diferida se usa `loadChildren` seguido de una función que usa la sintaxis `import('...')` incorporada en el navegador para importaciones dinámicas.
La ruta de importación es relativa a la ruta del módulo.

#### Agregar otro módulo de funcionalidades

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
