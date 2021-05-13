{@a router-tutorial}

# Tutorial de enrutador: tour de héroes

Este tutorial provee una descripción extensa sobre el enrutador de Angular. En este tutorial, podrás configurar un enrutador básico para explorar sus características como rutas hijas, parámetros de rutas, carga diferida de NgModules, rutas protegidas y pre-cargar datos para mejorar la experiencia de usuario.

Si deseas un ejemplo funcional de la versión final de la app, puedes ver <live-example name="router"></live-example>.

{@a router-tutorial-objectives}

## Objetivos

Esta guía describe el desarrollo de una aplicación de ejemplo con rutas multi-página. En el camino, se destacarán las características clave del enrutador como:

- Organizar las funcionalidades de la aplicación en módulos.
- Navegación a un componente (_Heroes_ enlaza a "Heroes List").
- Incluir parámetros de ruta (pasar el `id` de Hero mientras enrutas a la sección `Hero Detail`).
- Rutas Hijas (la sección _Crisis Center_ tiene sus propias rutas).
- El guard de ruta `CanActivate` (control de acceso a la ruta).
- El guard de ruta `CanActivateChild` guard (verificando el acceso a la ruta hija).
- El guard de ruta `CanDeactivate` (pedir permiso para descartar cambios no guardados).
- El guard de ruta `Resolve` (pre-carga los datos de la ruta).
- Carga diferida y `NgModule`.
- El guard de ruta `CanLoad` (verificar antes de cargar los assets del mofulo de funciones).

Esta guía tiene una secuencia de hitos como si estuvieras creando la aplicación paso a paso, pero asume que estás familiarizado con lo básico de los [conceptos de Angular](guide/architecture).
Para una introducción general a angular, puede ver el [Comienzo con Angular](start). Para una descripción más detallada puedes ver el tutorial [Tour de Héroes](tutorial).

## Pre-requisitos

Para completar este tutorial, deberías de tener un entendimiento básico de los siguientes conceptos:

- JavaScript
- HTML
- CSS
- [Angular CLI](/cli)

Podrías encontrar el [tutorial Tour de Héroes](/tutorial) útil, pero no es requerido.

## La aplicación de ejemplo en acción

La aplicación de ejemplo para este tutorial ayuda a la Agencia de Empleo de Héroes a encontrar crisis para que los héroes los resuelvan.

La aplicación tiene tres áreas de funcionalidades principales:

1. El _Centro de Crisis_ que mantiene la lista de crisis para su asignación a los héroes.
2. El área de _Héroes_ que mantiene la lista de héroes empleados por la agencia.
3. El área de _Admin_ para administrar la lista de crisis y héroes.

Puedes probarlo haciendo clic en este <live-example name="router" title="Ejemplo en vivo de la Agencia de Empleo para Héroes.">vínculo al ejemplo en vivo</live-example>.

La app se renderiza con una fila de botones de navegación y la vista de _Héroes_ con la lista de héroes.

<div class="lightbox">
  <img src='generated/images/guide/router/hero-list.png' alt="Lista de Héroes">
</div>

Seleccionas un héroe y la app te lleva a una pantalla de edición de Héroe.

<div class="lightbox">
  <img src='generated/images/guide/router/hero-detail.png' alt="Detalle del Centro de Crisis">
</div>

Editando el nombre.
Haz clic en el botón de "Back" y la app regresa a la lista de heroes la cual muestra el nombre del héroe editado.
Puedes notar que el cambio del nombre toma efecto inmediatamente.

Si haces clic en el botón de retroceder del navegador en lugar del botón de "Back" de la app, la app también habría regresado a la lista de héroes.

La navegación de la app de Angular actualiza el historial del navegador de la misma manera que lo hace la navegación de un sitio web normal.

Ahora haz clic en el vínculo de _Centro de crisis_ para una lista de crisis en curso.

<div class="lightbox">
  <img src='generated/images/guide/router/crisis-center-list.png' alt="Lista del Centro de Crisis">
</div>

Seleccionas la crisis y la aplicación te lleva a la pantalla de edición de crisis.
El _Crisis Detail_ aparece en un componente hijo en la misma página, debajo de la lista.

Editando el nombre de la crisis.
Puedes nota que el nombre correspondiente a la crisis en la lista _no_ cambia.

<div class="lightbox">
  <img src='generated/images/guide/router/crisis-center-detail.png' alt="Detalle del Centro de Crisis">
</div>

A diferencia de _Hero Detail_, que se actualiza a medida que escribes, los cambios de _Crisis Detail_ son temporales hasta que los guardes o los descartes usando los botones de "Save" o "Cancelar".
Ambos botones navegan de regreso al _Crisis Center_ y su lista de crisis.

Haz clic en el botón de retroceso del navegador o el vínculo de "Heroes" para activar un cuadro de dialogo.

<div class="lightbox">
  <img src='generated/images/guide/router/confirm-dialog.png' alt="Diálogo de Confirmación">
</div>

Puedes presionar "Ok" y perder tus cambios o hacer clic en "Cancel" y continuar editando.

Detrás de este comportamiento se encuentra el guard `CanDeactivate` del enrutador.
Los guards te dan la oportunidad de limpiar o pedir permiso al usuario antes de navegar fuera de la vista actual.

Los botones de `Admin` y `Login` muestran otras capacidades del enrutador que se cubrirán más adelante en la guía.

{@a getting-started}

## Hito 1: Primeros pasos

Comienza con una versión básica de la app que navega entre dos vistas vacías.

<div class="lightbox">
  <img src='generated/images/guide/router/router-1-anim.gif' alt="App en acción">
</div>

{@a import}

Genera una aplicación de muestra con Angula CLI.

<code-example language="none" class="code-shell">
  ng new angular-router-sample
</code-example>

### Definir Rutas

El enrutador debe de configurarse con una lista de definiciones de rutas.

Cada definición traduce el objeto [Ruta](api/router/Route) el cual tiene dos cosas: un `path`, la URL para esta ruta; y un `component`, el componente asociado con esta ruta.

El enrutador se basa en su registro de definiciones cuando cambia la URL del navegador o cuando la aplicación le dice al enrutador que navegue por una ruta.

La primera ruta hace lo siguiente:

- Cuando la URL del navegador cambia para coincidir con la ruta de `/crisis-center`, el enrutador activa una instancia de `CrisisListComponent` y muestra su vista.

- Cuando la aplicación solicita a la navegación la ruta `/crisis-center`, el enrutador activa una instancia de `CrisisListComponent`, muestra su vista, y actualiza la dirección URL del navegador y el historial con la URL de esa ruta.

La primera configuración define una matriz de dos elementos con las rutas mínimas que conducen a `CrisisListComponent` y `HeroListComponent`.

Genera los componentes `CrisisList` y `HeroList` para que el enrutador tenga algo que renderizar.

<code-example language="none" class="code-shell">
  ng generate component crisis-list
</code-example>

<code-example language="none" class="code-shell">
  ng generate component hero-list
</code-example>

Remplaza el contenido de cada componente con el HTML de ejemplo que se muestra a continuación.

<code-tabs>

  <code-pane header="src/app/crisis-list/crisis-list.component.html" path="router/src/app/crisis-list/crisis-list.component.1.html">

  </code-pane>

  <code-pane header="src/app/hero-list/hero-list.component.html" path="router/src/app/hero-list/hero-list.component.1.html" region="template">

  </code-pane>

</code-tabs>

### Registrar el `Router` y las `Rutas`

Para utilizar el `Router`, primero debes de registrar `RouterModule` del paquete `@angular/router`.
Definir una matriz de rutas, `appRoutes`, y pasarlas al método `RouterModule.forRoot()`.
El método `RouterModule.forRoot()` devuelve un módulo que contiene el proveedor de servicios del `Router` configurado, además de otros proveedores que requiere la biblioteca del enrutador.
Una vez que inicia la aplicación, el `Enrutador` realiza la navegación inicial basada en la URL actual del navegador.

<div class="alert is-important">

**Nota:** El método `RouterModule.forRoot()` es un patrón usado para registrar los proveedores de toda la aplicación. Puedes encontrar más información sobre los proveedores de la aplicación en la guía de [servicios Singleton](guide/singleton-services#forRoot-router).

</div>

<code-example path="router/src/app/app.module.1.ts" header="src/app/app.module.ts (first-config)" region="first-config"></code-example>

<div class="alert is-helpful">

Agregar el `RouterModule` configurado al `AppModule` es suficiente para una configuración mínima de las rutas.
Sin embargo, a medida que la aplicación crece, puedes realizar una [refactorización de la configuración del enrutador](#refactor-the-routing-configuration-into-a-routing-module) en un archivo separado y crear un [Módulo de Rutas](#routing-module).
Un módulo de rutas es un tipo especial de `Service Module` dedicado al enrutamiento.

</div>

Registrando el `RouterModule.forRoot()` en un arreglo `AppModule` `imports` hace que el servicio de `Router` este disponible en cualquier parte de la aplicación.

{@a shell}

### Agregar la salida del enrutador

La raíz `AppComponent` es la carcaza de la aplicación. Tiene un título, una barra de navegación con dos enlaces, y una salida del enrutador donde el enrutador renderiza los componentes.

<div class="lightbox">
  <img src='generated/images/guide/router/shell-and-outlet.png' alt="Carcaza">
</div>

La salida del enrutador sirve como contenedor donde los componentes enrutados son renderizados.

{@a shell-template}

La plantilla correspondiente al componente se ve así:

<code-example path="router/src/app/app.component.1.html" header="src/app/app.component.html"></code-example>

{@a wildcard}

### Definir una ruta comodín

Hasta ahora has creado dos rutas en la app, una a `/crisis-center` y la otra a `/heroes`.
Cualquier otra URL causará que el enrutador arroje un error y bloquee la app.

Agrega una ruta comodín para interceptar URLs inválidas y manejarlas con elegancia.
Una ruta comodín tiene una ruta que consta de dos asteriscos.
Coincide con cada URL.
Por lo tanto, el enrutador selecciona esta ruta comodín si no coincide con otra ruta en la configuración.
Una ruta comodín puede navegar a un componente personalizado de "404 Not Found" o [redireccionar](#redirect) a otra ruta existente.

<div class="alert is-helpful">

El enrutador selecciona la ruta con la estrategia [_primera coincidencia gana_](/guide/router#example-config).
Dado que la ruta comodín es la ruta menos específica, colóquela en el último lugar en la configuración de la ruta.

</div>

Para probar esta funcionalidad, agrega un botón a `RouterLink` a la platilla `HeroListComponent` y establece un vínculo a una ruta aún no existente llamada `"/sidekicks"`.

<code-example path="router/src/app/hero-list/hero-list.component.1.html" header="src/app/hero-list/hero-list.component.html (excerpt)"></code-example>

La aplicación fallará si el usuario hace clic en ese botón porque aún no hemos definido la ruta `"/sidekicks"`.

En lugar de agregar la ruta `"/sidekicks"`, define una ruta `comodín` y haz que navegue hasta el componente `PageNotFoundComponent`.

<code-example path="router/src/app/app.module.1.ts" header="src/app/app.module.ts (wildcard)" region="wildcard"></code-example>

Crea el componente `PageNotFoundComponent` para que se muestre, cuando los usuarios visiten URLs no válidas.

<code-example language="none" class="code-shell">
  ng generate component page-not-found
</code-example>

<code-example path="router/src/app/page-not-found/page-not-found.component.html" header="src/app/page-not-found.component.html (404 component)"></code-example>

Ahora cuando el usuario visita `/sidekicks`, o cualquier otra URL no válida, el navegador muestra "Page not found".
La barra de direcciones del navegador sigue apuntando a la URL no válida.

{@a redirect}

### Configurar redireccionamientos

Cuando se inicia la aplicación, la URL inicial en la barra del navegador es la predeterminada:

<code-example>
  localhost:4200
</code-example>

Esto no coincide con ninguna de las rutas codificadas, lo que significa que el enrutador pasa la ruta comodín y muestra el componente `PageNotFoundComponent`.

La aplicación necesita una ruta predeterminada a una página válida.
La página predeterminada de esta aplicación es la lista de héroes.
La aplicación debería de navegar ahí como si el usuario hubiera hecho clic en el enlace "Héroes" ó hubiera escrito `localhost:4200/heroes` en la barra de direcciones.

Agrega una ruta de `redireccionamiento` que traduzca la URL inicial relativa (`''`) hacia la ruta predeterminada deseada (`/heroes`).

Agrega la ruta predeterminada en algún lugar _por encima_ de la ruta comodín.
Es justo encima de la ruta comodín, en el siguiente extracto que muestra las `appRoutes` completas para este hito.

<code-example path="router/src/app/app-routing.module.1.ts" header="src/app/app-routing.module.ts (appRoutes)" region="appRoutes"></code-example>

La barra de direcciones del navegador muestra `.../heroes` como si se hubiera navegado allí directamente.

Una ruta de redireccionamiento requiere una propiedad `pathMatch` para decirle al enrutador como hacer coincidir la URL con la ruta.
En esta aplicación, el enrutador debe seleccionar la ruta de `HeroListComponent` solamente cuando la _URL completa_ coincida `''`, así que configura el `pathMatch` con el valor de `'full'`.

{@a pathmatch}

<div class="callout is-helpful">

  <header>Enfocándose en pathMatch</header>

Técnicamente, `pathMatch = 'full'` da como resultado un acierto de ruta cuando los segmentos _restantes_ no coincidentes de la URL coinciden con `''`.
En este ejemplo, el redireccionamiento está en una ruta de nivel superior, por lo que la URL _restante_ y la URL _completa_ son la misma cosa.

El otro posible valor de `pathMatch` es `'prefix'`, que le dice al enrutador que coincida con la ruta de redireccionamiento cuando la URL restante comience con el prefijo de la ruta de redireccionamiento.
Esto no se aplica a esta aplicación de muestra porque si el valor de `pathMatch` fuera `'prefix'`, cada URL coincidiría con `''`.

Intenta establecer el `'prefix'` y haz clic en el botón `Go to sidekicks`.
Como es una URL incorrecta, deberías de ver la página de "Page not found".
En cambio, todavía estás en la página de "Héroes".
Ingresa una URL incorrecta en la barra de direcciones del navegador.
Serás redireccionado instantáneamente hacia `/heroes`.
Cada URL, correcta o incorrecta, que entra en esta definición de ruta es una coincidencia.

La ruta predeterminada debería redirigir al componente `HeroListComponent` solo cuando la URL completa sea `''`.
Recuerda restaurar la redirección de `pathMatch = 'full'`.

Más información en Victor Savkin's
[Publicar en redireccionamientos](http://vsavkin.tumblr.com/post/146722301646/angular-router-empty-paths-componentless-routes).

</div>

### Resumen del Hito 1

Tu aplicación de muestra puede cambiar entre dos vistas cuando el usuario hace clic en un enlace.

El hito 1 ha cubierto cómo hacer lo siguiente:

- Cargar la librería del enrutador.
- Agregaste una barra de navegación a la coraza de la plantilla con las etiquetas de anclaje, y las directivas `routerLink` y `routerLinkActive`.
- Agregaste un `router-outlet` a la coraza de la plantilla donde se muestran las vistas.
- Configuraste el módulo del enrutador con `RouterModule.forRoot()`.
- Estableciste el enrutador para componer las URLs del navegador HTML5.
- Manejo de rutas no válidas con la ruta `comodín`.
- Navegar a la ruta predeterminada cuando la aplicación se inicie con una ruta vacía.

La estructura de la aplicación inicial se ve así:

<div class='filetree'>

  <div class='file'>
    angular-router-sample
  </div>

  <div class='children'>

    <div class='file'>
      src
    </div>

    <div class='children'>

      <div class='file'>
        app
      </div>

      <div class='children'>

        <div class='file'>
          crisis-list
        </div>

        <div class='children'>

          <div class='file'>

            crisis-list.component.css

          </div>

          <div class='file'>

            crisis-list.component.html

          </div>

          <div class='file'>

            crisis-list.component.ts

          </div>

        </div>

        <div class='file'>
          hero-list
        </div>

        <div class='children'>

          <div class='file'>

            hero-list.component.css

          </div>

          <div class='file'>

            hero-list.component.html

          </div>

          <div class='file'>

            hero-list.component.ts

          </div>

        </div>

        <div class='file'>
          page-not-found
        </div>

        <div class='children'>

          <div class='file'>

            page-not-found.component.css

          </div>

          <div class='file'>

            page-not-found.component.html

          </div>

          <div class='file'>

            page-not-found.component.ts

          </div>

        </div>

        <div class='file'>
          app.component.css
        </div>

        <div class='file'>
          app.component.html
        </div>

        <div class='file'>
          app.component.ts
        </div>

        <div class='file'>
          app.module.ts
        </div>

      </div>

      <div class='file'>
        main.ts
      </div>

      <div class='file'>
        index.html
      </div>

      <div class='file'>
        styles.css
      </div>

      <div class='file'>
        tsconfig.json
      </div>

    </div>

    <div class='file'>
      node_modules ...
    </div>

    <div class='file'>
      package.json
    </div>

  </div>

</div>

Aquí están los archivos de este hito.

<code-tabs>

  <code-pane header="app.component.html" path="router/src/app/app.component.1.html">

  </code-pane>

  <code-pane header="app.module.ts" path="router/src/app/app.module.1.ts">

  </code-pane>

  <code-pane header="hero-list/hero-list.component.html" path="router/src/app/hero-list/hero-list.component.1.html">

  </code-pane>

  <code-pane header="crisis-list/crisis-list.component.html" path="router/src/app/crisis-list/crisis-list.component.1.html">

  </code-pane>

  <code-pane header="page-not-found/page-not-found.component.html" path="router/src/app/page-not-found/page-not-found.component.html">

  </code-pane>

  <code-pane header="index.html" path="router/src/index.html">

  </code-pane>

</code-tabs>

{@a routing-module}

## Hito 2: _Módulo de enrutamiento_

Este hito muestra como configurar un módulo de propósito especial llamado _Módulo de Enrutamiento_, el cual contiene la configuración de enrutamiento de tu aplicación.

El Módulo de Enrutamiento contiene varias características:

- Separa las responsabilidades de enrutamiento de otras responsabilidades de la aplicación.
- Provee de un módulo para reemplazar o remover al probar la aplicación.
- Provee un espacio específico para los proveedores de servicios de enrutamiento como guards y resolvers.
- No declara componentes.

{@a integrate-routing}

### Integrar el enrutamiento con tu aplicación

La aplicación de enrutamiento de muestra no incluye un enrutador preestablecido.
Cuando uses el [Angular CLI](cli) para crear un proyecto que use el enrutador, debes de establecer la opción de `--routing` para el proyecto o la aplicación, y para cada NgModule.
Cuando tu crear o inicializas un proyecto nuevo (usando el comando de CLI [`ng new`](cli/new)) o una nueva aplicación (usando el comando [`ng generate app`](cli/generate)), especifica la opción de `--routing`.
Esto le dirá al CLI que incluya el paquete de npm `@angular/router` y cree un archivo llamado `app-routing.module.ts`.
Puedes usar ese enrutador en cualquier NgModule que agregues a tu proyecto ó aplicación.

Por ejemplo, el siguiente comando genera un NgModule que puede usar ese enrutador.

```sh
ng generate module my-module --routing
```

Esto crea un archivo separado llamado `my-module-routing.module.ts` para almacenar las rutas del NgModule.
El archivo incluye un objeto de `Ruta` vacía que puedes llenar con rutas de diferentes componentes y NgModules.

{@a routing-refactor}
{@a refactor-the-routing-configuration-into-a-routing-module}

### Integrar el enrutamiento con tu aplicación

Crea un módulo llamado `AppRouting` en el directorio `/app` para guardar la configuración del enrutador.

<code-example language="none" class="code-shell">
  ng generate module app-routing --module app --flat
</code-example>

Importa los símbolos `CrisisListComponent`, `HeroListComponent`, y `PageNotFoundComponent` tal y como lo hiciste en `app.module.ts`.
A continuación mueve la importación del `Ennrutador` y la configuración de enrutamiento, incluyendo `RouterModule.forRoot()`, dentro de este módulo de enrutamiento.

Vuelve a exportar el `RouterModule` de Angular agregándola al array de `exports` del módulo.
Al volver a exportar el `RouterModule` aquí, los componentes declarados en `AppModule` tienen acceso a las directivas del enrutador como `RouterLink` y `RouterOutlet`.

Después de estos pasos, el archivo debería de verse así:

<code-example path="router/src/app/app-routing.module.1.ts" header="src/app/app-routing.module.ts"></code-example>

A continuación, actualiza el archivo `app.module.ts` removiendo el `RouterModule.forRoot` en el array de `imports`.

<code-example path="router/src/app/app.module.2.ts" header="src/app/app.module.ts"></code-example>

<div class="alert is-helpful">

Después, esta guía te mostrará como crear [múltiples módulos de enrutamiento](#heroes-functionality) e importarlos [en el orden correcto](#routing-module-order).

</div>

La aplicación continúa trabajando exactamente igual, y ahora puedes usar el `AppRoutingModule` como centro para mantener futuras configuraciones de enrutamiento.

{@a why-routing-module}

### Beneficios de un módulo de enrutamiento

El módulo de enrutamiento, usualmente es llamado `AppRoutingModule`, reemplaza la configuración de enrutamiento de la raíz o el módulo de funciones.

El módulo de enrutamiento es muy útil cuando tu aplicación crece y cuando la configuración incluye servicios de guards y resolvers especializados.

Algunos desarrolladores omiten el módulo de enrutamiento cuando la configuración es mínima y combinan la configuración de enrutamiento directamente en el módulo complementario (por ejemplo, `AppModule`).

La mayoria de las aplicaciónes deberian de implementar un módulo de enrutamiento para mantener la coherencia.
Mantiene el código limpio cuando la configuración se vuelve compleja.
Esto facilita las pruebas en el módulo de funciones.
Su existencia llama la atención sobre el hecho de que un módulo está enrutado.
Es aquí donde los desarrolladores esperan encontrar y expandir la configuración de enrutamiento.

{@a heroes-feature}

## Hito 3: Funciones de los Héroes

Este hito cubre lo siguiente:

- Organizar la aplicación y las rutas en áreas funcionales usando los módulos.
- Navegar imperativamente de un componente a otro.
- Pasar la información requerida u opcional en parámetros de rutas.

Esta aplicación de muestra recrea la funcionalidad de los héroes en la sección de "Services" del [Tutorial Tour de Héroes](tutorial/toh-pt4 "Tour de Héroes: Servicios"), y reúsa mucho del código de <live-example name="toh-pt4" title="Tour de Héroes: Codigo de ejemplo de servicios"></live-example>.

<!-- KW - this gif isn't ideal for accessibility. Would like to remove it.-->
<!-- Here's how the user will experience this version of the app:


<div class="lightbox">
  <img src='generated/images/guide/router/router-2-anim.gif' alt="App in action">
</div> -->

Una aplicación típica tiene múltiples áreas funcionales, cada una dedicada a un propósito comercial particular con su propia carpeta.

Esta sección muestra como refactorizar la aplicación en diferentes módulos funcionales, impórtalos en el módulo principal y navegar entre ellos.

{@a heroes-functionality}

### Agregar funcionalidades de héroes

Sigue los siguientes pasos:

- Para administrar los héroes, crea un `HeroesModule` con enrutador en la carpeta de héroes y regístralo en la raíz `AppModule`.

<code-example language="none" class="code-shell">
  ng generate module heroes/heroes --module app --flat --routing
</code-example>

- Mueve el contenido de la carpeta `hero-list` que se encuentra en la carpeta `app` a la carpeta de `heroes`.
- Copia el contenido de `heroes/heroes.component.html` desde el <live-example name="toh-pt4" title="Tour de Héroes: Codigo de ejemplo de servicios">tutorial de "Services"</live-example> en la plantilla `hero-list.component.html`.

  - Cambia la etiqueta `<h2>` con `<h2>HEROES</h2>`.
  - Borra el componente `<app-hero-detail>` que está en la parte inferior de la plantilla.

- Copia el contenido de `heroes/heroes.component.css` del ejemplo en vivo al archivo `hero-list.component.css`.
- Copia el contenido de `heroes/heroes.component.ts` del ejemplo en vivo al archivo `hero-list.component.ts`.

  - Cambia el nombre de la clase del componente a `HeroListComponent`.
  - Cambia el `selector` a `app-hero-list`.

<div class="alert is-helpful">

Los selectores no son requeridos por los componentes de enrutamiento porque los componentes son insertados dinámicamente cuando la página es renderizada. Sin embargo, son útiles para identificarlos en los elementos HTML del DOM.

</div>

- Copia la carpeta `hero-detail`, los archivos `hero.ts`, `hero.service.ts` y `mock-heroes.ts` dentro de la sub carpeta `heroes`.
- Copia el archivo `message.service.ts` en la carpeta `src/app`.
- Actualiza la ruta relativa del import de `message.service` en el archivo `hero.service.ts`.

A continuación, actualiza los metadatos de `HeroesModule`.

- Importe y agrega `HeroDetailComponent` y `HeroListComponent` al array de `declarations` en el `HeroesModule`.

<code-example path="router/src/app/heroes/heroes.module.ts" header="src/app/heroes/heroes.module.ts"></code-example>

La estructura de archivos del administrador de héroes es la siguiente:

<div class='filetree'>

  <div class='file'>
    src/app/heroes
  </div>

  <div class='children'>

    <div class='file'>
      hero-detail
    </div>

      <div class='children'>

        <div class='file'>
          hero-detail.component.css
        </div>

        <div class='file'>
          hero-detail.component.html
        </div>

        <div class='file'>
          hero-detail.component.ts
        </div>

      </div>

    <div class='file'>
      hero-list
    </div>

      <div class='children'>

        <div class='file'>
          hero-list.component.css
        </div>

        <div class='file'>
          hero-list.component.html
        </div>

        <div class='file'>
          hero-list.component.ts
        </div>

      </div>

    <div class='file'>
      hero.service.ts
    </div>

    <div class='file'>
      hero.ts
    </div>

    <div class='file'>
      heroes-routing.module.ts
    </div>

    <div class='file'>
      heroes.module.ts
    </div>

    <div class='file'>
      mock-heroes.ts
    </div>

    </div>

  </div>

</div>

{@a hero-routing-requirements}

#### Requisitos del enrutamiento de funciones de Héroe

Las funcionalidades de los héroes tienen dos componentes que interactúan, la lista de héroes y el detalle de los héroes.
Cuando navegas a la vista de lista, se obtiene una lista de héroes y la muestra.
Cuando haces clic en un héroe, el detalle de la vista debe de mostrar ese héroe en particular.

Puedes decirle a la vista de detalle cuál es el héroe que mostrara, incluyendo el `id` del héroe seleccionado en la URL de la ruta.

Debes de importar los componentes de héroe a su nueva ubicación en la carpeta `src/app/heroes/` y definir las dos rutas del héroe.

Ahora que tienes las rutas del módulo de `Heroes`, registralas en el `Router` mediante el módulo `RouterModule` como lo hiciste anteriormente en `AppRoutingModule`, con una diferencia importante.

En el `AppRoutingModule`, usaste el metodo estatico `RouterModule.forRoot()` para registrar las rutas y el nivel de aplicación de los proveedores de servicio.
En el módulo de funciones usa el método estatico `forChild()`.

<div class="alert is-helpful">

Solo llama `RouterModule.forRoot()` en la raíz `AppRoutingModule` (o el `AppModule` si es donde registraras las rutas de nivel superior de la aplicación).
En cualquier otro módulo, debes llamar el metodo `RouterModule.forChild()` para registrar las rutas adicionales.

</div>

El módulo `HeroesRoutingModule` se ve así:

<code-example path="router/src/app/heroes/heroes-routing.module.1.ts" header="src/app/heroes/heroes-routing.module.ts"></code-example>

<div class="alert is-helpful">

Considera darle a cada módulo de funciones su propio archivo de configuraciones de rutas.
Aunque las funciones de las rutas son mínimas, estas tienen la tendencia de crecer y volverse más complejas, incluso en aplicaciones pequeñas.

</div>

{@a remove-duplicate-hero-routes}

#### Remover rutas de héroes duplicadas

Las rutas de los héroes actualmente están definidas en dos lugares: en el `HeroesRoutingModule` a través del `HeroesModule` y en el `AppRoutingModule`.

Las rutas proporcionadas por los módulos de funciones se combinan en los módulos de rutas importados por el enrutador.
Esto te permite continuar definiendo los módulos de funciones de rutas sin tener que modificar la configuración de la ruta principal.

Eliminar la importación del `HeroListComponent` y la ruta `/heroes` del archivo `app-routing.module.ts`.

Deja las rutas predeterminadas y las rutas de comodín, ya que todavía se están usando en el nivel superior de la aplicación.

<code-example path="router/src/app/app-routing.module.2.ts" header="src/app/app-routing.module.ts (v2)"></code-example>

{@a merge-hero-routes}

#### Remover las declaraciones de los héroes

Debido a que `HeroesModule` provee el `HeroListComponent`, puedes eliminarlo de array `declarations` en el `AppModule`'s.
Ahora que tienes un `HeroesModule` separado, puedes mejorar las funcionalidades del héroe con más componentes y diferentes rutas.

Después de estos pasos el `AppModule` debería verse así:

<code-example path="router/src/app/app.module.3.ts" header="src/app/app.module.ts" region="remove-heroes"></code-example>

{@a routing-module-order}

### Orden de importación de módulos

Observa que en el array de `imports` del módulo, `AppRoutingModule` es el último y viene _después_ del `HeroesModule`.

<code-example path="router/src/app/app.module.3.ts" region="module-imports" header="src/app/app.module.ts (module-imports)"></code-example>

El orden de configuración de la ruta es importante porque el enrutador acepta la primera ruta que coincida con la ruta de navegación solicitada.

Cuando todas las rutas están en un `AppRoutingModule`, tu pones las rutas predeterminadas y [comodín](#wildcard) al final, después de la ruta de `/heroes`, para que el enrutador tenga la oportunidad de encontrar la URL que coincide con la ruta de `/heroes` _antes_ de llegar a la ruta comodín y navegar a la página "Page not found".

Cada módulo de enrutamiento aumenta la configuración de la ruta en el orden de importación.
Si pones `AppRoutingModule` primero, la ruta comodín se encontraría _antes_ que la ruta de héroes.
La ruta comodín &mdash;coincide con _cada_ URL&mdash; e interceptara el intento de navegar a la ruta de héroe.

<div class="alert is-helpful">

Invierte el orden del módulo de enrutamiento para ver que al hacer un clic en el enlace de héroes serás llevado a la página "Page not found".
Obtén más información acerca de como inspeccionar la configuración del enrutador en tiempo de ejecución [a continuación](#inspect-config "Inspeccionar la configuración del enrutador").

</div>

### Parámetros de rutas

{@a route-def-with-parameter}

#### Definición de una ruta con parámetros

Regresa al `HeroesRoutingModule` y mira las definiciones de las rutas de nuevo.
La ruta hacia `HeroDetailComponent` tiene un token `:id` en la ruta.

<code-example path="router/src/app/heroes/heroes-routing.module.1.ts" header="src/app/heroes/heroes-routing.module.ts (excerpt)" region="hero-detail-route"></code-example>

El token `:id` crea un espacio en la ruta para un parámetro de ruta.
En este caso, esta configuración cause que el enrutador inserte el `id` de un héroe en ese espacio.

Si llamas la ruta para navegar al componente de detalle y muestras "Magneta", esperas que el `id` del héroe aparezca en la URL del navegador como esta:

<code-example format="nocode">
  localhost:4200/hero/15

</code-example>

Si un usuario escribe esa URL en la barra de direcciones de su navegador, el enrutador debería de reconocer el patrón e ir a la misma vista de detalle de "Magneta".

<div class="callout is-helpful">

<header>
  Parámetro de ruta: ¿obligatorio u opcional?
</header>

Al incrustar el token del parámetro de ruta, `:id`, en la definición de rutas es una buena opción para este escenario, ya que el `:id` es _requerido_ por el `HeroDetailComponent` y el valor de `15` en la ruta, se distingue claramente de la ruta "Magneta" a cualquier ruta para otro héroe.

</div>

{@a route-parameters}

#### Configurando los parámetros de la ruta en la vista de lista

Después de navegar a `HeroDetailComponent`, esperas ver los detalles del héroe seleccionado.
Para esto se necesitan dos piezas de información: la ruta de enrutamiento al componente y el `id` del héroe.

Por lo que, el _array de parámetros del enlace_ debe tener dos elementos: la _ruta_ del enrutador y el _parámetro de ruta_ que especifica el `id` del héroe seleccionado.

<code-example path="router/src/app/heroes/hero-list/hero-list.component.1.html" header="src/app/heroes/hero-list/hero-list.component.html (link-parameters-array)" region="link-parameters-array"></code-example>

El enrutador compone la URL de destino desde el array como esto: `localhost:4200/hero/15`.

El enrutador extrae el parámetro de la ruta (`id:15`) de la URL y lo proporciona a `HeroDetailComponent` a través del servicio `ActivatedRoute`.

{@a activated-route-in-action}

### `Ruta activada` en acción

Importa el `Router`, `ActivatedRoute`, y `ParamMap` del paquete del enrutador.

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.1.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (activated route)" region="imports"></code-example>

Importaremos el operador `switchMap` porque lo necesitaremos más tarde para procesar los parámetros de la ruta `Observable`.

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.3.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (switchMap operator import)" region="rxjs-operator-import"></code-example>

{@a hero-detail-ctor}

Agrega los servicios como variables privadas al constructor para que Angular las inyecte (y los haga visibles al componente).

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.3.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (constructor)" region="ctor"></code-example>

En el método `ngOnInit()`, usa el servicio de `ActivatedRoute` para recuperar los parámetros de la ruta, extrae el `id` del héroe de los parámetros y recupéralo para mostrar el héroe.

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.3.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (ngOnInit)" region="ngOnInit"></code-example>

Cuando el mapa cambie, `paramMap` tomara el parámetro `id` de los parámetros modificados.

Entonces le indicaras al `HeroService` que busque el héroe con ese `id` y devuelva el resultado de la solicitud de `HeroService`.

El operador `switchMap` hace dos cosas. Aplana el `Observable<Hero>` que devuelve `HeroService` y cancela las solicitudes pendientes anteriores.
Si el usuario vuelve a navegar hacia la ruta con el nuevo `id` mientras el `HeroService` está aun devolviendo un `id` anterior, `switchMap` descartara esa solicitud vieja y regresara el héroe para el nuevo `id`.

`AsyncPipe` maneja la subscripción al observable y la propiedad `hero` del componente se restablecerá con el héroe recuperado.

#### _ParamMap_ API

The API `ParamMap` está inspirada en la [interfaz URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams).
Proporciona métodos para manejar el acceso a los parámetros tanto para los parámetros de ruta (`paramMap`) y los parámetros de consulta (`queryParamMap`).

<table>
  <tr>
    <th>
      Miembro
    </th>

    <th>
      Descripción
    </th>

  </tr>

  <tr>
    <td>
      <code>has(name)</code>
    </td>
    <td>

    Devuelve `true` si el nombre del parámetro está en el mapa de parámetros .

    </td>

  </tr>

  <tr>
    <td>
      <code>get(name)</code>
    </td>
    <td>

    Devuelve el nombre del valor del parámetro (como `string`) si se encuentra, o `null` si el nombre del parametro no está en el mapa. Regresa el _primer_ elemento si el valor del parámetro es un array de valores.

    </td>

  </tr>

  <tr>
    <td>
      <code>getAll(name)</code>
    </td>
    <td>

    Devuelve un `string array` con el nombre del valor del parámetro si lo encuentra, o un `array` vacío si el nombre del valor del parámetro no está en el mapa. Usa `getAll` cuando un solo parámetro puede tener varios valores.

    </td>

  </tr>

  <tr>
    <td>
      <code>keys</code>
    </td>
    <td>

    Devuelve un `string array` de todos los nombres de parámetros en el mapa.

    </td>

  </tr>
</table>

{@a reuse}

#### <i>paramMap</i> observable y la reutilización de componentes

En este ejemplo, recuperarás el mapa del parámetro de ruta de un `Observable`.
Esto implica que el mapa de parámetros de ruta pueda cambiar durante la vida útil de este componente.

De manera predeterminada, el enrutador reutiliza la instancia del componente cuando vuelve a navegar componente del mismo tipo sin visitar un componente diferente. Los parámetros de la ruta puede cambiar cada vez.

Supongamos que la barra de navegación de un componente principal tiene botones "Avanzar" y "Retroceder" para desplazarse por la lista de héroes.
En cada clic navegará imperativamente al `HeroDetailComponent` con el `id` siguiente o previo.

No querrá que el enrutador elimine la instancia actual de `HeroDetailComponent` del DOM solo para volver a crearla para el próximo `id`, ya que esto renderizara la vista de nuevo.
Para una mejor UX, el enrutador reutiliza la misma instancia del componente y actualiza el parámetro.

Dado que `ngOnInit()` solo se llama una vez por cada instancia de un componente, puedes detectar cuando los parámetros de la ruta cambian _dentro de la misma instancia_ utilizando la propiedad observable `paramMap.

<div class="alert is-helpful">

Cuando se suscribe a un observable en un componente, casi siempre se da de baja esta subscripción cuando el componente se destruye.

Sin embargo, los observables de `ActivatedRoute` están entre las excepciones porque `ActivatedRoute` y sus observables están aislados del `Router`.
El `Router` destruye un componente de ruta cuando ya no se necesita junto con el `ActivatedRoute` inyectado.

</div>

{@a snapshot}
{@a snapshot-the-no-observable-alternative}

#### `snapshot`: la alternativa no observable

Esta aplicación no reutilizará el `HeroDetailComponent`.
El usuario siempre regresará a la lista de héroes para seleccionar otro héroe en la vista.
No hay manera de navegar del detalle de un héroe al detalle de otro héroe sin visitar el componente de lista.
Por lo tanto, el enrutador crea una nueva instancia `HeroDetailComponent` cada vez.

Cuando sepas con certeza que una instancia de `HeroDetailComponent` nunca será reutilizada, puedes usar un `snapshot`.

`route.snapshot` proporciona el valor inicial del mapa de parámetros de ruta.
Puedes acceder a los parámetros directamente sin subscribirse o agregar operadores observables como se muestra a continuación:

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.2.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (ngOnInit snapshot)" region="snapshot"></code-example>

<div class="alert is-helpful">

`snapshot` solo obtiene el valor inicial del mapa de parámetros con esta técnica.
Utiliza un enfoque observable `paramMap` si existe la posibilidad de que el enrutador pueda reutilizar el componente.
Este tutorial de aplicación de muestra utiliza el observable `paramMap`.

</div>

{@a nav-to-list}

### Navegando de regreso al componente lista

El botón "Retroceder" de `HeroDetailComponent` utiliza el método `gotoHeroes()` para navegar imperativamente de regreso al `HeroListComponent`.

El método `navigate()` del enrutador toma el mismo elemento _array con parámetros de enlace_ que puedes vincular a la directiva `[routerLink]`.
Este contiene la ruta a `HeroListComponent`:

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.1.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (excerpt)" region="gotoHeroes"></code-example>

{@a optional-route-parameters}

#### Parámetros de ruta: Requeridos u opcionales?

Utiliza [parámetros de ruta](#route-parameters) para especificar el valor de un parámetro requerido dentro de la URL de la ruta como la hace cuando navega al `HeroDetailComponent` para mostrar el héroe con `id` 15:

<code-example format="nocode">
  localhost:4200/hero/15

</code-example>

También puedes agregar información opcional a una solicitud de la ruta.
Por ejemplo, al regresar a la lista `hero-detail.component.ts` desde la vista de detalles del héroe, sería agradable si el héroe visto estuviera preseleccionado en la lista.

<div class="lightbox">
  <img src='generated/images/guide/router/selected-hero.png' alt="Héroe seleccionado">
</div>

Para implementar esta funcionalidad al incluir el `id` del héroe visto en la URL como parámetro opcional cuando regresas del `HeroDetailComponent`.

La información opcional también puede incluir:

- Criterios de búsqueda poco estructurados; por ejemplo: `name='wind*'`.
- Múltiples valores; por ejemplo, `after='12/31/2015' & before='1/1/2017'`&mdash;sin un orden en particula&mdash;`before='1/1/2017' & after='12/31/2015'`&mdash; en una variedad de formatos&mdash;`during='currentYear'`.

Como este tipo de parámetros no encajan fácilmente en la ruta URL, puedes usar los parámetros opcionales para transmitir información arbitrariamente compleja durante la navegación.
Los parámetros opcionales no están involucrados en los patrones de coincidencia y permiten una expresión más flexible.

El enrutador permite la navegación con parámetros opcionales, así como los parámetros de ruta requeridos.
Define los parámetros opcionales en una objeto separado _después_ de definir los parámetros de ruta requeridos.

En general, utiliza los parámetros de ruta requeridos cuando el valor es obligatorio (por ejemplo, si es necesario distinguir entre una ruta de otra): y agrega un parámetro opcional cuando el valor es opcional, complejo y/o variado.

{@a optionally-selecting}

#### Lista de Héroes: Seleccionado a un héroe opcional

Al navegar a `HeroDetailComponent` se especificó el `id` requerido del héroe-a-editar en el parámetro y se convirtió en el segundo elemento del [_array de parámetros del enlace_](#link-parameters-array).

<code-example path="router/src/app/heroes/hero-list/hero-list.component.1.html" header="src/app/heroes/hero-list/hero-list.component.html (link-parameters-array)" region="link-parameters-array"></code-example>

El enrutador incrusta el valor `id` en la URL de la navegación porque está definido en los parámetros de ruta como `:id` en la ruta `path`:

<code-example path="router/src/app/heroes/heroes-routing.module.1.ts" header="src/app/heroes/heroes-routing.module.ts (hero-detail-route)" region="hero-detail-route"></code-example>

Cuando el usuario hace clic en el botón Atras, el `HeroDetailComponent` construye otro _array de parámetros de enlace_ que se utiliza para navegar de regreso al `HeroListComponent`.

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.1.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (gotoHeroes)" region="gotoHeroes"></code-example>

Este array carece de parámetros de ruta porque previamente no era necesario enviar información al `HeroListComponent`.

Ahora, enviamos el `id` del héroe actual con la solicitud de navegación por eso el `HeroListComponent` puede destacar el héroe en esta lista.

Enviar el `id` con un objeto que contiene un parámetro `id` opcional. Para fines de demostración, hay un parámetro basura extra (`foo`) en el objeto que el `HeroListComponent` debería de ignorar. Aquí está la declaración de la navegación:

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.3.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (go to heroes)" region="gotoHeroes"></code-example>

La aplicación aún funciona. Haciendo clic en "Retroceso" regresa a la vista de lista del héroe.

Mira la barra de direcciones del navegador.

Debería de verse algo como esto, dependiendo de donde lo ejecutes:

<code-example language="bash">
  localhost:4200/heroes;id=15;foo=foo

</code-example>

El valor `id` aparece en la URL como (`;id=15;foo=foo`), no en la ruta de la URL.
La ruta para "Heroes" no tiene un token `:id`.

Los parámetros de ruta opcionales no están separador por un "?" o "&" como estarían en la cadena de consulta de la URL.
Estan separador por punto y coma ";".
Esta es la notación de una matriz URL.

<div class="alert is-helpful">

La notación de una matriz URL es una idea introducida por primera vez en [propuesta de 1996](http://www.w3.org/DesignIssues/MatrixURIs.html) por el fundador de la web, Tim Berners-Lee.

Aunque la notación de matriz nunca se incorporó al estándar de HTML, es legal y se hizo popular entre los sistemas de enrutamiento del navegador como una forma de aislar los parámetros que pertenecen a las rutas padres e hijas.
Como tal, el Enrutador proporciona soporte a la notación de matriz en todos los navegadores.

</div>

{@a route-parameters-activated-route}

### Parámetros de ruta en el servicio `ActivatedRoute`

En el estado actual del desarrollo, la lista de héroes no ha cambiado.
No se resalta ninguna fila de héroe.

El `HeroListComponent` necesita el código que espera parámetros.

Previamente, cuando navegamos desde el `HeroListComponent` hacia el `HeroDetailComponent`, es necesario suscribirse al mapa de parámetros de ruta `Observable` y ponerlo a disposición del `HeroDetailComponent` en el servicio de `ActivatedRoute`.
Este servicio fue inyectado en el constructor de `HeroDetailComponent`.

Esta vez navegarás en dirección opuesta, desde el `HeroDetailComponent` al `HeroListComponent`.

Primero, extenderemos la declaración de importación del enrutador para incluir el servicio de `ActivatedRoute`.

<code-example path="router/src/app/heroes/hero-list/hero-list.component.ts" header="src/app/heroes/hero-list/hero-list.component.ts (import)" region="import-router"></code-example>

Importa el operador `switchMap` para realizar una operación en el mapa de parámetros de ruta `Observable`.

<code-example path="router/src/app/heroes/hero-list/hero-list.component.ts" header="src/app/heroes/hero-list/hero-list.component.ts (rxjs imports)" region="rxjs-imports"></code-example>

Inyecta el `ActivatedRoute` en el contructor `HeroListComponent`.

<code-example path="router/src/app/heroes/hero-list/hero-list.component.ts" header="src/app/heroes/hero-list/hero-list.component.ts (constructor and ngOnInit)" region="ctor"></code-example>

La propiedad `ActivatedRoute.paramMap` es un mapa de ruta de parámetros `Observable`.
`paramMap` emite un nuevo mapa de valores que incluye el `id` cuando el usuario navega al componente.
En `ngOnInit()` te suscribes a esos valores, estableces el `selectedId` y obtienes los héroes.

Actualiza la plantilla con un [enlace de clase](guide/attribute-binding#class-binding).
El enlace agrega la clase CSS `selected` cuando la comparación devuelve `true` y la elimina cuando es `false`.
Busca dentro de la etiqueta repetida `<li>` como se muestra:

<code-example path="router/src/app/heroes/hero-list/hero-list.component.html" header="src/app/heroes/hero-list/hero-list.component.html"></code-example>

Agrega algunos estilos que se aplicaran cuando un elemento de la lista es seleccionada.

<code-example path="router/src/app/heroes/hero-list/hero-list.component.css" region="selected" header="src/app/heroes/hero-list/hero-list.component.css"></code-example>

Cuando el usuario navega desde la lista de héroes al héroe "Magneta" y viceversa, "Magneta" aparece seleccionada:

<div class="lightbox">
  <img src='generated/images/guide/router/selected-hero.png' alt="Selected List">
</div>

El parámetro de ruta opcional `foo` es inofensivo y el enrutador continúa ignorándolo.

{@a route-animation}

### Agregando animaciones enrutables

Esta sección muestra como agregar algunas [animations](guide/animations) al `HeroDetailComponent`.

Primero, importa el `BrowserAnimationsModule` y agrégalo al array de `imports`:

<code-example path="router/src/app/app.module.ts" header="src/app/app.module.ts (animations-module)" region="animations-module"></code-example>

A continuación, agrega un objeto `data` a las rutas del `HeroListComponent` y `HeroDetailComponent`.
Las transiciones se basan en `states` y se utilizan los datos de `animation` desde el enrutador que proporciona un `state` con nombre `animation` para las transiciones.

<code-example path="router/src/app/heroes/heroes-routing.module.2.ts" header="src/app/heroes/heroes-routing.module.ts (animation data)"></code-example>

Crea un archivo `animations.ts` en la carpeta raíz `src/app/`. El contenido se ve así:

<code-example path="router/src/app/animations.ts" header="src/app/animations.ts (excerpt)"></code-example>

Este archivo realiza lo siguiente:

- Importa los símbolos de animación que crean los disparados de la animación, controlan el estado, y administran la transición entre estados.

- Exporta una constante llamada `slideInAnimation` y establece un disparador de animación llamado `routeAnimation`.

- Define una transición cuando se alterna entre las rutas `heroes` y `hero` que hace una animación para el componente desde la izquierda de la pantalla cuando entra a la vista de aplicación (`:enter`), la otra realiza una animación del componente a la derecha cuando sale de la vista de la aplicación (`:leave`).

De vuelta al `AppComponent`, importa el token `RouterOutlet` desde el paquete `@angular/router` y el `slideInAnimation` de `'./animations.ts`.

Agrega el array `animations` a los metadatos de `@Component` que contiene `slideInAnimation`.

<code-example path="router/src/app/app.component.2.ts" header="src/app/app.component.ts (animations)" region="animation-imports"></code-example>

Para usar las animaciones enrutables, envuelve el `RouterOutlet` dentro de un elemento, usa el disparador `@routeAnimation` y enlázalo al elemento.

Para la transición de `@routeAnimation` a un estado desactivado, proporciona `data` al `ActivatedRoute`.
El `RouterOutlet` es expuesto como una variable de la plantilla `outlet`, por lo que enlaza la referencia a la salida del enrutador.
Este ejemplo usa una variable de `routerOutlet`.

<code-example path="router/src/app/app.component.2.html" header="src/app/app.component.html (router outlet)"></code-example>

La propiedad `@routeAnimation` está vinculada a `getAnimationData()` con la referencia de `routerOutlet` proporciona, el siguiente paso es definir la función en el `AppComponent`.
La función `getAnimationData()` devuelve una propiedad animación desde el `data` proporcionado por la `ActivatedRoute`. La propiedad `animation` coincide con el nombre de `transition` que usaste en `slideInAnimation` definido en `animations.ts`.

<code-example path="router/src/app/app.component.2.ts" header="src/app/app.component.ts (router outlet)" region="function-binding"></code-example>

Cuando se cambia entre las dos rutas, el `HeroDetailComponent` y `HeroListComponent` entran desde la izquierda cuando se enrutan y se deslizan hacia la derecha cuando navegas fuera de la ruta.

{@a milestone-3-wrap-up}

### Resumen del hito 3

En esta sección hemos cubierto lo siguiente:

- Organizamos la aplicación en áreas de funcionalidad.
- Navegación imperativa de un componente a otro.
- Pasamos información en los parámetros de ruta y las subscribimos a un componente.
- Importamos el área de funciones NgModule en el `AppModule`.
- Aplicamos animaciones enrutables basadas en la página.

Después de estos cambios, la estructura de carpetas es la siguiente:

<div class='filetree'>

  <div class='file'>
    angular-router-sample
  </div>

  <div class='children'>

    <div class='file'>
      src
    </div>

    <div class='children'>

      <div class='file'>
        app
      </div>

      <div class='children'>

        <div class='file'>
          crisis-list
        </div>

          <div class='children'>

            <div class='file'>
              crisis-list.component.css
            </div>

            <div class='file'>
              crisis-list.component.html
            </div>

            <div class='file'>
              crisis-list.component.ts
            </div>

          </div>

        <div class='file'>
          heroes
        </div>

        <div class='children'>

          <div class='file'>
            hero-detail
          </div>

            <div class='children'>

              <div class='file'>
                hero-detail.component.css
              </div>

              <div class='file'>
                hero-detail.component.html
              </div>

              <div class='file'>
                hero-detail.component.ts
              </div>

            </div>

          <div class='file'>
            hero-list
          </div>

            <div class='children'>

              <div class='file'>
                hero-list.component.css
              </div>

              <div class='file'>
                hero-list.component.html
              </div>

              <div class='file'>
                hero-list.component.ts
              </div>

            </div>

          <div class='file'>
            hero.service.ts
          </div>

          <div class='file'>
            hero.ts
          </div>

          <div class='file'>
            heroes-routing.module.ts
          </div>

          <div class='file'>
            heroes.module.ts
          </div>

          <div class='file'>
            mock-heroes.ts
          </div>

        </div>

        <div class='file'>
          page-not-found
        </div>

        <div class='children'>

          <div class='file'>

            page-not-found.component.css

          </div>

          <div class='file'>

            page-not-found.component.html

          </div>

          <div class='file'>

            page-not-found.component.ts

          </div>

        </div>

      </div>

      <div class='file'>
        animations.ts
      </div>

      <div class='file'>
        app.component.css
      </div>

      <div class='file'>
        app.component.html
      </div>

      <div class='file'>
        app.component.ts
      </div>

      <div class='file'>
        app.module.ts
      </div>

      <div class='file'>
        app-routing.module.ts
      </div>

      <div class='file'>
        main.ts
      </div>

      <div class='file'>
        message.service.ts
      </div>

      <div class='file'>
        index.html
      </div>

      <div class='file'>
        styles.css
      </div>

      <div class='file'>
        tsconfig.json
      </div>

    </div>

    <div class='file'>
      node_modules ...
    </div>

    <div class='file'>
      package.json
    </div>

  </div>

</div>

Aquí están los archivos relevantes para esta versión de la aplicación de muestra.

<code-tabs>

  <code-pane header="animations.ts" path="router/src/app/animations.ts">

  </code-pane>

  <code-pane header="app.component.html" path="router/src/app/app.component.2.html">

  </code-pane>

  <code-pane header="app.component.ts" path="router/src/app/app.component.2.ts">

  </code-pane>

  <code-pane header="app.module.ts" path="router/src/app/app.module.3.ts">

  </code-pane>

  <code-pane header="app-routing.module.ts" path="router/src/app/app-routing.module.2.ts" region="milestone3">

  </code-pane>

  <code-pane header="hero-list.component.css" path="router/src/app/heroes/hero-list/hero-list.component.css">

  </code-pane>

  <code-pane header="hero-list.component.html" path="router/src/app/heroes/hero-list/hero-list.component.html">

  </code-pane>

  <code-pane header="hero-list.component.ts" path="router/src/app/heroes/hero-list/hero-list.component.ts">

  </code-pane>

  <code-pane header="hero-detail.component.html" path="router/src/app/heroes/hero-detail/hero-detail.component.html">

  </code-pane>

  <code-pane header="hero-detail.component.ts" path="router/src/app/heroes/hero-detail/hero-detail.component.3.ts">

  </code-pane>

  <code-pane header="hero.service.ts" path="router/src/app/heroes/hero.service.ts">

  </code-pane>

  <code-pane header="heroes.module.ts" path="router/src/app/heroes/heroes.module.ts">

  </code-pane>

  <code-pane header="heroes-routing.module.ts" path="router/src/app/heroes/heroes-routing.module.2.ts">

  </code-pane>

  <code-pane header="message.service.ts" path="router/src/app/message.service.ts">

  </code-pane>

</code-tabs>

{@a milestone-4}

## Hito 4: Funcionalidad del Centro de Crisis

En esta sección te mostraremos como agregar rutas hijas y usar el enrutamiento relativo en tu aplicación.

Para agregar más funcionalidades al centro de crisis actual de la aplicación, sigue unos pasos similares a los de la función de héroes:

- Crea una sub carpeta `crisis-center` en la carpeta `src/app`.
- Copia los archivos y carpetas de `app/heroes` en la nueva carpeta `crisis-center`.
- En los archivos nuevos, cambia cada mención de "hero" por "crisis", y "heroes" por "crises".
- Renombra los archivos de NgModule de `crisis-center.module.ts` y `crisis-center-routing.module.ts`.

Utiliza el mock de crisis en lugar del mock de héroes.

<code-example path="router/src/app/crisis-center/mock-crises.ts" header="src/app/crisis-center/mock-crises.ts"></code-example>

El centro de crisis resultante es una base para la introducción de un nuevo concepto&mdash;enrutamiento hijo. Puedes dejar Héroes en su estado actual como contraste con el Centro de crisis.

<div class="alert is-helpful">

De acuerdo con <a href="https://blog.8thlight.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html" title="Separación de Responsabilidad">Principio de separación de responsabilidades</a>, los cambios al Centro de Crisis no afectan el `AppModule` o las funcionalidades de otros componentes.

</div>

{@a crisis-child-routes}
{@a a-crisis-center-with-child-routes}

### Un centro de crisis con rutas hijas

Esta sección te mostrará como organizar el centro de crisis de acuerdo a los siguientes patrones recomendados para aplicaciones en Angular:

- Cada área de funcionalidad reside en su propia carpeta.
- Cada funcionalidad tiene su propio módulo de función de Angular.
- Cada área tiene su propio componente en raíz.
- Cada componente en raíz tiene su propia salida de enrutamiento y rutas hijas.
- Las áreas de funcionalidad de rutas rara vez (si alguna vez) se cruzan con otras rutas de otras funcionalidades.

Si tu aplicación tiene muchas áreas de funcionalidad, el árbol de componentes de la aplicación podría verse así:

<div class="lightbox">
  <img src='generated/images/guide/router/component-tree.png' alt="Component Tree">
</div>

{@a child-routing-component}

### Componente de enrutamiento hijo

Genera un componente `CrisisCenter` en la carpeta `crisis-center`:

<code-example language="none" class="code-shell">
  ng generate component crisis-center/crisis-center
</code-example>

Actualiza la plantilla del componente con las siguientes etiquetas:

<code-example path="router/src/app/crisis-center/crisis-center/crisis-center.component.html" header="src/app/crisis-center/crisis-center/crisis-center.component.html"></code-example>

El `CrisisCenterComponent` tiene lo siguiente en común con el `AppComponent`:

- Es la raíz del área del centro de crisis, solo que el `AppComponent` está en la raíz de toda la aplicación.
- Es una carcasa para el área de funcionalidad de la gestion de crisis, solamente que el `AppComponent` es una carcasa para administrar el flujo de trabajo de alto nivel.

Como la mayoría de las carcasas, la clase `CrisisCenterComponent` es mínima porque no tiene lógica de negocios, y su plantilla no tiene vínculos, solamente `<router-outlet>` para el componente hijo del centro de crisis.

{@a child-route-config}

### Configuración de rutas hijas

Como página anfitriona para la funcionalidad de "Crisis Center", genera un componente `CrisisCenterHome` en la carpeta `crisis-center`.

<code-example language="none" class="code-shell">
  ng generate component crisis-center/crisis-center-home
</code-example>

Actualiza la plantilla con el mensaje de bienvenida al `Crisis Center`.

<code-example path="router/src/app/crisis-center/crisis-center-home/crisis-center-home.component.html" header="src/app/crisis-center/crisis-center-home/crisis-center-home.component.html"></code-example>

Actualiza el archivo `crisis-center-routing.module.ts` que renombraste después de copiarlo del archivo `heroes-routing.module.ts`.
Esta vez, definirás las rutas hijas dentro de la ruta padre `crisis-center`.

<code-example path="router/src/app/crisis-center/crisis-center-routing.module.1.ts" header="src/app/crisis-center/crisis-center-routing.module.ts (Routes)" region="routes"></code-example>

Observa que la ruta padre `crisis-center` tiene una propiedad `children` con una única ruta que contiene el `CrisisListComponent`. La ruta `CrisisListComponent` también tiene un array `children` con dos rutas.

Estas dos rutas navegan hacia los componentes hijos del centro de crisis, `CrisisCenterHomeComponent` y `CrisisDetailComponent`, respectivamente.

Existen diferencias importantes en la forma que el enrutador trata las rutas hijas.

El enrutador muestra los componentes de estas rutas en el `RouterOutlet` de `CrisisCenterComponent`, no en el `RouterOutlet` de la carcasa de `AppComponent`.

El `CrisisListComponent` contiene la lista de crisis y un `RouterOutlet` para mostrar los componentes de ruta de `Crisis Center Home` y `Crisis Detail`.

La ruta de `Crisis Detail` es un hijo de `Crisis List`.
El enrutador [reutiliza componentes](#reuse) de forma predeterminada, por lo que el componente `Crisis Detail` será reutilizado cada vez que selecciones una crisis diferente.
Por el contrario, en la ruta de `Hero Detail` [el componente es creado de nuevo](#snapshot-the-no-observable-alternative) cada vez que seleccionas un héroe diferente de la lista de héroes.

En un nivel superior, las rutas comienza con `/` para referirse a la raíz de la aplicación.
Pero las rutas hijas extienden la ruta padre.
En cada paso del árbol de ruta, agrega una barra (`/`) seguida de la ruta, a menos que la ruta este vacía.

Aplica esta lógica a la navegación dentro del centro de crisis con la ruta padre `/crisis-center`.

- Para navegar al `CrisisCenterHomeComponent`, la URL completa es `/crisis-center` (`/crisis-center` + `''` + `''`).

- Para navegar al `CrisisDetailComponent` por una crisis con `id=2`, la URL completa es `/crisis-center/2` (`/crisis-center` + `''` + `'/2'`).

La URL absoluta para el último ejemplo, incluido el origen `localhost`, es la siguiente:

<code-example>
  localhost:4200/crisis-center/2

</code-example>

Aquí está el archivo completo `crisis-center-routing.module.ts` con sus respectivos `imports`.

<code-example path="router/src/app/crisis-center/crisis-center-routing.module.1.ts" header="src/app/crisis-center/crisis-center-routing.module.ts (excerpt)"></code-example>

{@a import-crisis-module}

### Importar el módulo de centro de crisis en las rutas de `AppModule`

Al igual que con el `HeroesModule`, debes agregar el `CrisisCenterModule` al array de `imports` del `AppModule` _antes_ del `AppRoutingModule`:

<code-tabs>

<code-pane path="router/src/app/crisis-center/crisis-center.module.ts"header="src/app/crisis-center/crisis-center.module.ts">

  </code-pane>

  <code-pane path="router/src/app/app.module.4.ts" header="src/app/app.module.ts (import CrisisCenterModule)" region="crisis-center-module">

  </code-pane>

</code-tabs>

Elimina la ruta de centro de crisis inicial del archivo `app-routing.module.ts` porque ahora los módulos de `HeroesModule` y `CrisisCenter` proporcionan la funcionalidad de las rutas.

El archivo `app-routing.module.ts` conserva las rutas de nivel superior de la aplicación, como las rutas predeterminadas y las rutas comodín.

<code-example path="router/src/app/app-routing.module.3.ts" header="src/app/app-routing.module.ts (v3)" region="v3"></code-example>

{@a relative-navigation}

### Navegación relativa

Mientras construías la funcionalidad del centro de crisis, navegaste a la ruta del detalle de crisis usando una ruta absoluta que comienza con una barra.

El enrutador hace coincidir las rutas absolutas con las rutas, comenzado desde la parte superior de la configuración de la ruta.

Puedes seguir usando rutas absolutas como estas para navegar dentro de la funcionalidad del Centro de Crisis, pero eso fija los enlaces a la estructura del enrutamiento padre.
Si cambias la ruta padre `/crisis-center`, tendrías que cambiar el array con los parámetros del enlace.

Puedes liberar los enlaces de esta dependencia definiendo las rutas que son relativas al segmento de URL actual.
La navegación dentro del área de funcionalidad permanece intacta incluso si cambias la ruta padre a esta funcionalidad.

<div class="alert is-helpful">

El enrutador admite una sintaxis similar a un directorio en una _lista de parámetros de enlace_ para ayudarte a guiar la búsqueda de nombres de rutas:

`./` o `sin barra inclinada` es relativa al nivel actual.

`../` para subir un nivel en la ruta.

Puedes combinar la sintaxis de navegación relativa con una ruta anterior.
Si debes navegar a una ruta hermana, puedes usar la convención `../<sibling>` para subir un nivel por encima de la ruta hermana.

</div>

Para navegar por una ruta relativa con el método `Router.navigate`, debes proporcionar el `ActivatedRoute` para informar al enrutador donde te encuentras en el árbol de rutas actual.

Después del _array de parámetros de enlaces_, agrega un objeto con la propiedad `relativeTo` establecida en la `ActivatedRoute`.
Luego el enrutador calculara la URL de destino en función de la ubicación de la ruta activa.

<div class="alert is-helpful">

Siempre especifica la ruta absoluta completa cuando llames al método `navigateByUrl()` del enrutador.

</div>

{@a nav-to-crisis}

### Navega a la lista de crisis usando una URL relativa

Ya has inyectado el `ActivatedRoute` que se necesita para componer la ruta de navegación relativa.

Al usar el `RouterLink` para navegar en lugar del servicio del `Router`, utilizara el mismo array de parámetros de enlace, pero no proporcionara al objeto la propiedad `relativeTo`.
El `ActivatedRoute` está implícito en la directiva `RouterLink`.

Actualiza el método `gotoCrises()` de `CrisisDetailComponent` para navegar de vuelta a la lista del Centro de Crisis utilizando la navegación de ruta relativa.

<code-example path="router/src/app/crisis-center/crisis-detail/crisis-detail.component.ts" header="src/app/crisis-center/crisis-detail/crisis-detail.component.ts (relative navigation)" region="gotoCrises-navigate"></code-example>

Observa que la ruta sube un nivel usando la sintaxis `../`.
Si la crisis actual tiene un `id` de `3`, la ruta resultante de regreso a la lista de crisis es `/crisis-center/;id=3;foo=foo`.

{@a named-outlets}

### Visualizando múltiples rutas en outlets nombrados

Decides darle a los usuarios una manera de contactar con el centro de crisis.
Cuando un usuario hace clic en el botón de "Contact", quieres mostrar un mensaje en una vista emergente.

La ventana emergente debe de permanecer abierta, incluso al cambiar de página en la aplicación, hasta que el usuario la cierre enviando el mensaje o cancelando.
Claramente no puedes colocar la ventana emergente en el mismo outlet que las otras páginas.

Hasta ahora, has definido un único outlet y has anidado rutas hijas debajo de ese outlet para agrupar las rutas.
El enrutador solo admite una salida principal de un outlet sin nombre por plantilla.

Una plantilla puede tener cualquier cantidad de outlets con nombre.
Cada outlet nombrado tiene su propio conjunto de rutas con sus propios componentes.
Múltiples outlets pueden mostrar diferente contenido, determinado por diferentes rutas, todas al mismo tiempo.

Agrega un outlet llamado "popup" en el `AppComponent`, directamente debajo del outlet sin nombre.

<code-example path="router/src/app/app.component.4.html" header="src/app/app.component.html (outlets)" region="outlets"></code-example>

Ahí es donde ira la ventana emergente, una vez que aprendas a enrutar un componente emergente a ella.

{@a secondary-routes}

#### Rutas secundarias

Los outlets nombrados son objetivo de las _rutas secundarias_.

Las rutas secundarias se ven como rutas principales y se configuran de la misma manera.
Se diferencian en algunos aspectos clave:

- Son independientes entre sí.
- Funcionan en combinación con otras rutas.
- Se muestran en puntos con outlets nombrados.

Genera un nuevo componente para redactar el mensaje.

<code-example language="none" class="code-shell">
  ng generate component compose-message
</code-example>

Muestra un formulario con un encabezado corto, una caja de input para el mensaje y dos botones, "Send" y "Cancel".

<div class="lightbox">
  <img src='generated/images/guide/router/contact-popup.png' alt="Ventana emergente de contacto">
</div>

Aquí está el componente, la plantilla y sus estilos:

<code-tabs>

  <code-pane header="src/app/compose-message/compose-message.component.css" path="router/src/app/compose-message/compose-message.component.css">

  </code-pane>

  <code-pane header="src/app/compose-message/compose-message.component.html" path="router/src/app/compose-message/compose-message.component.html">

  </code-pane>

  <code-pane header="src/app/compose-message/compose-message.component.ts" path="router/src/app/compose-message/compose-message.component.ts">

  </code-pane>

</code-tabs>

Se parece a cualquier otro componente en esta guía, pero existen dos diferencias clave:

Ten en cuenta que el método `send()` simula la latencia esperando un segundo antes de "enviar" el mensaje y cerrar la ventana emergente.

El método `closePopup()` cierra la vista emergente navegando al outlet de la ventana emergente con un `null` que cubre la sección sobre como [borrar rutas secundarias](#clear-secondary-routes).

{@a add-secondary-route}

#### Agregar una ruta secundaria

Abre el `AppRoutingModule` y agrega una nueva ruta `compose` a las `appRoutes`.

<code-example path="router/src/app/app-routing.module.3.ts" header="src/app/app-routing.module.ts (compose route)" region="compose"></code-example>

Además de las propiedades `path` y `component`, hay una nueva propiedad llamada `outlet`, que se establece en `'popup'`.
Esta ruta ahora apunta al outlet popup y el `ComposeMessageComponent` se mostrará allí.

Para darle a los usuarios una forma de abrir la ventana emergente, agrega un enlace de "Contact" a la plantilla `AppComponent`.

<code-example path="router/src/app/app.component.4.html" header="src/app/app.component.html (contact-link)" region="contact-link"></code-example>

Aunque la ruta `compose` está configurada para el outlet "popup", eso no es suficiente para conectar la ruta con la directiva `RouterLink`.
Tienes que especificar el outlet con nombre en un _array de parámetros de enlace_ y enlaza la al `RouterLink` con la propiedad binding.

El _array de parámetros de enlace_ contiene un objeto con una única propiedad `outlets`, cuyo valor es otro objeto con una (ó más) nombres de outlets.
En este caso hay solo un outlet con la propiedad "popup" y su valor es otro _array de parámetros de enlace_ que especifica la ruta `compose`.

En otras palabras, cuando el usuario hace clic en este enlace, el enrutador muestra el componente asociado con la ruta `compose` en el outlet `popup`.

<div class="alert is-helpful">

Este objeto `outlets` dentro de un objeto exterior es innecesario cuando solo hay una ruta y un outlet sin nombre.

El enrutador asume que su especificación en la ruta tiene como objetivo el outlet principal sin nombre y crea ese objeto por ti.

Enrutando un outlet con nombre revela una funcionalidad del enrutador:
Puedes apuntar a multiples outlets con multiples rutas en una misma directiva `RouterLink`.

</div>

{@a secondary-route-navigation}

#### Navegación de ruta secundaria: combinando rutas durante la navegación

Navega al _Centro de Crisis_ y haz clic en "Contact".
Deberías de ver algo como la siguiente URL en la barra de direcciones del navegador:

<code-example>
  http://.../crisis-center(popup:compose)

</code-example>

La parte relevante de la URL es lo que sigue del `...`:

- El `crisis-center` es la navegación principal.
- Los paréntesis rodean la ruta secundaria.
- La ruta secundaria consiste en un outlet con nombre (`popup`), un separador de `dos puntos`, y la ruta secundaria (`compose`).

Haz clic en el enlace de _Héroes_ y vuelve a mirar la URL.

<code-example>
  http://.../heroes(popup:compose)
</code-example>

La parte de la navegación primaria ha cambiado; la ruta secundaria es la misma.

El enrutador realiza un seguimiento de dos ramas separadas en un mismo árbol de navegación y genera una representación de ese árbol en la URL.

Puedes agregas muchos más outlets y rutas, en el nivel superior y en niveles anidados, creando una navegación de árbol con muchas ramas y el enrutador va a generar las URLs correspondientes.

Puedes decirle al enrutador que navegue hacia una por un árbol completo a la vez completando el objeto `outlets` y luego pasar ese objeto dentro de un _array de parámetros de enlace_ al método `router.navigate`.

{@a clear-secondary-routes}

#### Limpiando las rutas secundarias

Al igual que los outlets regulares, los outlets secundarios persisten hasta que navegas hacia un nuevo componente.

Cada outlet secundario tiene su propia navegación, independiente de la navegación del outlet principal. Cambiar la ruta actual que se muestra en el outlet primario no tiene efecto en el outlet de la ventana emergente. Eso es porque la ventana emergente se mantiene visible mientras navegas entre las crisis y los héroes.

De nuevo el método `closePopup()`:

<code-example path="router/src/app/compose-message/compose-message.component.ts" header="src/app/compose-message/compose-message.component.ts (closePopup)" region="closePopup"></code-example>

Al hacer clic en los botones de "send" o "cancel", se borra la vista de la ventana emergente.
La función `closePopup()` navega imperativamente con el método `Router.navigate()`, pasando un [array de parámetros de enlace](#link-parameters-array).

Al igual que el array vinculado al _Contacto_ del `RouterLink` en el `AppComponent`, este incluye un objeto con una propiedad `outlets`. El valor de la propiedad `outlets` es otro objeto con los nombres de los outlets. El único outlet nombrado es `'popup'`.

Esta vez, el valor de `'popup'` es `null`.
Esa no es una ruta, pero es un valor legítimo. Establecer la ventana emergente del `RouterOutlet` a `null` borra el outlet y elimina la ruta secundaria de la ventana emergente de la URL actual.

{@a guards}

{@a milestone-5-route-guards}

## Hito 5: Guards de ruta

Por el momento, cualquier usuario puede navegar a cualquier lugar de la aplicación en cualquier momento, pero en a veces es necesario controlar el acceso a diferentes partes de la aplicación por varias razones. Alguna de las cuales pueden ser:

- Quizás el usuario no este autorizado a navegar hacia un componente en específico.
- Quizás el usuario deberías de iniciar sesión (autenticarse) antes.
- Quizás debería de buscar algunos datos antes de mostrar un componente en específico.
- Es posible que desee guardar los cambios antes de salir del componente actual.
- Es posible que debas preguntarle al usuario si está bien descartar los cambios pendientes en lugar de ser guardados.

Puedes agregar guards a la configuración de la ruta para manejar estos escenarios.

El valor de retorno de un guard controla el comportamiento del enrutador:

- Si retorna `true`, el proceso de navegación continúa.
- Si retorna `false`, el proceso de navegación se detiene y el usuario permanece quieto.
- Si retorna un `UrlTree`, la navegación actual se cancela y se inicia una nueva navegación con el `UrlTree` devuelto.

<div class="alert is-helpful">

**Nota:** El guard también puede decirle al enrutador que navegue a otra parte, cancelando efectivamente la navegación actual.
Al hacerlo dentro de un guard, el guard debe devolver `false`;

</div>

El guard podría devolver una respuesta booleana síncrona. Pero en muchos casos, el guard no puede producir una respuesta síncrona. El guard podría hacerle una pregunta al usuario, guardar cambios en el servidor o buscar nuevos datos. Todas son operaciones asíncronas.

En consecuencia, un guard de enrutamiento puede devolver un `Observable<boolean>` o un `Promise<boolean>` y el enrutador esperará a que el observable se resuelva en `true` o `false`.

<div class="alert is-critical">

**Nota:** El observable proporcionado al `Router` debe de completarse. Si el observable no se completa, la navegación no continua.

</div>

El enrutador admite múltiples interfaces de guard:

- [`CanActivate`](api/router/CanActivate) para mediar la navegación _hacia_ una ruta.

- [`CanActivateChild`](api/router/CanActivateChild) para mediar la navegación _hacia_ una ruta hija.

- [`CanDeactivate`](api/router/CanDeactivate) para mediar la navegación _fuera_ de la ruta actual.

- [`Resolve`](api/router/Resolve) para realizar la recuperación de datos _antes_ de la activación de la ruta.

- [`CanLoad`](api/router/CanLoad) para mediar la navegación _hacia_ un módulo de funciones cargado _asíncronamente_.

Puedes tener varios gards en cada nivel de la jerarquía de enrutamiento. El enrutador comprueba primero los guards `CanDeactivate` y `CanActivateChild`, desde la ruta hija más profunda hasta la parte superior. Luego verifica los guards `CanActivate` desde la ruta superior hasta la ruta hija más profunda. Si el módulo de funciones se carga de manera asíncrona, el guard `CanLoad` se verifica antes que el módulo sea cargado. Si _cualquier_ guard devuelve `false`, los guards pendientes que no se hayan completado se cancelarán y se cancelará toda la navegación.

Hay varios ejemplos en las siguientes secciones:

{@a can-activate-guard}

### `CanActivate`: requiere autenticación

Las aplicaciones a menudo restringen el acceso a una área de funciones basadas en la identidad del usuario. Puedes permitir el acceso solo a los usuarios autenticados o a los usuarios con un rol en específico. Puedes bloquear o limitar el acceso hasta que la cuenta del usuario sea activado.

El guard `CanActivate` es la herramienta para administrar estas reglas de negocio en la navegación.

#### Agregar un módulo de funciones de administrador

Esta sección te guiará para extender el centro de crisis con algunas nuevas funcionalidades administrativas.
Empieza agregando un nuevo módulo de función llamado `AdminModule`.

Genera una carpeta `admin` con un archivo de módulo de funciones y un archivo de configuración de enrutamiento.

<code-example language="none" class="code-shell">
  ng generate module admin --routing
</code-example>

Ahora, genera los componentes de apoyo.

<code-example language="none" class="code-shell">
  ng generate component admin/admin-dashboard
</code-example>

<code-example language="none" class="code-shell">
  ng generate component admin/admin
</code-example>

<code-example language="none" class="code-shell">
  ng generate component admin/manage-crises
</code-example>

<code-example language="none" class="code-shell">
  ng generate component admin/manage-heroes
</code-example>

La estructura de archivos de la funcionalidad del administrador se ve así:

<div class='filetree'>

  <div class='file'>
    src/app/admin
  </div>

  <div class='children'>

    <div class='file'>
      admin
    </div>

      <div class='children'>

        <div class='file'>
          admin.component.css
        </div>

        <div class='file'>
          admin.component.html
        </div>

        <div class='file'>
          admin.component.ts
        </div>

      </div>

    <div class='file'>
      admin-dashboard
    </div>

      <div class='children'>

        <div class='file'>
          admin-dashboard.component.css
        </div>

        <div class='file'>
          admin-dashboard.component.html
        </div>

        <div class='file'>
          admin-dashboard.component.ts
        </div>

      </div>

    <div class='file'>
      manage-crises
    </div>

      <div class='children'>

        <div class='file'>
          manage-crises.component.css
        </div>

        <div class='file'>
          manage-crises.component.html
        </div>

        <div class='file'>
          manage-crises.component.ts
        </div>

      </div>

    <div class='file'>
      manage-heroes
    </div>

      <div class='children'>

        <div class='file'>
          manage-heroes.component.css
        </div>

        <div class='file'>
          manage-heroes.component.html
        </div>

        <div class='file'>
          manage-heroes.component.ts
        </div>

      </div>

    <div class='file'>
      admin.module.ts
    </div>

    <div class='file'>
      admin-routing.module.ts
    </div>

  </div>

</div>

El módulo de funciones de la administración contiene el `AdminComponent` utilizado para el enrutamiento dentro del módulo de funciones, una ruta al tablero y dos componentes sin terminar para gestionar las crisis y héroes.

<code-tabs>

  <code-pane header="src/app/admin/admin/admin.component.html"  path="router/src/app/admin/admin/admin.component.html">

  </code-pane>

  <code-pane header="src/app/admin/admin-dashboard/admin-dashboard.component.html" path="router/src/app/admin/admin-dashboard/admin-dashboard.component.1.html">

  </code-pane>

  <code-pane header="src/app/admin/admin.module.ts" path="router/src/app/admin/admin.module.ts">

  </code-pane>

  <code-pane header="src/app/admin/manage-crises/manage-crises.component.html" path="router/src/app/admin/manage-crises/manage-crises.component.html">

  </code-pane>

  <code-pane header="src/app/admin/manage-heroes/manage-heroes.component.html"  path="router/src/app/admin/manage-heroes/manage-heroes.component.html">

  </code-pane>

</code-tabs>

<div class="alert is-helpful">

Aunque el panel de administración `RouterLink` solo contiene una barra inclinada relativa, sin un segmento de URL adicional, coincide con cualquier ruta dentro del área de funciones administrativas. Solo deseas que el enlace al `Dashboard` esté activado cuando el usuario visite esa ruta. Añadiendo un enlace adicional al `Dashboard` routerLink, [routerLinkActiveOptions]="{ exact: true }"`, marca el enlace `./`como activo cuando el usuario navega a la URL de`/admin` y no cuando navegas a cualquiera de las rutas hijas.

</div>

{@a component-less-route}
{@a component-less-route-grouping-routes-without-a-component}

##### Ruta sin componentes: agrupación de rutas sin un componente

La configuración de enrutamiento inicial de administración:

<code-example path="router/src/app/admin/admin-routing.module.1.ts" header="src/app/admin/admin-routing.module.ts (admin routing)" region="admin-routes"></code-example>

La ruta hija bajo el `AdminComponent` tiene una propiedad `path` y una `children`, pero no utiliza un `component`.
Esto se define como una ruta _sin componente_.

Para agrupar las rutas de gestión del `Crisis Center` bajo la ruta `admin` no es necesario un componente.
Ademas una ruta _sin componente_ facilita el [proteger las rutas hijas](#can-activate-child-guard).

A continuación, importa el `AdminModule` en `app.module.ts` y agrégalo al array de `imports` para registrar las rutas de administración.

<code-example path="router/src/app/app.module.4.ts" header="src/app/app.module.ts (admin module)" region="admin-module"></code-example>

Agrega un enlace "Admin" a la carcasa de `AppComponent` para que los usuarios, puedan acceder a esta función.

<code-example path="router/src/app/app.component.5.html" header="src/app/app.component.html (template)"></code-example>

{@a guard-admin-feature}

#### Protege la función de administrador

Actualmente, todas las rutas del Centro de Crisis están abiertas para todos. La nueva funcionalidad de administración debería de ser accesible solo para los usuarios autenticados.

Escribe un método guard `canActivate()` para redirigir a los usuarios anónimos a la página de inicio de sesión cuanto intenten ingresar al área de administración.

Genera un `AuthGuard` en la carpeta de `auth`.

<code-example language="none" class="code-shell">
  ng generate guard auth/auth
</code-example>

Para demostrar los fundamentos, este ejemplo solo registra en consola, `returns` verdadero inmediatamente, y permite que la navegación continúe:

<code-example path="router/src/app/auth/auth.guard.1.ts" header="src/app/auth/auth.guard.ts (excerpt)"></code-example>

A continuación, abre el archivo `admin-routing.module.ts `. importa la clase `AuthGuard` y actualiza la ruta de administración con una propiedad guard `canActivate` que haga referencia a ella:

<code-example path="router/src/app/admin/admin-routing.module.2.ts" header="src/app/admin/admin-routing.module.ts (guarded admin route)" region="admin-route"></code-example>

La función de administración ahora esta protegida por el guard, pero el guard requiere más personalización para funcionar completamente.

{@a teach-auth}

#### Autenticar con `AuthGuard`

Haz que el `AuthGuard` imite la autenticación.

El `AuthGuard` debe de llamar al servicio de la aplicación que pueda iniciar la sesión como usuario y retener la información del usuario actual.

Genera un nuevo `AuthService` en la carpeta `auth`:

<code-example language="none" class="code-shell">
  ng generate service auth/auth
</code-example>

Actualiza el `AuthService` para iniciar sesión como usuario:

<code-example path="router/src/app/auth/auth.service.ts" header="src/app/auth/auth.service.ts (excerpt)"></code-example>

Aunque en realidad no inicia sesión, tiene una bandera `isLoggedIn` para indicar que el usuario está autenticado.
Su método `login()` simula una llamada a un servicio de API externo al devolver un observable que resuelve con éxito después de una breve pausa.
La propiedad `redirectUrl` almacena la URL a la cual el usuario quería acceder para que pueda navegar a ella después de la autenticación.

<div class="alert is-helpful">

Para mantener la cosas al mínimo, este ejemplo redirige a los usuarios no autenticados a `/admin`.

</div>

Revisa el `AuthGuard` para llamar al `AuthService`.

<code-example path="router/src/app/auth/auth.guard.2.ts" header="src/app/auth/auth.guard.ts (v2)"></code-example>

Observa que se inyectaron el `AuthService` y el `Router` en el constructor.
Aún no has proporcionado el `AuthService`, pero es bueno saber que puedes inyectar servicios útiles en los guards del enrutador.

Este guard retorna un resultado booleano síncrono.
Si el usuario ha iniciado sesión, regresara verdadero y la navegación continuara.

El `ActivatedRouteSnapshot` contiene la ruta _futura_ que se activara y `RouterStateSnapshot` contiene el _futuro_ `RouterState` de la aplicación, en caso de que pase por el guard.

Si el usuario no ha iniciado sesión, almacena el intento URL de donde proviene el usuario usando el `RouterStateSnapshot.url` y le dice al enrutador que lo redireccione a la página de inicio de sesión&mdash; página que aún no has creado.
Devuelve un `UrlTree` que llama al `Router` a cancelar la navegación actual y programa una nueva para redirigir al usuario.

{@a add-login-component}

#### Agrega el `LoginComponent`

Necesitas un `LoginComponent` para que el usuario inicie sesión en la aplicación. Después de iniciar sesión, será redireccionado a la URL almacenada si esta disponible o usara la URL predeterminada.
No hay nada nuevo en este componente o en la forma en que se utiliza la configuración del enrutador.

<code-example language="none" class="code-shell">
  ng generate component auth/login
</code-example>

Registra una ruta `/login` en el archivo `auth/auth-routing.module.ts`.
En `app.module.ts`, importa y agrega el `AuthModule` al import de `AppModule`.

<code-tabs>

  <code-pane header="src/app/app.module.ts" path="router/src/app/app.module.ts" region="auth">

  </code-pane>

  <code-pane header="src/app/auth/login/login.component.html" path="router/src/app/auth/login/login.component.html">

  </code-pane>

  <code-pane header="src/app/auth/login/login.component.ts" path="router/src/app/auth/login/login.component.1.ts">

  </code-pane>

  <code-pane header="src/app/auth/auth.module.ts" path="router/src/app/auth/auth.module.ts">

  </code-pane>

</code-tabs>

{@a can-activate-child-guard}

### `CanActivateChild`: protegiendo las rutas hijas

También puedes proteger las rutas hijas con el guard `CanActivateChild`.
El guard `CanActivateChild` es similar al guard `CanActivate`.
La diferencia clave es que corre antes de que se active cualquier ruta hija.

El módulo de función de administración está protegido de accesos no autorizados.
También deben de ser protegidas las hijas rutas _dentro_ del módulo de funciones.

Extiende el `AuthGuard` para proteger al navegar entre las rutas de `admin`.
Abre el archivo `auth.guard.ts` y agrega la interfaz `CanActivateChild` a los tokens importados del paquete del enrutador.

A continuación implementa el método `canActivateChild()` el cual toma los mismos argumentos que el método `canActivate()`: un `ActivatedRouteSnapshot` y el `RouterStateSnapshot`.
El método `canActivateChild()` puede devolver un `Observable<boolean|UrlTree>` o `Promise<boolean|UrlTree>` para comprobaciones asíncronas y un `boolean` or `UrlTree` para comprobaciones síncronas.
Este devuelve `true` para permitir al usuario acceder al módulo de funciones de administración o `UrlTree` que redirige al usuario a la página de inicio de sesión:

<code-example path="router/src/app/auth/auth.guard.3.ts" header="src/app/auth/auth.guard.ts (excerpt)" region="can-activate-child"></code-example>

Agrega el mismo `AuthGuard` a la ruta de administración `component-less` para proteger todas las otras rutas hijas al mismo tiempo, en lugar de agregar el `AuthGuard` a cada ruta individualmente.

<code-example path="router/src/app/admin/admin-routing.module.3.ts" header="src/app/admin/admin-routing.module.ts (excerpt)" region="can-activate-child"></code-example>

{@a can-deactivate-guard}

### `CanDeactivate`: manejo de cambios no guardados

De vuelta al flujo de trabajo de "Héroes", la aplicación acepta cambios en los héroes inmediatamente sin ningún tipo de validación.

En el mundo real, es posible que debas de acumular los cambios de los usuarios, validar los campos, validar en el servidor ó mantener los cambios en pendiente hasta que el usuario los confirme ó los cancele y revertir todos los cambios.

Cuando el usuario sale de la página, puedes permitir que el usuario decida que hacer con los cambios no guardados.
Si el usuario los cancela, se mantendrá en la misma página y permitira continuar con los cambios.
Si el usuario los aprueba, la aplicación deberá guardarlos.

Aún puedes retrasar la navegación hasta que el guardado de los cambios se realice correctamente.
Si permites que el usuario cambie de pantalla inmediatamente y el guardado fallara (talvez los datos se consideren inválidos), perderías el contexto del error.

Se necesita parar la navegación mientras esperas, de forma asíncrona, hasta que el servidor devuelva la respuesta.

El guard `CanDeactivate` te ayuda a decidir que hacer con los cambios no guardados y cómo proceder.

{@a cancel-save}

#### Cancelar y guardar

Los usuarios actualizan la información de una crisis en el `CrisisDetailComponent`.
A diferencia del `HeroDetailComponent`, los cambios del usuario no actualizan la entidad de la crisis inmediatamente.
En cambio, la aplicación actualiza la entidad cuando el usuario presiona el botón de Guardar y descarta los cambios cuando el usuario presiona el botón de Cancelar.

Ambos botones regresan a la lista de crisis después de guardar o cancelar.

<code-example path="router/src/app/crisis-center/crisis-detail/crisis-detail.component.ts" header="src/app/crisis-center/crisis-detail/crisis-detail.component.ts (cancel and save methods)" region="cancel-save"></code-example>

En este escenario, el usuario podría hacer clic en el enlace de héroes, cancelar, presionar el botón del navegador para volver, o cambiar la página sin guardar.

Esta aplicación de ejemplo le pide al usuario que sea explícito con un cuadro de diálogo que espera sincrónicamente la respuesta del usuario.

<div class="alert is-helpful">

Podrías esperar la respuesta del usuario con bloque de código sincrónico, sin embargo, la aplicación responde mejor&mdash; y puedes continuar trabajando&mdash; mientras esperas la respuesta del usuario de forma asincrónica.

</div>

Genera un servicio de `Dialog` para manejar la confirmación del usuario.

<code-example language="none" class="code-shell">
  ng generate service dialog
</code-example>

Agrega un método `confirm()` al `DialogService` para pedirle al usuario que confirme su intención.
El `window.confirm` es una acción de bloqueo que muestra un diálogo modal y espera la interacción del usuario.

<code-example path="router/src/app/dialog.service.ts" header="src/app/dialog.service.ts"></code-example>

Devuelve un `Observable` que resuelve cuando el usuario finalmente decide que hacer: descartar los cambios y continuar navegando (`true`) ó mantener los cambios pendientes y permanecer en el editor de crisis (`false`).

{@a CanDeactivate}

Genera un guard que compruebe la presencia de un método `canDeactivate()` en un componente&mdash; cualquier componente.

<code-example language="none" class="code-shell">
  ng generate guard can-deactivate
</code-example>

Pega el siguiente código en tu guard.

<code-example path="router/src/app/can-deactivate.guard.ts" header="src/app/can-deactivate.guard.ts"></code-example>

Si bien el guard no tiene porque saber cuál componente tiene un método de desactivación, puede detectar que el componente `CrisisDetailComponent` tiene un método `canDeactivate()` y llamarlo.
Que el guard no conozca los detalles del método de desactivación de ningún componente, hace que el guard sea re usable.

Alternativamente, puedes crear un guard `CanDeactivate` específicamente para el componente de `CrisisDetailComponent`.
El método `canDeactivate()` proporciona la instancia actual del `component`, su actual `ActivatedRoute`, y `RouterStateSnapshot` en caso de que se necesite acceder a información externa.

Esto sería útil si solo quisieras usar este guard para este componente y necesitaras obtener las propiedades del componente o confirmar si el enrutador debe permitir la navegación fuera de él.

<code-example path="router/src/app/can-deactivate.guard.1.ts" header="src/app/can-deactivate.guard.ts (component-specific)"></code-example>

Mirando de vuelta al `CrisisDetailComponent`, implementa el flujo de trabajo de confirmación para los cambios no guardados.

<code-example path="router/src/app/crisis-center/crisis-detail/crisis-detail.component.ts" header="src/app/crisis-center/crisis-detail/crisis-detail.component.ts (excerpt)" region="canDeactivate"></code-example>

Observa que el método `canDeactivate()` puede regresar sincrónicamente; devuelve `true` inmediatamente si no hay crisis o cambios pendientes.
Pero también puede devolver una `Promise` o un `Observable` y el enrutador esperará a que se resuelva como verdadero (navegar) ó falso (permanecer en la ruta actual).

Agrega el `Guard` a la ruta del detalle de crisis en `crisis-center-routing.module.ts` usando la propiedad del array `canDeactivate`.

<code-example path="router/src/app/crisis-center/crisis-center-routing.module.3.ts" header="src/app/crisis-center/crisis-center-routing.module.ts (can deactivate guard)"></code-example>

Ahora el usuario tiene una protección contra los cambios no guardados.

{@a Resolve}

{@a resolve-guard}

### _Resolve_: pre-cargando datos de componente

En el `Hero Detail` y `Crisis Detail`, la aplicación espera hasta que enrutador sea activado para buscar héroe o crisis correspondiente.

Si está utilizando una API del mundo real, puede haber algún retraso antes que el servidor devuelva los datos a mostrar.
No deseamos mostrar un componente vacío mientras esperamos por los datos.

Para mejorar este comportamiento, puedes pre-cargar datos del servidor usando un resolver para que los datos estén listos al momento en que la ruta sea activada.
Esto también permite el manejar los errores antes de enrutar al componente.
No tiene sentido navegar al detalle de una crisis por un `id` que no tiene un registro.
Sería mejor enviar al usuario de vuelta a `Crisis List` que muestra solo los centros de crisis válidos.

En resumen, deseas retrasar la renderización del componente enrutado hasta que se hayan obtenido todos los datos necesarios.

{@a fetch-before-navigating}

#### Obtener datos antes de navegar

Por el momento el `CrisisDetailComponent` recupera la información de la crisis seleccionada.
Si la crisis no es encontrada, el enrutador vuelve a la vista de la lista de crisis.

La experiencia podría ser mejor si todo se manejara primero, antes que la ruta sea activada.
El servicio `CrisisDetailResolver` podria recuperar una `Crisis` o navegar fuera, si la `Crisis` no existiera, _antes_ de activar la ruta y crear el `CrisisDetailComponent`.

Genera un archivo para el servicio `CrisisDetailResolver` dentro del área de función de `Crisis Center`.

<code-example language="none" class="code-shell">
  ng generate service crisis-center/crisis-detail-resolver
</code-example>

<code-example path="router/src/app/crisis-center/crisis-detail-resolver.service.1.ts" header="src/app/crisis-center/crisis-detail-resolver.service.ts (generated)"></code-example>

Mueve las partes relevantes de la lógica de recuperación de crisis en `CrisisDetailComponent.ngOnInit()` a `CrisisDetailResolverService`.
Importa el modelo `Crisis`, `CrisisService`, y el `Router` para que pueda navegar a otra parte si no pudiera encontrar la crisis.

Se explicitó e implementa la interfaz del `Resolve` con un tipo de `Crisis`.

Inyecta el `CrisisService` y `Router` e implementa el método `resolve()`.
Este método podría devolver una `Promise`, y un `Observable`, o un valor sincrónico.

El método `CrisisService.getCrisis()` devuelve un observable para evitar que la ruta cargue hasta que se obtengan los datos.
El guard `Router` requiere un observable `complete`, lo que significa que ha emitido todos sus valores.
Puedes usar el operador `take` con el argumento `1` para asegurar que el `Observable` se completa después de recuperar el primer valor del Observable devuelto por el método `getCrisis()`.

Si no devuelve una `Crisis` válida, entonces devuelve un `Observable` vacío, cancela la navegación en progreso al `CrisisDetailComponent`, y regresa al usuario al `CrisisListComponent`.
La actualización del servicio resolver tiene este aspecto:

<code-example path="router/src/app/crisis-center/crisis-detail-resolver.service.ts" header="src/app/crisis-center/crisis-detail-resolver.service.ts"></code-example>

Importar este resolver en el archivo `crisis-center-routing.module.ts` y agrega el objeto `resolve` en la configuración de ruta de `CrisisDetailComponent`.

<code-example path="router/src/app/crisis-center/crisis-center-routing.module.4.ts" header="src/app/crisis-center/crisis-center-routing.module.ts (resolver)"></code-example>

El `CrisisDetailComponent` ya no debería de buscar la crisis.
Cuando reconfiguras la ruta, cambias donde está la crisis.
Actualiza el `CrisisDetailComponent` para obtener la crisis de la propiedad `ActivatedRoute.data.crisis` en su lugar.

<code-example path="router/src/app/crisis-center/crisis-detail/crisis-detail.component.ts" header="src/app/crisis-center/crisis-detail/crisis-detail.component.ts (ngOnInit v2)" region="ngOnInit"></code-example>

Ten en cuenta los siguientes puntos importantes:

1. La interfaz del `Resolve` del enrutador es opcional.
   El `CrisisDetailResolverService` no hereda de una clase base.
   El enrutador busca ese método y lo llama si lo encuentra.

1. El enrutador llama el resolver en cualquier caso donde el usuario pueda navegar para que no tengas que codificar cada caso de uso.

1. Devolver un `Observable` vacío en al menos un resolver cancelará la navegación.

A continuación, el código relevante para el Centro de Crisis para este hito:

<code-tabs>

  <code-pane header="app.component.html" path="router/src/app/app.component.html">

  </code-pane>

  <code-pane header="crisis-center-home.component.html" path="router/src/app/crisis-center/crisis-center-home/crisis-center-home.component.html">

  </code-pane>

  <code-pane header="crisis-center.component.html" path="router/src/app/crisis-center/crisis-center/crisis-center.component.html">

  </code-pane>

  <code-pane header="crisis-center-routing.module.ts" path="router/src/app/crisis-center/crisis-center-routing.module.4.ts">

  </code-pane>

  <code-pane header="crisis-list.component.html" path="router/src/app/crisis-center/crisis-list/crisis-list.component.html">

  </code-pane>

  <code-pane header="crisis-list.component.ts" path="router/src/app/crisis-center/crisis-list/crisis-list.component.ts">

  </code-pane>

  <code-pane header="crisis-detail.component.html" path="router/src/app/crisis-center/crisis-detail/crisis-detail.component.html">

  </code-pane>

  <code-pane header="crisis-detail.component.ts" path="router/src/app/crisis-center/crisis-detail/crisis-detail.component.ts">

  </code-pane>

  <code-pane header="crisis-detail-resolver.service.ts" path="router/src/app/crisis-center/crisis-detail-resolver.service.ts">

  </code-pane>

  <code-pane header="crisis.service.ts" path="router/src/app/crisis-center/crisis.service.ts">

  </code-pane>

  <code-pane header="dialog.service.ts" path="router/src/app/dialog.service.ts">

  </code-pane>

</code-tabs>

Guards

<code-tabs>

  <code-pane header="auth.guard.ts" path="router/src/app/auth/auth.guard.3.ts">

  </code-pane>

  <code-pane header="can-deactivate.guard.ts" path="router/src/app/can-deactivate.guard.ts">

  </code-pane>

</code-tabs>

{@a query-parameters}

{@a fragment}
{@a query-parameters-and-fragments}

### Parámetros de consulta y fragmentos

En la sección de [parametros de ruta](#optional-route-parameters), solo se trataron los parámetros específicos de la ruta.
Sin embargo, puedes utilizar parámetros opcionales disponibles para todas las rutas.

Los [Fragmentos](https://en.wikipedia.org/wiki/Fragment_identifier) hacen referencia a ciertos elementos en la página identificados con un atributo `id`.

Actualiza el `AuthGuard` para proporcionar una consulta de `session_id` que permanecerá después de navegar a otra ruta.

Agrega un elemento `anchor` para que puedas dar un salto a un punto determinado de la página.

Agrega el objeto `NavigationExtras` al método `router.navigate()` que te lleva a la ruta `/login`.

<code-example path="router/src/app/auth/auth.guard.4.ts" header="src/app/auth/auth.guard.ts (v3)"></code-example>

También puedes conservar los parámetros de consulta y los fragmentos en la navegación sin necesidad de volverlos a proporcionar.
En el `LoginComponent`, agrega un _objeto_ como segundo argumento en la función `router.navigateUrl()` y proporcionara el `queryParamsHandling` y `preserveFragment` para pasar los parámetros de consulta actuales y el fragmento a la siguiente ruta.

<code-example path="router/src/app/auth/login/login.component.ts" header="src/app/auth/login/login.component.ts (preserve)" region="preserve"></code-example>

<div class="alert is-helpful">

La función `queryParamsHandling` proporciona una opción de `merge`, que conserva y combina los parámetros de consulta actuales con cualquier parámetro de consulta proporcionado al navegar.

</div>

Para navegar a la ruta de Admin Dashboard después de iniciar sesión, actualiza el archivo `admin-dashboard.component.ts` para manejar los parámetros de consulta y fragmentos.

<code-example path="router/src/app/admin/admin-dashboard/admin-dashboard.component.1.ts" header="src/app/admin/admin-dashboard/admin-dashboard.component.ts (v2)"></code-example>

Los parámetros de consulta y los fragmentos también están disponibles a través del servicio `ActivatedRoute`.
Al igual que los parámetros de ruta, los parámetros de consulta y los fragmentos se proporcionan como un `Observable`.
El componente de Administración de Crisis alimenta el `Observable` directamente en la plantilla utilizando el `AsyncPipe`.

Ahora, puedes hacer clic en el botón de administración, el cual te lleva a la página de Login con el `queryParamMap` y `fragment` proporcionados.
Después de hacer clic en el botón de Login, te darás cuenta de que has sido redirigido a la página `Admin Dashboard` con los parámetros de consulta y fragmentos aún intactos en la barra de direcciones.

Puedes usas esos bits persistentes de información para cosas que deben proporcionarse en páginas como tokens de autenticación ó ids de sesión.

<div class="alert is-helpful">

Los `query params` y `fragment` también pueden ser preservados usando un `RouterLink` con los enlaces a `queryParamsHandling` y `preserveFragment` respectivamente.

</div>

{@a asynchronous-routing}

## Hito 6: Enrutamiento asincrónico

A medida que has trabajado en los hitos, la aplicación ha ido creciendo de manera natural.
En algún momento, llegará un punto en el que la aplicación tendrá tiempos de carga muy largos.

Para solucionar este problema, usa el enrutamiento asincrónico, que carga módulos de funciones de manera perezosa sobre pedido.
El `Lazy loading` tiene múltiples beneficios.

- Puedes cargar áreas de funciones solo cuando sean solicitadas por el usuario.
- Puede acelerar el tiempo de carga para los usuarios que solo visiten determinadas áreas de la aplicación.
- Puedes continuar expandiendo las áreas de funciones sin incrementar el tamaño del paquete en la carga inicial.

Ya eres parte del camino.
Al organizar la aplicación en módulos&mdash;`AppModule`,`HeroesModule`, `AdminModule` y `CrisisCenterModule`&mdash;tienes candidatos naturales para la carga diferida.

Algunos módulos, como el `AppModule`, deben de ser cargados desde el inicio.
Pero otros pueden y deben cargar de manera diferida.
El `AdminModule`, por ejemplo, es necesario para algunos usuarios autorizados, por lo que solo debe de ser cargado cuando lo soliciten las personas adecuadas.

{@a lazy-loading-route-config}
{@a lazy-loading-route-configuration}

### Configuración de ruta de carga diferida

Cambia la ruta `admin` en el archivo `admin-routing.module.ts` de `'admin'` a una cadena vacía, `''`, la ruta vacía.

Utiliza rutas vacías para agrupar las rutas sin agregar ningún segmento de ruta adicional a la URL.
Los usuarios seguirán visitando `/admin` y el `AdminComponent` seguiran sirviendo el Componente de Enrutamiento que contiene las rutas hijas.

Abre el `AppRoutingModule` y agrega una nueva ruta `admin` al array de `appRoutes`.

Asigna una propiedad `loadChildren` en lugar de una propiedad `children`.
La propiedad `loadChildren` toma una función que devuelve una promesa usando la sintaxis incorporada en el navegador para el código de carga diferida usando imports dinámicos `import('...')`.
La ruta es la ubicación de `AdminModule` (relativa a la raíz de la aplicación).
Después de que se solicita y carga el código, la `Promise` resuelve un objeto que contiene el `NgModule` en este caso el `AdminModule`.

<code-example path="router/src/app/app-routing.module.5.ts" region="admin-1" header="app-routing.module.ts (load children)"></code-example>

<div class="alert is-important">

_Nota_: Cuando se utilizan rutas absolutas, la ubicación del archivo `NgModule` debe de comenzar con `src/app` para que resuelva correctamente. Para un [mapeo de ruta con rutas absolutas](https://www.typescriptlang.org/docs/handbook/module-resolution.html#path-mapping) personalizado, debes de configurar las propiedades `baseUrl` y `paths` en el proyecto `tsconfig.json`.

</div>

Cuando el enrutador navega a esta ruta, usa la cadena `loadChildren` para cargar dinámicamente el `AdminModule`.
Entonces agrega las rutas del `AdminModule` a su configuración de ruta actual.
Finalmente, carga la ruta solicitada al componente admin de destino.

La carga diferida y la reconfiguración ocurren solo una vez, cuando la ruta es solicitada por primera vez; el módulo y las rutas están disponibles inmediatamente para las solicitudes posteriores.

<div class="alert is-helpful">

Angular proporciona un cargador de módulos incorporado que soporta SystemJS para cargar módulos de forma asincrónica. Su fueras a usar otra herramienta de empaquetado, como Webpack, se utilizara el mecanismo de Webpack para cargar módulos de manera asincrónica.

</div>

Da el último pasa y desconecta el conjunto de funciones de administración de la aplicación principal.
La raíz `AppModule` no debe cargar ni hacer referencia al `AdminModule` ó sus archivos.

En `app.module.ts`, elimina la declaración del import a `AdminModule` de la parte superior del archivo y elimina el `AdminModule` del array de `imports` de NgModule.

{@a can-load-guard}
{@a can-load-guard-unauthorized-loading-of-feature-modules}

### `CanLoad`: proteger la carga no autorizada de módulos de funciones

Ya estás protegiendo el `AdminModule` con el guard `CanActivate` que evita que usuarios no autorizados accedan al área de funciones de administración.
Redirige a la página de inicio de sesión si el usuario no está autorizado.

Pero el enrutador todavía está cargando el `AdminModule` incluso si el usuario no puede visitar ninguno de esos componentes.
Idealmente, solo debería de cargar el `AdminModule` si el usuario inicio sesión.

Agrega un guard `CanLoad` que solo cargue el `AdminModule` una vez que el usuario tenga una sesión iniciada _y_ intente acceder al área de funciones de administración.

El `AuthGuard` existente ya cuenta con la lógica esencial en su método `checkLogin()` para dar soporte al guard `CanLoad`.

Abre el archivo `auth.guard.ts`.
Importa la interfaz `CanLoad` de `@angular/router`.
Agrégalo a la lista de implementos de la clase `AuthGuard`.
Entonces implementa `canLoad()` de la siguiente manera:

<code-example path="router/src/app/auth/auth.guard.ts" header="src/app/auth/auth.guard.ts (CanLoad guard)" region="canLoad"></code-example>

El enrutador establece el parámetro `route` del método `canLoad()` en la URL de destino deseada.
El método `checkLogin()` redirige esa URL una vez que el usuario ha iniciado sesión.

Ahora importa el `AuthGuard` en el `AppRoutingModule` y agrega el `AuthGuard` al array la propiedad de `canLoad` para la ruta de `admin`.
La ruta de administración completa se ve así:

<code-example path="router/src/app/app-routing.module.5.ts" region="admin" header="app-routing.module.ts (lazy admin route)"></code-example>

{@a preloading}
{@a preloading-background-loading-of-feature-areas}

### Precarga: carga en segundo plano las áreas de funciones

Además de cargar módulos bajo demanda, puedes cargar módulos de forma asíncrona con la precarga.

El `AppModule` es cargado cuando la aplicación inicial, lo que significa que se carga de inmediato.
Ahora el `AdminModule` carga solo cuando el usuario hace clic en un enlace, lo que se denomina carga diferida.

La precarga permite el cargar módulos en segundo plano para que los datos estén listos para renderizarse cuando el usuario activa una ruta en particular.
Considera el Centro de Crisis
No es la primera vista que ve un usuario.
Por defecto, los héroes son la primera vista.
Para obtener una carga inicial pequeña y un tiempo de inicio veloz, deben de cargar con entusiasmo el `AppModule` and the `HeroesModule`.

Podrías cargar el Centro de Crisis de forma diferida.
Pero estás casi seguro que el usuario visitara el Centro de Crisis en los primeros minutos después de iniciar la aplicación.
Idealmente, la aplicación se inciaría solo con el `AppModule` y el `HeroesModule` cargados y entonces, casi inmediatamente, cargar el `CrisisCenterModule` en segundo plano.
Para cuando el usuario navegue al Crisis Center, el módulo estará cargado y listo.

{@a how-preloading}
{@a how-preloading-works}

#### Cómo funciona la precarga

Después de cada navegación exitosa, el enrutador busca en su configuración un módulo que aún no haya sido cargado que pueda pre cargar.
Si va a pre cargar un módulo, y que módulo pre cargara, depende de la estrategia de precarga.

El `Router` ofrece dos estrategias de precarga:

- Sin precarga, que es la opción predeterminada. Las áreas de funciones cargadas de forma diferida todavía se cargan bajo demanda.
- Precarga todas las áreas de funciones de forma diferida.

El enrutador nunca precarga, o precarga todos los módulos de forma diferida.
El `Router` incluso soporta [estrategias personalizadas de precarga](#custom-preloading) para un control fino sobre cuáles módulos pre cargar y cuando.

Esta sección te guía a través de la actualización del `CrisisCenterModule` para cargar de forma diferida por defecto y utilizar la estrategia `PreloadAllModules` para cargar todos los módulos cargados de forma diferida.

{@a lazy-load-crisis-center}

#### Carga diferida del centro de crisis

Actualiza la configuración de la ruta para cargar de forma diferida el `CrisisCenterModule`.
Sigue los mismos pasos que utilizaste para configurar el `AdminModule` para la carga diferida.

1. Cambia la ruta del `crisis-center` en el `CrisisCenterRoutingModule` a una cadena vacía.
2. Agrega la ruta `crisis-center` al `AppRoutingModule`.
3. Configura la cadena `loadChildren` para cargar el `CrisisCenterModule`.
4. Elimina todas las menciones de `CrisisCenterModule` del archivo `app.module.ts`.

Aquí están los módulos actualizados _antes de habilitar la precarga_:

<code-tabs>

  <code-pane header="app.module.ts" path="router/src/app/app.module.ts" region="preload">

  </code-pane>

  <code-pane header="app-routing.module.ts" path="router/src/app/app-routing.module.6.ts" region="preload-v1">

  </code-pane>

  <code-pane header="crisis-center-routing.module.ts" path="router/src/app/crisis-center/crisis-center-routing.module.ts">

  </code-pane>

</code-tabs>

Puedes probarlo ahora y confirmar que el `CrisisCenterModule` carga después de que hagas clic en el botón de "Crisis Center".

Para habilitar la precarga de todos los módulos cargados de forma diferida, importa el token `PreloadAllModules` del paquete del enrutador de Angular.

El segundo argumento en el método `RouterModule.forRoot()` toma un objeto con opciones de configuración adicionales.
El `preloadingStrategy` es una de esas opciones.
Agrega el token `PreloadAllModules` a la llamada `forRoot()`:

<code-example path="router/src/app/app-routing.module.6.ts" header="src/app/app-routing.module.ts (preload all)" region="forRoot"></code-example>

Estas configuraciones a la precarga del `Router` para cargar inmediatamente todas las rutas cargadas de forma diferida (rutas con una propiedad `loadChildren`).

Cuando visitas `http://localhost:4200`, la ruta `/heroes` carga inmediatamente después del lanzamiento y el enrutador comienza a cargar el `CrisisCenterModule` justo después que el `HeroesModule` cargue.

Actualmente, el `AdminModule` no precarga porque `CanLoad` está bloqueándolo.

{@a preload-canload}

#### `CanLoad` bloqueo de precarga

La estrategia `PreloadAllModules` no carga todas las áreas de funciones protegidas por un guard [CanLoad](#can-load-guard).

Agrega un guard `CanLoad` a la ruta en el `AdminModule`, unos pasos hacia atrás para bloquear la carga de ese módulo hasta que el usuario esté autorizado.
Ese guard `CanLoad` tiene prioridad sobre la estrategia de precarga.

Si deseas pre cargar un módulo y protegerte contra el acceso no autorizado, elimina el método guard `canLoad()` y confía solo en el guard [canActivate()](#can-activate-guard).

{@a custom-preloading}
{@a custom-preloading-strategy}

### Estrategia de precarga personalizada

Pre cargar cada módulo cargado de forma diferida funciona bien en muchas situaciones.
Sin embargo, en consideración de ciertos aspectos como un ancho de banda bajo y las métricas de usuario, puedes usar una estrategia de precarga personalizada para módulos de funciones específicas.

Esta sección lo guía a través de la adición de una estrategia personalizada que solamente pre cargue las rutas con el indicador `data.preload` está establecido en `true`.
Recuerda que puedes agregar cualquier cosa a la propiedad `data` de una ruta.

Establece el indicador `data.preload` en la ruta `crisis-center` en el `AppRoutingModule`.

<code-example path="router/src/app/app-routing.module.ts" header="src/app/app-routing.module.ts (route data preload)" region="preload-v2"></code-example>

Genera un nuevo servicio `SelectivePreloadingStrategy`.

<code-example language="none" class="code-shell">
  ng generate service selective-preloading-strategy
</code-example>

Reemplaza el contenido del archivo `selective-preloading-strategy.service.ts` por lo siguiente:

<code-example path="router/src/app/selective-preloading-strategy.service.ts" header="src/app/selective-preloading-strategy.service.ts"></code-example>

`SelectivePreloadingStrategyService` implementa la `PreloadingStrategy`, que tiene un método, `preload()`.

En enrutador llama al método `preload()` con dos argumentos:

1. La ruta a considerar.
2. Una función de carga que puede cargar el módulo enrutado de forma asíncrona.

Una implementación de `preload` debe de devolver un `Observable`.
Si la ruta precarga, devuelve el observable devuelto al llamar la función de carga.
Si la ruta no precarga, devuelve un `Observable` con `null`.

En esta muestra, el método `preload()` carga la ruta si el indicador `data.preload` de la ruta es verdadero.

Como efecto secundario, `SelectivePreloadingStrategyService` registra el `path` de la ruta seleccionada en su array publico `preloadedModules`.

En breve, extendrás el `AdminDashboardComponent` para inyectar este servicio y mostrar su array `preloadedModules`.

Pero primero, haremos unos cambios al `AppRoutingModule`.

1. Importa `SelectivePreloadingStrategyService` en el `AppRoutingModule`.
2. Reemplaza la estrategia `PreloadAllModules` en la llamada de `forRoot()` con este `SelectivePreloadingStrategyService`.
3. Agrega la estrategia `SelectivePreloadingStrategyService` al array de proveedores `AppRoutingModule` para que puedas inyectarlo en cualquier parte de la aplicación.

Ahora edita el `AdminDashboardComponent` para mostrar el registro de rutas pre cargadas.

1. Importa el `SelectivePreloadingStrategyService`.
2. Inyéctalo en el contructor del dashboard.
3. Actualiza la plantilla para mostrar el array `preloadedModules` del servicio de estrategia.

Ahora el archivo es el siguiente:

<code-example path="router/src/app/admin/admin-dashboard/admin-dashboard.component.ts" header="src/app/admin/admin-dashboard/admin-dashboard.component.ts (preloaded modules)"></code-example>

Una vez que la aplicación carga la ruta inicial, él `CrisisCenterModule` es pre cargado.
Verifica esto iniciando sesión en el área de función del `Admin` y observa que el `crisis-center` está listado en el `Preloaded Modules`.
Incluso se registra en la consola del navegador.

{@a redirect-advanced}
{@a migrating-urls-with-redirects}

### Migrando URLs con redireccionamientos

Has configurado las rutas para navegar por tu aplicación y utilizado la navegación de manera imperativa y declarativa.
Pero como cualquier aplicación, los requisitos cambian con el tiempo.
Has configurado enlaces y navegación a `/heroes` y `/hero/:id` desde los componentes `HeroListComponent` y `HeroDetailComponent`.
Si existiera el requisito de que los enlaces a `heroes` se convirtieran en `superheroes`, aún querrías que las URLs anteriores navegaran correctamente.
Tampoco deseas actualizar cada enlace en tu aplicación, por lo que los redireccionamientos hacen que la refactorización de rutas sea trivial.

{@a url-refactor}
{@a changing-heroes-to-superheroes}

#### Cambiando `/heroes` a `/superheroes`

Esta sección te guía a través de la migración de las rutas `Hero` a nuevas URLs.
El `Router` comprueba si hay redireccionamientos en su configuración antes de navegar, porque cada redireccionamiento se activa cuando es necesario. Para respaldar este cambio, agrega redireccionamientos de las rutas antiguas a las nuevas rutas en el `heroes-routing.module`.

<code-example path="router/src/app/heroes/heroes-routing.module.ts" header="src/app/heroes/heroes-routing.module.ts (heroes redirects)"></code-example>

Observa que hay dos diferentes tipos de redireccionamiento.
El primero cambia de `/heroes` a `/superheroes` sin ningún parámetro.
El segundo cambia de `/hero/:id` a `/superhero/:id`, el cual incluye el parámetro de ruta `:id`.
El redireccionamiento del enrutador también utiliza una potente coincidencia de patrones, por lo que el `Router` inspecciona la URL y reemplaza los parámetros de ruta en el `path` con el destino apropiado.
Anteriormente, navegaba a una URL como `/hero/15` con un parámetro de ruta `id` de `15`.

<div class="alert is-helpful">

El `Router` también admite [parámetros de consulta](#query-parameters) y fragmentos [fragment](#fragment) cuando usa redireccionamientos.

- Al usar redireccionamientos absolutos, el `Router` usará los parámetros de consulta y el fragmento `redirectTo` en la configuración de ruta.
- Cuando se usan redireccionamientos relativos, el `Router` usa parámetros de consulta y el fragmento de la URL de origen.

</div>

Actualmente, la ruta vacía redirige a `/heroes`, que redirige a `/superheroes`.
Esto no funciona porque el `Router` maneja el redireccionamiento una vez en cada nivel de configuración del enrutamiento.
Esto evita el encadenamiento de redireccionamientos, lo que puede dar lugar a bucles de redireccionamiento interminables.

En su lugar, actualiza la ruta vacía en `app-routing.module.ts` para redirigir a `/superheroes`.

<code-example path="router/src/app/app-routing.module.ts" header="src/app/app-routing.module.ts (superheroes redirect)"></code-example>

Un `routerLink` no está vinculado a la configuración de la ruta, así que actualiza los enlaces del enrutador asociados para que permanezcan activos cuando la nueva ruta esté activa.
Actualiza la plantilla `app.component.ts` para el `/heroes` `routerLink`.

<code-example path="router/src/app/app.component.html" header="src/app/app.component.html (superheroes active routerLink)"></code-example>

Actualiza el método `goToHeroes()` en el archivo `hero-detail.component.ts` para navegar de regreso a `/superheroes` con los parámetros de ruta opcionales.

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.ts" region="redirect" header="src/app/heroes/hero-detail/hero-detail.component.ts (goToHeroes)"></code-example>

Con la configuración de redireccionamientos, todas las rutas anteriores ahora apuntan a sus nuevos destinos y ambas URLs funcionan según lo previsto.

{@a inspect-config}
{@a inspect-the-routers-configuration}

### Inspecciona la configuración del enrutador

Para determinar si las rutas se evalúan [en el orden correcto](#routing-module-order), puedes inspecciona la configuración del enrutador.

Haz esto inyectando el enrutador y registrando en la consola su propiedad `config`.
Por ejemplo, actualiza el `AppModule` de la siguiente manera y mira en la consola del navegador para ver la configuración de la ruta terminada.

<code-example path="router/src/app/app.module.7.ts" header="src/app/app.module.ts (inspect the router config)" region="inspect-config"></code-example>

{@a final-app}

## Aplicación Final

Para obtener la aplicación de enrutador completa, consulta <live-example name="router"></live-example> para obtener el código fuente final.

{@a link-parameters-array}
