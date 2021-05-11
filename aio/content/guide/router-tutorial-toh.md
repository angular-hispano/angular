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

Tu puedes presionar "Ok" y perder tus cambios o hacer clic en "Cancel" y continuar editando.

Detras de este comportamiento se encuentra el guard `CanDeactivate` del enrutador.
Los guards te dan la oportunidad de limpiar o pedir permiso al usuario antes de navegar fuera de la vista actual.

Los botones de `Admin` y `Login` muestran otras capacidades del enrutador que se cubriran mas adelante en la guía.

{@a getting-started}

## Hito 1: Primeros pasos

Comienza con una version basica de la app que navega entre dos vistas vacias.
Begin with a basic version of the app that navigates between two empty views.

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

La primera configuración define una matriz de dos elementos con las rutas minimas que conducen a `CrisisListComponent` y `HeroListComponent`.

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
El método `RouterModule.forRoot()` devuelve un modulo que contiene el proovedor de servicios del `Router` configurado, ademas de otros proveedores que requiere la biblioteca del enrutador.
Una vez que inicia la aplicación, el `Enrutador` realiza la navegación inicial basado en la URL actual del navegador.

<div class="alert is-important">

**Nota:** El método `RouterModule.forRoot()` es un patron usado para registrar los proveedores de toda la aplicación. Puedes encontrar más información sobre los proveedores de la aplicación en la guía de [servicios Singleton](guide/singleton-services#forRoot-router).

</div>

<code-example path="router/src/app/app.module.1.ts" header="src/app/app.module.ts (first-config)" region="first-config"></code-example>

<div class="alert is-helpful">

Agregar el `RouterModule` configurado al `AppModule` es suficiente para una configuración minima de las rutas.
Sin embargo, a medida que la aplicación crece, puedes realizar una [refactorización de la configuración del enrutador](#refactor-the-routing-configuration-into-a-routing-module) en un archivo separado y crear un [Modulo de Rutas](#routing-module).
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

La plantilla correspondiente al componente se ve asi:

<code-example path="router/src/app/app.component.1.html" header="src/app/app.component.html"></code-example>

{@a wildcard}

### Definir una ruta comodín

Hasta ahora has creado dos rutas en la app, una a `/crisis-center` y la otra a `/heroes`.
Cualquier otra URL causara que el enrutador arroje un error y bloquee la app.

Agrega un ruta comodín para interceptar URLs invalidas y manejarlas con elegancia.
Una ruta comodín tiene una ruta que consta de dos asteriscos.
Coincide con cada URL.
Por lo tanto, el enrutador selecciona esta ruta comodín si no coincide con otra ruta en la configuración.
Una ruta comodín puede navegar a un componente personalizado de "404 Not Found" o [redireccionar](#redirect) a otra ruta existente.

<div class="alert is-helpful">

El enrutador selecciona la ruta con la estrategia [_primera coincidencia gana_](/guide/router#example-config).
Dado que la ruta comodín es la ruta menos especifica, colóquela en el último lugar en la configuración de la ruta.

</div>

Para probar esta funcionalidad, agrega un botón a `RouterLink` a la platilla `HeroListComponent` y establece un vínculo a una ruta aun no existente llamada `"/sidekicks"`.

<code-example path="router/src/app/hero-list/hero-list.component.1.html" header="src/app/hero-list/hero-list.component.html (excerpt)"></code-example>

La aplicación fallara si el usuario hace clic en ese botón porque aún no hemos definido la ruta `"/sidekicks"`.

En lugar de agregar la ruta `"/sidekicks"`, define una ruta `comodín` y haz que navegue hasta el componente `PageNotFoundComponent`.

<code-example path="router/src/app/app.module.1.ts" header="src/app/app.module.ts (wildcard)" region="wildcard"></code-example>

Crea el componente `PageNotFoundComponent` para que se muestre, cuando los usuarios visiten URLs no válidas.

<code-example language="none" class="code-shell">
  ng generate component page-not-found
</code-example>

<code-example path="router/src/app/page-not-found/page-not-found.component.html" header="src/app/page-not-found.component.html (404 component)"></code-example>

Ahora cuando el usuario vista `/sidekicks`, o cualquier otra URL no válida, el navegador muestra "Page not found".
La barra de direcciones del navegador sigue apuntando a la URL no válida.

{@a redirect}

### Configurar redireccionamientos

Cuando se inicia la aplicación, la URL inicial en la barra del navegador es la predeterminada:

<code-example>
  localhost:4200
</code-example>

Esto no coincide con ninguna de las rutas codificadas, lo que significa que el enrutados pasa la ruta comodín y muestra el componente `PageNotFoundComponent`.

La aplicación necesita una ruta predeterminada a una página válida.
La página predeterminada de esta aplicación es la lista de héroes.
La aplicación deberia de navegar ahí como si el usuario hubiera hecho clic en el enlace "Héroes" ó hubiera escrito `localhost:4200/heroes` en la barra de direcciones.

Agrega una ruta de `redireccionamiento` que traduzca la URL inicial relativa (`''`) hacia la ruta predeterminada deseada (`/heroes`).

Agrega la ruta predeterminada en algun lugar _por encima_ de la ruta comodín.
Es justo encima de la ruta comodín, en el siguiente extracto que muestra las `appRoutes` completas para este hito.

<code-example path="router/src/app/app-routing.module.1.ts" header="src/app/app-routing.module.ts (appRoutes)" region="appRoutes"></code-example>

La barra de direcciones del navegador muestra `.../heroes` como si se hubiera navegado allí directamente.

Una ruta de redireccionamiento requiere una propiedad `pathMatch` para decirle al enrutador como hacer coincidir la URL con el ruta.
En esta aplicación, el enrutador debe seleccionar la ruta de `HeroListComponent` solamente cuando la _URL completa_ coincida `''`, asi que configura el `pathMatch` con el valor de `'full'`.

{@a pathmatch}

<div class="callout is-helpful">

  <header>Enfocandose en pathMatch</header>

Técnicamente, `pathMatch = 'full'` da como resultado un acierto de ruta cuando los segmentos _restantes_ no coincidentes de la URL coinciden con `''`.
En este ejemplo, el redireccionamiento está en una ruta de nivel superior, por lo que la URL _restante_ y la URL _completa_ son la misma cosa.

El otro posible valor de `pathMatch` es `'prefix'`, que le dice al enrutador que coincida con la ruta de redireccionamiento cuando la URL restante comience con el prefijo de la ruta de redireccionamiento.
Esto no se aplica a esta aplicación de muestra por que si el valor de `pathMatch` fuera `'prefix'`, cada URL coincidiria con `''`.

Intenta establecer el `'prefix'` y haz clic en el botón `Go to sidekicks`.
Como es una URL incorrecta, deberias de ver la página de "Page not found".
En cambio, todavía estás en la página de "Héroes".
Ingresa una URL incorrecta en la barra de direcciones del navegador.
Seras redireccionado instantaneamente hacia `/heroes`.
Cada URL, correcta o incorrecta, que entra en esta definicion de ruta es una coincidencia.

La ruta predeterminada deberia redirigir al componente `HeroListComponent` solo cuando la URL completa sea `''`.
Recuerda restaurar la redirección de `pathMatch = 'full'`.

Más información en Victor Savkin's
[Publicar en redireccionamientos](http://vsavkin.tumblr.com/post/146722301646/angular-router-empty-paths-componentless-routes).

</div>

### Resumen del Hito 1

Tu aplicación de muestra puede cambiar entre dos vistas cuando el usuario hace clic en un enlace.

El hito 1 ha cubierto cómo hacer lo siguiente:

- Cargar la libreria del enrutador.
- Agregaste una barra de navegación a la coraza de la plantilla con las etiquetas de anclaje, y las directivas `routerLink` y `routerLinkActive`.
- Agregaste un `router-outlet` a la coraza de la plantilla donde se muestran las vistas.
- Configuraste el modulo del enrutador con `RouterModule.forRoot()`.
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

Este hito muestra como configurar un modulo de proposito especial llamado _Módulo de Enrutamiento_, el cual contiene la configuración de enrutamiento de tu aplicación.

El Módulo de Enrutamiento contiene varias caracteristicas:

- Separa las responsabilidades de enrutamiento de otras responsabilidades de la aplicación.
- Provee de un módulo para reemplazar o remover al probar la aplicación.
- Provee un espacio especifico para los proveedores de servicios de enrutamiento como guards y resolvers.
- No declara componentes.

{@a integrate-routing}

### Integrar el enrutamiento con tu aplicación

La aplicación de enrutamiento de muestra no incluye un enrutador preestablecido.
Cuando tu uses el [Angular CLI](cli) para crear un proyecto que use el enrutador, debes de establecer la opción de `--routing` para el proyecto o la aplicación, y para cada NgModule.
Cuando tu crear o inicializas un proyecto nuevo (usando el comando de CLI [`ng new`](cli/new)) o una nueva aplicación (usando el comando [`ng generate app`](cli/generate)), especifica la opción de `--routing`.
Esto le dira al CLI que incluya el paquete de npm `@angular/router` y cree un archivo llamado `app-routing.module.ts`.
Tu puedes usar ese enrutador en cualquier NgModule que agregues a tu proyecto ó aplicación.

Por ejemplo, el siguiente comando genera un NgModule que puede usar ese enrutador.

```sh
ng generate module my-module --routing
```

Esto crea un archivo separado llamado `my-module-routing.module.ts` para almacenar las rutas del NgModule.
El archivo incluye un objecto de `Ruta` vacia que puedes llenar con rutas de diferentes componentes y NgModules.

{@a routing-refactor}
{@a refactor-the-routing-configuration-into-a-routing-module}

### Integrar el enrutamiento con tu aplicación

Crea un módulo llamado `AppRouting` en el directorio `/app` para guardar la configuración del enrutador.

<code-example language="none" class="code-shell">
  ng generate module app-routing --module app --flat
</code-example>

Importa los simbolos `CrisisListComponent`, `HeroListComponent`, y `PageNotFoundComponent` tal y mo lo hiciste en `app.module.ts`.
A continuación mueve la importación del `Ennrutador` y la configuración de enrutamiento, incluyendo `RouterModule.forRoot()`, dentro de este módulo de enrutamiento.

Vuelve a exportar el `RouterModule` de Angular agregandola al array de `exports` del modulo.
Al volver a exportar el `RouterModule` aquí, los componentes declarados en `AppModule` tienen acceso a las directivas del enrutador como `RouterLink` y `RouterOutlet`.

Despues de estos pasos, el achivo deberia de verse asi:

<code-example path="router/src/app/app-routing.module.1.ts" header="src/app/app-routing.module.ts"></code-example>

A continuación, actualiza el archivo `app.module.ts` removiendo el `RouterModule.forRoot` en el array de `imports`.

<code-example path="router/src/app/app.module.2.ts" header="src/app/app.module.ts"></code-example>

<div class="alert is-helpful">

Despues, esta guía te mostrara como crear [multiples módulos de enrutamiento](#heroes-functionality) e importarlos [en el orden correcto](#routing-module-order).

</div>

La aplicación continua trabajando exatamente igual, y ahora puedes usar el `AppRoutingModule` como centro para mantener futuras configuraciones de enrutamiento.

{@a why-routing-module}

### Beneficios de un modulo de enrutamiento

El módulo de enrutamiento, usualmente es llamado `AppRoutingModule`, reemplaza la configuración de enrutamiento de la raíz or el módulo de funciones.

El módulo de enrutamiento es muy util cuando tu aplicación crece y cuando la configuración incluye servicios de guards y resolvers especializados.

Algunos desarrolladores omiten el modulo de enrutamiento cuando la configuración es mínima y combinan la configuración de enrutamiento directamente en el modulo complementario (por ejemplo, `AppModule`).

La mayoria de las aplicaciónes deberian de implementar un modulo de enrutamiento para mantener la coherencia.
Mantiene el codigo limpio cuando la configuración se vuelve compleja.
Esto facilita las pruebas en el módulo de funciones.
Su existencia llama la atención sobre el hecho de que un módulo esta enrutado.
Es aquí donde los desarrolladores esperan encontrar y expendir la configuración de enrutamiento.

{@a heroes-feature}

## Hito 3: Funciones de los Héroes

Este hito cubre lo siguiente:

- Organizar la aplicación y las rutas en áreas funcionales usando los módulos.
- Navegar imperativamente de un componente a otro.
- Pasar la información requerida u opcional en parámetros de rutas.

Esta aplicación de muesta recrea la funcionalidad de los héroes en la sección de "Services" del [Tutorial Tour de Héroes](tutorial/toh-pt4 "Tour de Héroes: Servicios"), y reusa mucho del codigo de <live-example name="toh-pt4" title="Tour of Héroes: Codigo de ejemplo de servicios"></live-example>.

<!-- KW - this gif isn't ideal for accessibility. Would like to remove it.-->
<!-- Here's how the user will experience this version of the app:


<div class="lightbox">
  <img src='generated/images/guide/router/router-2-anim.gif' alt="App in action">
</div> -->

Una aplicación tipica tiene multiples áreas funcionales, cada una dedicada a un proposito comercial particular con su propia carpeta.

Esta sección muesta como refactorizar la aplicación en diferentes módulos funcionales, importalos en el módulo principal y navegar entre ellos.

{@a heroes-functionality}

### Agregar funcionalidades de héroes

Sigue los siguientes pasos:

- Para administrar los héroes, crea un `HeroesModule` con enrutador en la carpeta de heroes y regístralo en la raíz `AppModule`.

<code-example language="none" class="code-shell">
  ng generate module heroes/heroes --module app --flat --routing
</code-example>

- Mueve el contenido de la carpeta `hero-list` que se encuentra en la carpeta `app` a la carpeta de `heroes`.
- Copia el contenido de `heroes/heroes.component.html` desde el <live-example name="toh-pt4" title="Tour de Héroes: Codigo de ejemplo de servicios">tutorial de "Services"</live-example> en la plantilla `hero-list.component.html`.

  - Cambia la etiqueta `<h2>` con `<h2>HEROES</h2>`.
  - Borra el componente `<app-hero-detail>` que esta en la parte inferior de la plantilla.

- Copia el contenido de `heroes/heroes.component.css` del ejemplo en vivo al archivo `hero-list.component.css`.
- Copia el contenido de `heroes/heroes.component.ts` del ejemplo en vivo al archivo `hero-list.component.ts`.

  - Cambia el nombre de la clase del componente a `HeroListComponent`.
  - Cambia el `selector` a `app-hero-list`.

<div class="alert is-helpful">

Los selectores no son requeridos por los componentes de enrutamiento por que los componentes son insertados dinamicamente cuando la página es renderizada. Sin embargo, son utiles para identificarlos en los elementos HTML del DOM.

</div>

- Copia la carpeta `hero-detail`, los archivos `hero.ts`, `hero.service.ts` y `mock-heroes.ts` dentro de la subcarpeta `heroes`.
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

Las funcionalidades de los héroes tienen dos componentes que interactuan, la lista de héroes y el detalle de los héroes.
Cuando tu navegas a la vista de lista, se obtiene una lista de héroes y la muestra.
Cuando haces clic en un héroe, el detalle de la vista debe de mostrar ese héroe en particular.

Tu puedes decirle a la vista de detalle cual es el héroe que mostrara, incluyendo el id del héroe seleccionado en la URL de la ruta.

Debes de importar los componente de héroe a sus nueva ubicación en la carpeta `src/app/heroes/` y definir las dos rutas del héroe.

Ahora que tienes las rutas del módulo de `Heroes`, registralas en el `Router` mediante el modulo `RouterModule` como lo hiciste anteriormente en `AppRoutingModule`, con una diferencia importante.

En el `AppRoutingModule`, usaste el metodo estatico `RouterModule.forRoot()` para registrar las rutas y el nivel de aplicación de los proveedores de servicio.
En el modulo de funciones usa el método estatico `forChild()`.

<div class="alert is-helpful">

Solo llama `RouterModule.forRoot()` en la raíz `AppRoutingModule` (o el `AppModule` si es donde registraras las rutas de nivel superior de la aplicación).
En cualquier otro módulo, tu debes de llamar el metodo `RouterModule.forChild()` para registrar las rutas adicionales.

</div>

El modulo `HeroesRoutingModule` se ve así:

<code-example path="router/src/app/heroes/heroes-routing.module.1.ts" header="src/app/heroes/heroes-routing.module.ts"></code-example>

<div class="alert is-helpful">

Considera darle a cada módulo de funciones su propio archivo de configuraciones de rutas.
Aunque las funciones de las rutas son minimas, estas tienen la tendencia de crecer y volverse mas complejas, incluso en aplicaciones pequeñas.

</div>

{@a remove-duplicate-hero-routes}

#### Remover rutas de héroes duplicadas

Las rutas de los héroes estan actualmente definidas en dos lugares: en el `HeroesRoutingModule` a traves del `HeroesModule` y en el `AppRoutingModule`.

Las rutas proporcionadas por los módulos de funciones se combinan en los módulos de rutas importados por el enrutador.
Esto te permite continuar definiendo los módulos de funciones de rutas sin tener que modificar la configuración de la ruta principal.

Eliminar la importación del `HeroListComponent` y la ruta `/heroes` del archivo `app-routing.module.ts`.

Deja las rutas predeterminadas y las rutas de comodín, ya que todavia se estan usando en el nivel superior de la aplicación.

<code-example path="router/src/app/app-routing.module.2.ts" header="src/app/app-routing.module.ts (v2)"></code-example>

{@a merge-hero-routes}

#### Remover las declaraciones de los héroes

Debido a que `HeroesModule` provee el `HeroListComponent`, puedes eliminarlo de array `declarations` en el `AppModule`'s.
Ahora que tienes un `HeroesModule` separado, tu puedes evolucionar las funcionalidades del héroe con mas componentes y diferentes rutas.

Despues de estos pasos el `AppModule` debería verse así:

<code-example path="router/src/app/app.module.3.ts" header="src/app/app.module.ts" region="remove-heroes"></code-example>

{@a routing-module-order}

### Orden de importación de módulos

Observa que en el array de `imports` del modulo, `AppRoutingModule` es el último y viene _después_ del `HeroesModule`.

<code-example path="router/src/app/app.module.3.ts" region="module-imports" header="src/app/app.module.ts (module-imports)"></code-example>

El orden de configuración de la ruta es importante por que el enrutador acepta la primera ruta que coincida con la ruta de navegación solicitada.

Cuando todas las rutas estan en un `AppRoutingModule`, tu pones las rutas predeterminadas y [comodín](#wildcard) al final, desíes de la ruta de `/heroes`, para que el enrutador tenga la oportunidad de encontrar la URL que coincide con la ruta de `/heroes` _antes_ de llegar a la ruta comodín y navegar a la página "Page not found".

Cada modulo de enrutamiento aumenta la configuración de la ruta en el orden de importación.
Si tu pones `AppRoutingModule` primero, la ruta comodón se encontraria _antes_ que la ruta de héroes.
La ruta comodín &mdash;coincide con _cada_ URL&mdash; e interceptaria el intento de navegar a la ruta de héroe.

<div class="alert is-helpful">

Invierte el orden del modulo de enrutamiento para ver que al hacer un clic en el enlace de héroes seras llebado a la página "Page not found".
Obten mas información acerca de como inspeccionar la configuración del enrutador en tiempo de ejecución [a continuación](#inspect-config "Inspeccionar la configuración del enrutador").

</div>

### Parámetros de rutas

{@a route-def-with-parameter}

#### Definición de una ruta con parámetros

Regresa al `HeroesRoutingModule` y mira las definiciones de las rutas de nuevo.
La ruta hacia `HeroDetailComponent` tiene un token `:id` en la ruta.

<code-example path="router/src/app/heroes/heroes-routing.module.1.ts" header="src/app/heroes/heroes-routing.module.ts (excerpt)" region="hero-detail-route"></code-example>

El token `:id` crea un espacio en la ruta para un parametro de ruta.
En este caso, esta configuración cause que el enrutador inserte el `id` de un héroe en ese espacio.

Si tu llamas la ruta para navegar al compomente de detalle y muestras "Magneta", tu esperas que el `id` del héroe aparezca en la URL del navegador como esta:

<code-example format="nocode">
  localhost:4200/hero/15

</code-example>

Si un usuario escribe esa URL en la barra de direcciones de su navegador, el enrutador deberia de reconocer el patron e ir a la misma vista de detalle de "Magneta".

<div class="callout is-helpful">

<header>
  Parametro de ruta: ¿obligatorio u opcional?
</header>

Al incrustar el token del parametro de ruta, `:id`, en la definicion de rutas es una buena opcion para este escenario, ya que el `:id` es _requerido_ por el `HeroDetailComponent` y el valor de `15` en la ruta, se distingue claramente de la ruta "Magneta" a cualquier ruta para otro héroe.

</div>

{@a route-parameters}

#### Configurando los parámetros de la ruta en la vista de lista

Despues de navegar a `HeroDetailComponent`, tu esperas ver los detalles del héroe seleccionado.
Para esto se necesitan dos piezas de información: la ruta de enrutamiento al componente y el `id` del héroe.

Por lo que, el _array de parámetros del enlace_ debe tener dos elementos: la _ruta_ del enrutador y el _parametro de ruta_ que especifica el `id` del héroe selecciondo.

<code-example path="router/src/app/heroes/hero-list/hero-list.component.1.html" header="src/app/heroes/hero-list/hero-list.component.html (link-parameters-array)" region="link-parameters-array"></code-example>

El enrutador compone la URL de destino desde el array como esto: `localhost:4200/hero/15`.

El enrutador extrae el parametro de la ruta (`id:15`) de la URL y lo proporciona a `HeroDetailComponent` a través del servicio `ActivatedRoute`.

{@a activated-route-in-action}

### `Ruta activada` en acción

Importa el `Router`, `ActivatedRoute`, y `ParamMap` del paquete del enrutador.

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.1.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (activated route)" region="imports"></code-example>

Importaremos el operador `switchMap` por que lo necesitaremos mas tarde para procesar los parámetros de la ruta `Observable`.

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.3.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (switchMap operator import)" region="rxjs-operator-import"></code-example>

{@a hero-detail-ctor}

Agrega los servicios como variables privadas al constructor para que Angular las inyecte (y los haga visibles al componente).

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.3.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (constructor)" region="ctor"></code-example>

En el método `ngOnInit()`, usa el servicio de `ActivatedRoute` para recuperar los parámetros de la ruta, extrae el `id` del héroe de los parámetros y recuperalo para mostrar el héroe.

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.3.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (ngOnInit)" region="ngOnInit"></code-example>

Cuando el mapa cambie, `paramMap` tomara el parametro `id` de los parámetros modificados.

Entonces tu le diras al `HeroService` que busque el héroe con ese `id` y devuelva el resultado de la solicitud de `HeroService`.

El operador `switchMap` hace dos cosas. Aplana el `Observable<Hero>` que devuelve `HeroService` y cancela las solicitudes pendientes anteriores.
Si el usuario vuelve a navegar hacia la ruta con el nuevo `id` mientras el `HeroService` esta aun devolviendo un `id` anterior, `switchMap` descartara esa solicitud vieja y regresara el héroe para el nuevo `id`.

`AsyncPipe` maneja la subscripción al observable y la propiedad `hero` del componente se re-establecera con el héroe recuperado.

#### _ParamMap_ API

The API `ParamMap` esta inspirada en la [interface URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams).
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

    Devuelve `true` si el nombre del parametro esta en el mapa de parámetros .

    </td>

  </tr>

  <tr>
    <td>
      <code>get(name)</code>
    </td>
    <td>

    Devuelve el nombre del valo del parametro (como `string`) si se encuentra, o `null` si el nombre del parametro no esta en el mapa. Regresa el _primer_ elemento si el valor del parametro es un array de valores.

    </td>

  </tr>

  <tr>
    <td>
      <code>getAll(name)</code>
    </td>
    <td>

    Devuelve un `string array` con el nombre del valor del parametro si lo encuentra, o un `array` vacio si el nombre del valor del parametro no esta en el mapa. Usa `getAll` cuando un solo parametro puede tener varios valores.

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

En este ejemplo, recuperaras el mapa del parametro de ruta de un `Observable`.
Esto implica que el mapa de parámetros de ruta pueda cambiar durante la vida útil de este componente.

De manera predeterminada, el enrutador reutiliza la instancia del componente cuando vuelve a navegar componente del mismo tipo sin vistar un componente diferent. Los parámetros de la ruta puede cambiar cada vez.

Supongamos que la barra de navegación de un componente principal tiene botones "Avanzar" y "Retroceder" para desplazarse por la lista de héroes.
En cada clic navegara imperativamente al `HeroDetailComponent` con el `id` siguiente o previo.

No querra que el enrutador elimine la instancia actual de `HeroDetailComponent` del DOM solo para volver a crearla para el proximo `id` ya que esto renderizara la vista de nuevo.
Para una mejor UX, el enrutados reutiliza la misma instancia del componente y actualiza el parametro.

Dado que `ngOnInit()` solo se llama una vez por cada instanciación de un componente, puedes detectar cuando los parámetros de la ruta cambian _dentro de la misma instancia_ utilizando la propiedad observable `paramMap.

<div class="alert is-helpful">

Cuando se suscribe a un observable en un componente, casi siempre se da de baja esta subscripción cuando el componente se destruye.

Sin embargo, los observables de `ActivatedRoute` estan entre las excepciones por que `ActivatedRoute` y sus observables estan aislados del `Router`.
El `Router` destruye un componente de ruta cuando ya no se necesita junto con el `ActivatedRoute` inyectado.

</div>

{@a snapshot}
{@a snapshot-the-no-observable-alternative}

#### `snapshot`: la alternativa no observable

Esta aplicación no reutilizará el `HeroDetailComponent`.
El usuario siempre regresara a la lista de héroes para seleccionar otro héroe en la vista.
No hay manera de navegar del detalle de un héroe al detalle de otro héroe sin visitar el componente de lista.
Por lo tanto, el enrutador crea una nueva instancia `HeroDetailComponent` cada vez.}

Cuando sepas con certeza que una instancia de `HeroDetailComponent` nunca sera reutilizada, puedes usar un `snapshot`.

`route.snapshot` proporciona el valor inicial del mapa de parámetros de ruta.
Puedes acceder a los parámetros directamente sin subscribirse o agregar operadoradores observables como se muestra a continuación:

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

Tambien puedes agregar información opcional a una solicitud de la ruta.
Por ejemplo, al regresar a la lista `hero-detail.component.ts` desde la vista de detalles del héroe, seria agradable si el héroe visto estuviera pre-seleccionado en la lista.

<div class="lightbox">
  <img src='generated/images/guide/router/selected-hero.png' alt="Héroe seleccionado">
</div>

Para implementar esta funcionalidad al incluir el `id` del héroe visto en la URL como parámetro opcional cuando regresas del `HeroDetailComponent`.

La información opcional tambien puede incluir:

- Criterios de busqueda poco estructurados; por ejemplo: `name='wind*'`.
- Multiples valores; por ejemplo, `after='12/31/2015' & before='1/1/2017'`&mdash;sin un orden en particula&mdash;`before='1/1/2017' & after='12/31/2015'`&mdash; en una variedad de formatos&mdash;`during='currentYear'`.

Como este tipo de parámetros no encajan fácilmente en la ruta URL, puedes usar los parámetros opcionales para transmitir información arbitrariamente compleja durante la navehación.
Los parámetros opcionales no estan involucrados en los patrones de coincidencia y permiten una expresión mas flexible.

El enrutador permite la navegación con parámetros opcionales, así como los parámetros de ruta requeridos.
Define los parámetros opcionales en una objeto separado _despues_ de definir los parámetros de ruta requeridos.

En general, utiliza los parámetros de ruta requeridos cuando el valor es obligatorio (por ejemplo, si es necesario distinguir entre una ruta de otra): y agrega un parametro opcional cuando el valor es opcional, complejo y/o variado.

{@a optionally-selecting}

#### Lista de Héroes: Seleccionado a un héroe opcional

Al navegar a `HeroDetailComponent` se especifico el `id` requerido del héroe-a-editar en el parametro y se convirtio en el segundo elemento del [_array de parámetros del enlace_](#link-parameters-array).

<code-example path="router/src/app/heroes/hero-list/hero-list.component.1.html" header="src/app/heroes/hero-list/hero-list.component.html (link-parameters-array)" region="link-parameters-array"></code-example>

El enrutador incrusta el valor `id` en la URL de la navegación por que esta definido en los parámetros de ruta como `:id` en la ruta `path`:

<code-example path="router/src/app/heroes/heroes-routing.module.1.ts" header="src/app/heroes/heroes-routing.module.ts (hero-detail-route)" region="hero-detail-route"></code-example>

Cuando el usuario hace clic en el botón Atras, el `HeroDetailComponent` construye otro _array de parámetros de enlance_ que se utiliza para navegar de regreso al `HeroListComponent`.

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.1.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (gotoHeroes)" region="gotoHeroes"></code-example>

Este array carece de parámetros de ruta por que previamente no era necesario enviar información al `HeroListComponent`.

Ahora, enviamos el `id` del héroe actual con la solicitud de navegación por eso el `HeroListComponent` puede destacar el héroe en esta lista.

Enviar el `id` con un objeto que contiene un parametro `id` opcional. Para fines de demostración, hay un parámetro basura extra (`foo`) en el objeto que el `HeroListComponent` deberia de ignorar. Aquí esta la declaración de la navegación:

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.3.ts" header="src/app/heroes/hero-detail/hero-detail.component.ts (go to heroes)" region="gotoHeroes"></code-example>

La aplicación aun funciona. Haciendo clic en "Retroceso" regresa a la vista de lista del héroe.

Mira la barra de direcciones del navegador.

Deberia de verse algo como esto, dependiendo de donde lo ejecutes:

<code-example language="bash">
  localhost:4200/heroes;id=15;foo=foo

</code-example>

El valor `id` aparece en la URL como (`;id=15;foo=foo`), no en la ruta de la URL.
La ruta para "Heroes" no tiene un token `:id`.

Los parámetros de ruta opcionales no estan separador por un "?" o "&" como estarian en la cadena de consulta de la URL.
Estan separador por punto y coma ";".
Esta es la notación de una matriz URL.

<div class="alert is-helpful">

La notación de una matriz URL es una idea introducida por primera vez en [propuesta de 1996](http://www.w3.org/DesignIssues/MatrixURIs.html) por el fundador de la web, Tim Berners-Lee.

Aunque la notación de matriz nunca se incorporo al estandar de HTML, es legal y se hizo popular entre los sistemas de enrutamiento del navegador como una forma de aislar los parámetros que pertenecen a las rutas padres e hijas.
Como tal, el Enrutador proporciona soporte a la notación de matriz en todos los navegadores.

</div>

{@a route-parameters-activated-route}

### Parámetros de ruta en el servicio `ActivatedRoute`

En el estado actual del desarrollo, la lista de heroes no ha cambiado.
No se resalta ninguna fila de héroe.

El `HeroListComponent` necesita el codigo que espera parámetros.

Previamente, cuando navegamos desde el `HeroListComponent` hacia el `HeroDetailComponent`, es necesario suscribirse al mapa de parámetros de ruta `Observable` y ponerlo a disposición del `HeroDetailComponent` en el servicio de `ActivatedRoute`.
Este servicio fue inyectado en el contructor de `HeroDetailComponent`.

Esta vez navagaras en dirección opuesta, desde el `HeroDetailComponent` al `HeroListComponent`.

Primero, extenderemos la declaración de importación del enrutador para incluir el servicio de `ActivatedRoute`.

<code-example path="router/src/app/heroes/hero-list/hero-list.component.ts" header="src/app/heroes/hero-list/hero-list.component.ts (import)" region="import-router"></code-example>

Importa el operador `switchMap` para realizar una operacion en el mapa de parámetros de ruta `Observable`.

<code-example path="router/src/app/heroes/hero-list/hero-list.component.ts" header="src/app/heroes/hero-list/hero-list.component.ts (rxjs imports)" region="rxjs-imports"></code-example>

Inyecta el `ActivatedRoute` en el contructor `HeroListComponent`.

<code-example path="router/src/app/heroes/hero-list/hero-list.component.ts" header="src/app/heroes/hero-list/hero-list.component.ts (constructor and ngOnInit)" region="ctor"></code-example>

La propiedad `ActivatedRoute.paramMap` es un mapa de ruta de parámetros `Observable`.
`paramMap` emite un nuevo mapa de valores que incluye el `id` cuando el usuario navega al componente.
En `ngOnInit()` te suscribes a esos valores, estableces el `selectedId` y obtienes los héroes.

Actualiza la plantialla con un [enlace de clase](guide/attribute-binding#class-binding).
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

Primero, importa el `BrowserAnimationsModule` y agregalo al array de `imports`:

<code-example path="router/src/app/app.module.ts" header="src/app/app.module.ts (animations-module)" region="animations-module"></code-example>

A continuación, agrega un objeto `data` a las rutas del `HeroListComponent` y `HeroDetailComponent`.
Las transiciones se basan en `states` y se utilizan los dartos de `animation` desde el enrutador que proporciona un `state` con nombre `animation` para las transiciones.

<code-example path="router/src/app/heroes/heroes-routing.module.2.ts" header="src/app/heroes/heroes-routing.module.ts (animation data)"></code-example>

Crea un archivo `animations.ts` en la carpeta raíz `src/app/`. El contenido se ve así:

<code-example path="router/src/app/animations.ts" header="src/app/animations.ts (excerpt)"></code-example>

Este archivo realiza lo siguiente:

- Importa los simbolos de animación que crean los disparados de la animación, controlan el estado, y administran la transición entre estados.

- Exporta una constante llamada `slideInAnimation` y establece un disparador de animación llamado `routeAnimation`.

- Define una transición cuando se alterna entre las rutas `heroes` y `hero` que hace una animación para el componente desde la izquierda de la pantalla cuando entra a la vista de aplicación (`:enter`), la otra anima el componente a la derecha cuando sale de la vista de la aplicación (`:leave`).

De vuelta al `AppComponent`, importa el token `RouterOutlet` desde el paquete `@angular/router` y el `slideInAnimation` de `'./animations.ts`.

Agrega el array `animations` a los metadatos de `@Component` que contiene `slideInAnimation`.

<code-example path="router/src/app/app.component.2.ts" header="src/app/app.component.ts (animations)" region="animation-imports"></code-example>

Para usar las animaciones enrutables, envuelve el `RouterOutlet` dentro de un elemento, usa el disparador `@routeAnimation` y enlazalo al elemento.

Para la transición de `@routeAnimation` a un estado desactivado, proporciona `data` al `ActivatedRoute`.
El `RouterOutlet` es expuesto como una variable de la plantilla `outlet`, por lo que enlaza la referencia a la salida del enrutador.
Este ejemplo usa una variable de `routerOutlet`.

<code-example path="router/src/app/app.component.2.html" header="src/app/app.component.html (router outlet)"></code-example>

La propiedad `@routeAnimation` esta vinculada a `getAnimationData()` con la referencia de `routerOutlet` proporciona, el siguiente paso es definir la funcion en el `AppComponent`.
La funcion `getAnimationData()` devuelve una propiedad animación desde el `data` proporcionado por la `ActivatedRoute`. La propiedad `animation` coincide con el nombre de `transition` que usaste en `slideInAnimation` definido en `animations.ts`.

<code-example path="router/src/app/app.component.2.ts" header="src/app/app.component.ts (router outlet)" region="function-binding"></code-example>

Cuando se cambia entre las dos rutas, el `HeroDetailComponent` y `HeroListComponent` entran desde la izquierda cuando se enrutan y se deslizan hacia la derecha cuando navegas fuera de la ruta.

{@a milestone-3-wrap-up}

### Resumen del hito 3

En esta sección hemos cubierto lo siguiente:

- Organizamos la aplicación en áreas de funcionalidad.
- Navegacion imperativa de un componente a otro.
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

Aquí estan los archivos relevantes para esta versión de la aplicación de muestra.

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

Para agregas más funcionalidades al centro de crisis actual de la aplicación, sigue unos pasos similares a los de la función de héroes:

- Crea una subcarpeta `crisis-center` en la carpeta `src/app`.
- Copia los archivos y carpetas de `app/heroes` en la nueva carpeta `crisis-center`.
- En los archivos nuevos, cambia cada mencion de "hero" por "crisis", y "heroes" por "crises".
- Renombra los archivos de NgModule de `crisis-center.module.ts` y `crisis-center-routing.module.ts`.

Utiliza el mock de crisis en lugar del mock de héroes.

<code-example path="router/src/app/crisis-center/mock-crises.ts" header="src/app/crisis-center/mock-crises.ts"></code-example>

El centro de crisis resultante es una base para la introducción de un nuevo concepto&mdash;enrutamiento hijo. Puedes dejar Héroes en su estado actual como contraste con el Centro de crisis.

<div class="alert is-helpful">

De acuerod con <a href="https://blog.8thlight.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html" title="Separación de Responsabilidad">Principio de separación de responsabilidades</a>, los cambios al Centro de Crisis no afectan el `AppModule` o las funcionalidades de otros componentes.

</div>

{@a crisis-child-routes}
{@a a-crisis-center-with-child-routes}

### Un centro de crisis con rutas hijas

En esta senccion te mostrara como organizar el centro de crisis de acuerdo a los siguientes patrones recomendados para aplicaciónes en Angular:

- Cada área de funcionalidad reside en su propia carpeta.
- Cada funcionalidad tiene su propio módulo de función de Angular.
- Cada área tiene su propio componente en raíz.
- Cada componente en raíz tiene su propia salida de enrutamiento y rutas hijas.
- Las áreas de funcionalidad de rutas rara vez (si alguna vez) se cruzan con otras rutas de otras funcionalidades.

Si tu aplicación tiene muchas áreas de funcionalidad, el arbol de componentes de la aplicación podria verse asi:

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

El `CrisisCenterComponent` tiene lo siguiente en comun con el `AppComponent`:

- Es la raíz del área del centro de crisis, solo que el `AppComponent` esta en la raíz de toda la aplicación.
- Es una carcasa para el área de funcionalidad de la gestion de crisis, solamente que el `AppComponent` es una carcasa para administrar el flujo de trabajo de alto nivel.

Como la mayoria de las carcasas, la clase `CrisisCenterComponent` es minima por que no tiene logica de negocios, y su plantilla no tiene vínculos, solamente `<router-outlet>` para el componente hijo del centro de crisis.

{@a child-route-config}

### Configuración de rutas hijas

Como página anfitriona para la funcionalidad de "Crisis Center", genera un componente `CrisisCenterHome` en la carpeta `crisis-center`.

<code-example language="none" class="code-shell">
  ng generate component crisis-center/crisis-center-home
</code-example>

Actualiza la plantilla con el mensaje de bienvenida al `Crisis Center`.

<code-example path="router/src/app/crisis-center/crisis-center-home/crisis-center-home.component.html" header="src/app/crisis-center/crisis-center-home/crisis-center-home.component.html"></code-example>

Actualiza el archivo `crisis-center-routing.module.ts` que renombraste despues de copiarlo del archivo `heroes-routing.module.ts`.
Esta vez, definiras las rutas hijas dentro de la ruta padre `crisis-center`.

<code-example path="router/src/app/crisis-center/crisis-center-routing.module.1.ts" header="src/app/crisis-center/crisis-center-routing.module.ts (Routes)" region="routes"></code-example>

Observa que la ruta padre `crisis-center` tiene una propiedad `children` con una única ruta que contiene el `CrisisListComponent`. La ruta `CrisisListComponent` tambien tiene un array `children` con dos rutas.

Estas dos rutas navegan hacia los componentes hijos del centro de crisis, `CrisisCenterHomeComponent` y `CrisisDetailComponent`, respectivamente.

Existen diferencias importantes en la forma que el enrutador trata las rutas hijas.

El enrutador muestra los componentes de estas rutas en el `RouterOutlet` de `CrisisCenterComponent`, no en el `RouterOutlet` de la carcasa de `AppComponent`.

El `CrisisListComponent` contiene la lista de crisis y un `RouterOutlet` para mostrar los componentes de ruta de `Crisis Center Home` y `Crisis Detail`.

La ruta de `Crisis Detail` es un hijo de `Crisis List`.
El enrutador [reutiliza componentes](#reuse) de forma predeterminada, por lo que el componente `Crisis Detail` sera reutilizado cada vez que selecciones una crisis diferente.
Por el contrario, en la ruta de `Hero Detail` [el componente es creado de nuevo](#snapshot-the-no-observable-alternative) cada vez que seleccionas un héroe diferente de la lista de héroes.

En un nivel superior, las rutas comienza con `/` para referirse a la raíz de la aplicación.
Pero las rutas hijas extienden la ruta padre.
En cada paso del arbol de ruta, agrega una barra (`/`) seguida de la ruta, a menos que la ruta este vacia.

Aplica esta lógica a la navegación dentro del centro de crisis con la ruta padre `/crisis-center`.

- Para navegar al `CrisisCenterHomeComponent`, la URL completa es `/crisis-center` (`/crisis-center` + `''` + `''`).

- Para navegar al `CrisisDetailComponent` por una crisis con `id=2`, la URL completa es `/crisis-center/2` (`/crisis-center` + `''` + `'/2'`).

La URL absoluta para el último ejemplo, incluido el origen `localhost`, es la siguiente:

<code-example>
  localhost:4200/crisis-center/2

</code-example>

Aquí esta el archivo completo `crisis-center-routing.module.ts` con sus respectivos `imports`.

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

Elimina la ruta de centro de crisis inivial del archivo `app-routing.module.ts` por que ahora los módulos de `HeroesModule` y `CrisisCenter` proporcionan la funcionalidad de las rutas.

El archivo `app-routing.module.ts` conserva las rutas de nivel superior de la aplicación, como las rutas predeterminadas y las rutas comodín.

<code-example path="router/src/app/app-routing.module.3.ts" header="src/app/app-routing.module.ts (v3)" region="v3"></code-example>

{@a relative-navigation}

### Navegación relativa

Mientras contruias la funcionalidad del centro de crisis, navegaste a la ruta del detalle de crisis usando una ruta absoluta que comienza con una barra.

El enrutador hace coincidir las rutas absolutas con las rutas comenzado desde la parte superior de la configuración de la ruta.

Puedes seguir usando rutas absolutas como estas para navegar dentro de la funcionalidad del Centro de Crisis, pero eso fija los enlaces a la estructura del enrutamiento padre.
Si cambias la ruta padre `/crisis-center`, tendrias que cambiar el array con los parámetros del enlace.

Puedes liberar los enlaces de esta dependencia definiendo las rutas que son relativas al segmento de URL actual.
La navegación dentro del área de funcionalidad permanece intacta incluso si cambias la ruta padre a esta funcionalidad.

<div class="alert is-helpful">

El enrutador admite una sintaxis similar a un directorio en una _lista de parámetros de enlace_ para ayudarte a guíar la busqueda de nombres de rutas:

`./` o `sin barra inclinada` es relativa al nivel actual.

`../` para subir un nivel en la ruta.

Puedes combinar la sintaxis de navegación relativa con una ruta anterior.
Si debes navegar a una ruta hermana, puedes usar la convención `../<sibling>` para subir un nivel por encima de la ruta hermana.

</div>

Para navegar por una ruta relativa con el método `Router.navigate`, debes proporcionar el `ActivatedRoute` para informar al enrutador donde te encuentras en el árbol de rutas actual.

Después del _array de parámetros de enlaces_, agrega un objecto con la propiedad `relativeTo` establecida en la `ActivatedRoute`.
Luego el enrutador calculara la URL de destino en función de la ubicación de la ruta activa.

<div class="alert is-helpful">

Siempre especifica la ruta absoluta completa cuando llames al método `navigateByUrl()` del enrutador.

</div>

{@a nav-to-crisis}

### Navega a la lista de crisis usando una URL relativa

Ya haz inyectado el `ActivatedRoute` que se necesita para componer la ruta de navegación relativa.

Al usar el `RouterLink` para navegar en lugar del servicio del `Router`, utilizara el mismo array de parámetros de enlace, pero no proporcionara al objeto la propiedad `relativeTo`.
El `ActivatedRoute` está implícito en la directiva `RouterLink`.

Actualiza el método `gotoCrises()` de `CrisisDetailComponent` para navegar de vuelta a la lista del Centro de Crisis utilizando la navegación de ruta relativa.

<code-example path="router/src/app/crisis-center/crisis-detail/crisis-detail.component.ts" header="src/app/crisis-center/crisis-detail/crisis-detail.component.ts (relative navigation)" region="gotoCrises-navigate"></code-example>

Observa que la ruta sube un nivel usando la sintaxis `../`.
Si la crisis actual tiene un `id` de `3`, la ruta resultante de regreso a la lista de crisis es `/crisis-center/;id=3;foo=foo`.

{@a named-outlets}

### Visualizando multiples rutas en outlets nombrados

Decides darle a los usuarios una manera de contactar con el centro de crisis.
Cuando un usuario hace clic en el botón de "Contact", quieres mostrar un mensaje en una vista emergente.

La ventana emergente debe de permanecer abierta, incluso al cambiar de página en la aplicación, hasta que el usuario la cierre enviando el mensaje o cancelando.
laramente no puedes colocar la ventana emergente en el mismo outlet que las otras paginas.

Hasta ahora, has definido un único outlet y has anidado rutas hijas debajo de ese outlet para agrupar las rutas.
El enrutador solo admite una salida principal de un outlet sin nombre por plantilla.

Una plantilla puede tener cualquier cantidad de outlets con nombre.
Cada outlet nombrado tiene su propio conjunto de rutas con sus propios componentes.
Multiples outlets pueden mostrar diferente contenido, determinado por diferentes rutas, todas al mismo tiempo.

Agrega un outlet llamado "popup" en el `AppComponent`, directamente debajo del outlet sin nombre.

<code-example path="router/src/app/app.component.4.html" header="src/app/app.component.html (outlets)" region="outlets"></code-example>

Ahí es donde ira la ventana emergente, una vez que aprendas a enrutar un componente emergente a ella.

{@a secondary-routes}

#### Rutas secundarias

Los outlets nombrados son objetivo de las _rutas secundarias_.

Las rutas secundarias se ven como rutas principales y se configuran de la misma manera.
Se diferencian en algunos aspectos clave:

- Son independientes entre si.
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

Aquí esta el componente, la plantilla y sus estilos:

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

Ademas de las propiedades `path` y `component`, hay una nueva propiedad llamada `outlet`, que se establece en `'popup'`.
Esta ruta ahora apunta al outlet popup y el `ComposeMessageComponent` se mostrará allí.

Para darle a los usuarios una forma de abrir la ventana emergente, agrega un enlace de "Contact" a la plantilla `AppComponent`.

<code-example path="router/src/app/app.component.4.html" header="src/app/app.component.html (contact-link)" region="contact-link"></code-example>

Aunque la ruta `compose` esta configurada para el outlet "popup", eso no es suficiente para conectar la ruta con la directiva `RouterLink`.
Tienes que especifica el outlet con nombre en un _array de parámetros de enlace_ y enlaza la al `RouterLink` con la propiedad binding.

El _array de parámetros de enlace_ contiene un objeto con una única propiedad `outlets`, cuyo valor es otro objeto con una (ó mas) nombres de outlets.
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
Deberias de ver algo como la siguiente URL en la barra de direcciones del navegador:

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

El enrutador realiza un seguimiento de dos ramas separadas en un mismo arbol de navegación y genera una representación de ese árbol en la URL.

Puedes agregas muchos mas outlets y rutas, en el nivel superior y en niveles anidades, creando una navehación de árbol con muchas ramas y el enrutador va a generar las URLs correpondientes.

Puedes decirle al enrutador que navegue hacia una por un árbol completo a la vez completando el objeto `outlets` y luego pasar ese objeto dentro de un _array de parámetros de enlace_ al método `router.navigate`.

{@a clear-secondary-routes}

#### Limpiando las rutas secundarias

Al igual que los outlets regulares, los outlets secundarios persisten hasta que navegas hacia un nuevo componente.

Cada outlet secundario tiene su propia navegación, independiente de la navegación del outlet principal. Cambiar la ruta actual que se muestra en el outlet primario no tiene efecto en el outlet de la ventana emergente. Eso es por que la ventana emergente se mantiene visible mientras navegas entre las crisis y los héroes.

De nuevo el método `closePopup()`:

<code-example path="router/src/app/compose-message/compose-message.component.ts" header="src/app/compose-message/compose-message.component.ts (closePopup)" region="closePopup"></code-example>

Al hacer clic en los botón de de "send" o "cancel", se borra la vista de la ventana emergente.
La función `closePopup()` navega imperativamente con el método `Router.navigate()`, pasando un [array de parámetros de enlace](#link-parameters-array).

Al igual que el array vinculado al _Contacto_ del `RouterLink` en el `AppComponent`, este incluye un objeto con una propiedad `outlets`. El valor de la propiedad `outlets` es otro objeto con los nombres de los outlets. El unico outlet nombrado es `'popup'`.

Esta vez, el valor de `'popup'` es `null`.
Esa no es una ruta, pero es un valor legitimo. Establecer la ventana emergente del `RouterOutlet` a `null` borra el outlet y elimina la ruta secundaria de la ventana emergente de la URL actual.

{@a guards}

{@a milestone-5-route-guards}

## Hito 5: Guards de ruta

Por el momento, cualquier usuario puede navegar a cualquier lugar de la aplicación en cualquier momento, pero en a veces es necesario controlar el acceso a diferentes partes de la aplicación por varias razones. Alguna de las cuales pueden ser:

- Quizas el usuario no este autorizado a navegar hacia un componente en especifico.
- Quizas el usuario deberias de iniciar sesion (autenticarse) antes.
- Quizas deberia de buscar algunos datos antes de mostrar un componente en especifico.
- Es posible que desee guardar los cambios antes de salir del componente actual.
- Es posible que debas preguntarle al usuario si esta bien descartar los cambios pendientes en lugar de ser guardados.

Puedes agregar guards a la configuración de la ruta para manejar estos escenarios.

El valor de retorno de un guard controla el comportamiento del enrutador:

- Si retorna `true`, el proceso de navegación continúa.
- Si retorna `false`, el proceso de navegación se detiene y el usuario permanece quieto.
- Si retorna un `UrlTree`, la navegación actual se cancela y se inicia una nueva navegación con el `UrlTree` devuelto.

<div class="alert is-helpful">

**Note:** The guard can also tell the router to navigate elsewhere, effectively canceling the current navigation.
When doing so inside a guard, the guard should return `false`;

</div>

El guard podria devolver una respuesta booleana sincrona. Pero en muchos casos, el guard no puede producir una respuesta sincrona. El guard podria hacerle una pregunta al usuario, guardar cambios en el servidor o buscar nuevos datos. Todas son operaciones asincronas.

En consecuencia, un guard de enrutamimento puede devovler un `Observable<boolean>` o un `Promise<boolean>` y el enrutador esperará a que el observable se resuelva en `true` o `false`.

<div class="alert is-critical">

**Nota:** El observable proporcionado al `Router` debe de completarse. Si el observable no se completa, la navegación no continua.

</div>

The router supports multiple guard interfaces:

- [`CanActivate`](api/router/CanActivate) para mediar la navegación _hacia_ una ruta.

- [`CanActivateChild`](api/router/CanActivateChild) para mediar la navegación _hacia_ una ruta hija.

- [`CanDeactivate`](api/router/CanDeactivate) para mediar la navegación _fuera_ de la ruta actual.

- [`Resolve`](api/router/Resolve) para realizar la recuperación de datos _antes_ de la activación de la ruta.

- [`CanLoad`](api/router/CanLoad) para mediar la navegación _hacia_ un módulo de funciones cargado _asincronamente_.

Puedes tener varios gards en cada nivel de la jerarquia de enrutamiento. El enrutador comprueba primero los guards `CanDeactivate` y `CanActivateChild`, desde la ruta hija mas profunda hasta la parte superior. Luego verifica los guards `CanActivate` desde la ruta superior hasta la ruta hija mas profunda. Si el módulo de funciones se carga de manera asincrona, el guard `CanLoad` se verifica antes que el módulo sea cargado. Si _cualquier_ guard devuelve `false`, los guards pendientes que no se hayan completado se cancelarán y se cancelará toda la navegación.

Hay varios ejemplos en las siguientes secciones:

{@a can-activate-guard}

### `CanActivate`: requiere autenticación

Las aplicaciones a menudo restringen el acceso a una área de funciones basadas en la identidad del usuario. Puedes permitir el acceso solo a los usuarios autenticados o a los usuarios con un role en especifico. Puedes bloquear o limitar el acceso hasta que la cuenta del usuaria sea activada.

El guard `CanActivate` es la herramienta para administrar estas reglas de negocio en la navegación.

#### Agregar un modulo de funciones de administrador

Esta seccion te guíara para extender el centro de crisis con algunas nuevas funcionalidades administrativas.
Empieza agregando un nuevo modulo de funcion llamado `AdminModule`.

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

La estructura de archivos de la funcionalida del administrados se ve así:

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

El modulo de funciones del administración contiene el `AdminComponent` utilizado para el enrutamiento dentro del módulo de funciones, una ruta al tablero y dos componentes sin terminar para gestionar las crisis y héroes.

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

Aunque el panel de administración `RouterLink` solo contiene un barra inclinada relativa, sin un segmento de URL adicional, coincide con cualquier ruta dentro del área de funciones administrativas. Solo deseas que el enlace al `Dashboard` este activado cuando el usuario visite esa ruta. Añadiendo un enlace adicional al `Dashboard` routerLink, [routerLinkActiveOptions]="{ exact: true }"`, marca el enlace `./`como activo cuando el usuario navega a la URL de`/admin` y no cuando navegas a cualquiera de las rutas hijas.

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

A continuación, importa el `AdminModule` en `app.module.ts` y agregalo al array de `imports` para registrar las rutas de administración.

<code-example path="router/src/app/app.module.4.ts" header="src/app/app.module.ts (admin module)" region="admin-module"></code-example>

Agrega un enlace "Admin" a la carcasa de `AppComponent` para que los usuarios, puedan acceder a esta función.

<code-example path="router/src/app/app.component.5.html" header="src/app/app.component.html (template)"></code-example>

{@a guard-admin-feature}

#### Protege la función de administrador

Actualmento, todas las rutas del Centro de Crisis estan abiertas para todos. La nueva funcionalidad de administración deberia de ser accesible solo para los usuario autenticados.

Escribe un método guard `canActivate()` para redirigir a los usuarios anonimos a la página de inicio de sesión cuanto intenten ingresar al área de administración.

Genera un `AuthGuard` en la carpeta de `auth`.

<code-example language="none" class="code-shell">
  ng generate guard auth/auth
</code-example>

Para demostrar los fundamentos, este ejemplo solo registra en consola, `returns` verdadero inmediatamente, y permite que la navegación continue:

<code-example path="router/src/app/auth/auth.guard.1.ts" header="src/app/auth/auth.guard.ts (excerpt)"></code-example>

A continuación, abre el archivo `admin-routing.module.ts `. importa la clase `AuthGuard` y actualiza la ruta de administración con una propiedad guard `canActivate` que haga referencia a ella:

<code-example path="router/src/app/admin/admin-routing.module.2.ts" header="src/app/admin/admin-routing.module.ts (guarded admin route)" region="admin-route"></code-example>

La función de administración ahora esta protegida por el guard, pero el guard requiere mas personalización para funcionar completamente.

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

Aunque en realidad no inicia sesión, tiene una bandera `isLoggedIn` para indicar que el usuario esta autenticado.
Su método `login()` simula una llamada a un servicio de API externo al devolver un obervable que resuelve con écito después de una breve pausa.
La propiedad `redirectUrl` almacena la URL a la cual el usuario queria acceder para que pueda navegar a ella después de la autenticación.

<div class="alert is-helpful">

Para mantener la cosas al mínimo, este ejemplo redirige a los usuarios no autenticados a `/admin`.

</div>

Revisa el `AuthGuard` para llamar al `AuthService`.

<code-example path="router/src/app/auth/auth.guard.2.ts" header="src/app/auth/auth.guard.ts (v2)"></code-example>

Observa que se inyectaron el `AuthService` y el `Router` en el contructor.
Aun no has proporcionado el `AuthService` pero es bueno saber que puedes inyectar servicios utiles en los guards del enrutador.

Este guard retorna un resultado booleano sincróno.
Si el usuario ha iniciado sesión, regresara verdadero y la navegación continuara.

El `ActivatedRouteSnapshot` contiene la ruta _futura_ que se activara y `RouterStateSnapshot` contiene el _futuro_ `RouterState` de la aplicación, en caso de que pase por el guard.

Si el usuario no ha iniciado sesión, alamacena el intento URL de donde proviene el usuario usando el `RouterStateSnapshot.url` y le dice al enrutador que lo redireccione a la página de inicio de sesión&mdash; página que aún no has creado.
Devuelve un `UrlTree` que llama al `Router` a cancelar la navegación actual y programa una nueva para redirigir al usuario.

{@a add-login-component}

#### Agrega el `LoginComponent`

Necesitas un `LoginComponent` para que el usuario inicie sesión en la aplicación. Después de iniciar sesión, sera redireccionado a la URL almacenada si esta disponible o usara la URL predeterminada.
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

Tambien puedes proteger las rutas hijas con el guard `CanActivateChild`.
El guard `CanActivateChild` es similar al guard `CanActivate`.
La diferencia clave es que corre antes de que se active cualquier ruta hija.

El módulo de función de administración esta protegido de accesos no autorizados.
Tambien deben de ser protegidas las hijas rutas _dentro_ del módulo de funciones.

Extiende el `AuthGuard` para proteger al navegar entre las rutas de `admin`.
Abre el archivo `auth.guard.ts` y agrega la interfaz `CanActivateChild` a los tokens importados del paquete del enrutador.

A continuación implementa el método `canActivateChild()` el cual toma los mismos argumentos que el método `canActivate()`: un `ActivatedRouteSnapshot` y el `RouterStateSnapshot`.
El método `canActivateChild()` puede devolver un `Observable<boolean|UrlTree>` o `Promise<boolean|UrlTree>` para comprobaciones asíncronas y un `boolean` or `UrlTree` para comprobaciones sincronas.
Este devuelve `true` para permitir al usuario acceder al módulo de funciones de administración o `UrlTree` que redirije al usuario a la pagina de inicio de sesión:

<code-example path="router/src/app/auth/auth.guard.3.ts" header="src/app/auth/auth.guard.ts (excerpt)" region="can-activate-child"></code-example>

Agrega el mismo `AuthGuard` a la ruta de administración `component-less` para proteger todas las otras rutas hijas al mismo tiempo, en lugar de agregar el `AuthGuard` a cada ruta individualmente.

<code-example path="router/src/app/admin/admin-routing.module.3.ts" header="src/app/admin/admin-routing.module.ts (excerpt)" region="can-activate-child"></code-example>

{@a can-deactivate-guard}

### `CanDeactivate`: handling unsaved changes

Back in the "Heroes" workflow, the app accepts every change to a hero immediately without validation.

In the real world, you might have to accumulate the users changes, validate across fields, validate on the server, or hold changes in a pending state until the user confirms them as a group or cancels and reverts all changes.

When the user navigates away, you can let the user decide what to do with unsaved changes.
If the user cancels, you'll stay put and allow more changes.
If the user approves, the app can save.

You still might delay navigation until the save succeeds.
If you let the user move to the next screen immediately and saving were to fail (perhaps the data is ruled invalid), you would lose the context of the error.

You need to stop the navigation while you wait, asynchronously, for the server to return with its answer.

The `CanDeactivate` guard helps you decide what to do with unsaved changes and how to proceed.

{@a cancel-save}

#### Cancel and save

Users update crisis information in the `CrisisDetailComponent`.
Unlike the `HeroDetailComponent`, the user changes do not update the crisis entity immediately.
Instead, the app updates the entity when the user presses the Save button and discards the changes when the user presses the Cancel button.

Both buttons navigate back to the crisis list after save or cancel.

<code-example path="router/src/app/crisis-center/crisis-detail/crisis-detail.component.ts" header="src/app/crisis-center/crisis-detail/crisis-detail.component.ts (cancel and save methods)" region="cancel-save"></code-example>

In this scenario, the user could clic the heroes link, cancel, push the browser back button, or navigate away without saving.

This example app asks the user to be explicit with a confirmation dialog box that waits asynchronously for the user's
response.

<div class="alert is-helpful">

You could wait for the user's answer with synchronous, blocking code, however, the app is more responsive&mdash;and can do other work&mdash;by waiting for the user's answer asynchronously.

</div>

Generate a `Dialog` service to handle user confirmation.

<code-example language="none" class="code-shell">
  ng generate service dialog
</code-example>

Add a `confirm()` method to the `DialogService` to prompt the user to confirm their intent.
The `window.confirm` is a blocking action that displays a modal dialog and waits for user interaction.

<code-example path="router/src/app/dialog.service.ts" header="src/app/dialog.service.ts"></code-example>

It returns an `Observable` that resolves when the user eventually decides what to do: either to discard changes and navigate away (`true`) or to preserve the pending changes and stay in the crisis editor (`false`).

{@a CanDeactivate}

Generate a guard that checks for the presence of a `canDeactivate()` method in a component&mdash;any component.

<code-example language="none" class="code-shell">
  ng generate guard can-deactivate
</code-example>

Paste the following code into your guard.

<code-example path="router/src/app/can-deactivate.guard.ts" header="src/app/can-deactivate.guard.ts"></code-example>

While the guard doesn't have to know which component has a deactivate method, it can detect that the `CrisisDetailComponent` component has the `canDeactivate()` method and call it.
The guard not knowing the details of any component's deactivation method makes the guard reusable.

Alternatively, you could make a component-specific `CanDeactivate` guard for the `CrisisDetailComponent`.
The `canDeactivate()` method provides you with the current instance of the `component`, the current `ActivatedRoute`, and `RouterStateSnapshot` in case you needed to access some external information.
This would be useful if you only wanted to use this guard for this component and needed to get the component's properties or confirm whether the router should allow navigation away from it.

<code-example path="router/src/app/can-deactivate.guard.1.ts" header="src/app/can-deactivate.guard.ts (component-specific)"></code-example>

Looking back at the `CrisisDetailComponent`, it implements the confirmation workflow for unsaved changes.

<code-example path="router/src/app/crisis-center/crisis-detail/crisis-detail.component.ts" header="src/app/crisis-center/crisis-detail/crisis-detail.component.ts (excerpt)" region="canDeactivate"></code-example>

Notice that the `canDeactivate()` method can return synchronously; it returns `true` immediately if there is no crisis or there are no pending changes.
But it can also return a `Promise` or an `Observable` and the router will wait for that to resolve to truthy (navigate) or falsy (stay on the current route).

Add the `Guard` to the crisis detail route in `crisis-center-routing.module.ts` using the `canDeactivate` array property.

<code-example path="router/src/app/crisis-center/crisis-center-routing.module.3.ts" header="src/app/crisis-center/crisis-center-routing.module.ts (can deactivate guard)"></code-example>

Now you have given the user a safeguard against unsaved changes.

{@a Resolve}

{@a resolve-guard}

### _Resolve_: pre-fetching component data

In the `Hero Detail` and `Crisis Detail`, the app waited until the route was activated to fetch the respective hero or crisis.

If you were using a real world API, there might be some delay before the data to display is returned from the server.
You don't want to display a blank component while waiting for the data.

To improve this behavior, you can pre-fetch data from the server using a resolver so it's ready the
moment the route is activated.
This also allows you to handle errors before routing to the component.
There's no point in navigating to a crisis detail for an `id` that doesn't have a record.
It'd be better to send the user back to the `Crisis List` that shows only valid crisis centers.

In summary, you want to delay rendering the routed component until all necessary data has been fetched.

{@a fetch-before-navigating}

#### Fetch data before navigating

At the moment, the `CrisisDetailComponent` retrieves the selected crisis.
If the crisis is not found, the router navigates back to the crisis list view.

The experience might be better if all of this were handled first, before the route is activated.
A `CrisisDetailResolver` service could retrieve a `Crisis` or navigate away, if the `Crisis` did not exist, _before_ activating the route and creating the `CrisisDetailComponent`.

Generate a `CrisisDetailResolver` service file within the `Crisis Center` feature área.

<code-example language="none" class="code-shell">
  ng generate service crisis-center/crisis-detail-resolver
</code-example>

<code-example path="router/src/app/crisis-center/crisis-detail-resolver.service.1.ts" header="src/app/crisis-center/crisis-detail-resolver.service.ts (generated)"></code-example>

Move the relevant parts of the crisis retrieval logic in `CrisisDetailComponent.ngOnInit()` into the `CrisisDetailResolverService`.
Import the `Crisis` model, `CrisisService`, and the `Router` so you can navigate elsewhere if you can't fetch the crisis.

Be explicit and implement the `Resolve` interface with a type of `Crisis`.

Inject the `CrisisService` and `Router` and implement the `resolve()` method.
That method could return a `Promise`, an `Observable`, or a synchronous return value.

The `CrisisService.getCrisis()` method returns an observable in order to prevent the route from loading until the data is fetched.
The `Router` guards require an observable to `complete`, which means it has emitted all
of its values.
You use the `take` operator with an argument of `1` to ensure that the `Observable` completes after retrieving the first value from the Observable returned by the `getCrisis()` method.

If it doesn't return a valid `Crisis`, then return an empty `Observable`, cancel the previous in-progress navigation to the `CrisisDetailComponent`, and navigate the user back to the `CrisisListComponent`.
The updated resolver service looks like this:

<code-example path="router/src/app/crisis-center/crisis-detail-resolver.service.ts" header="src/app/crisis-center/crisis-detail-resolver.service.ts"></code-example>

Import this resolver in the `crisis-center-routing.module.ts` and add a `resolve` object to the `CrisisDetailComponent` route configuration.

<code-example path="router/src/app/crisis-center/crisis-center-routing.module.4.ts" header="src/app/crisis-center/crisis-center-routing.module.ts (resolver)"></code-example>

The `CrisisDetailComponent` should no longer fetch the crisis.
When you re-configured the route, you changed where the crisis is.
Update the `CrisisDetailComponent` to get the crisis from the `ActivatedRoute.data.crisis` property instead;

<code-example path="router/src/app/crisis-center/crisis-detail/crisis-detail.component.ts" header="src/app/crisis-center/crisis-detail/crisis-detail.component.ts (ngOnInit v2)" region="ngOnInit"></code-example>

Note the following three important points:

1. The router's `Resolve` interface is optional.
   The `CrisisDetailResolverService` doesn't inherit from a base class.
   The router looks for that method and calls it if found.

1. The router calls the resolver in any case where the the user could navigate away so you don't have to code for each use case.

1. Returning an empty `Observable` in at least one resolver will cancel navigation.

The relevant Crisis Center code for this milestone follows.

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

### Query parameters and fragments

In the [route parameters](#optional-route-parameters) section, you only dealt with parameters specific to the route.
However, you can use query parameters to get optional parameters available to all routes.

[Fragments](https://en.wikipedia.org/wiki/Fragment_identifier) refer to certain elements on the page
identified with an `id` attribute.

Update the `AuthGuard` to provide a `session_id` query that will remain after navigating to another route.

Add an `anchor` element so you can jump to a certain point on the page.

Add the `NavigationExtras` object to the `router.navigate()` method that navigates you to the `/login` route.

<code-example path="router/src/app/auth/auth.guard.4.ts" header="src/app/auth/auth.guard.ts (v3)"></code-example>

You can also preserve query parameters and fragments across navigations without having to provide them again when navigating.
In the `LoginComponent`, you'll add an _object_ as the second argument in the `router.navigateUrl()` function and provide the `queryParamsHandling` and `preserveFragment` to pass along the current query parameters and fragment to the next route.

<code-example path="router/src/app/auth/login/login.component.ts" header="src/app/auth/login/login.component.ts (preserve)" region="preserve"></code-example>

<div class="alert is-helpful">

The `queryParamsHandling` feature also provides a `merge` option, which preserves and combines the current query parameters with any provided query parameters when navigating.

</div>

To navigate to the Admin Dashboard route after logging in, update `admin-dashboard.component.ts` to handle the
query parameters and fragment.

<code-example path="router/src/app/admin/admin-dashboard/admin-dashboard.component.1.ts" header="src/app/admin/admin-dashboard/admin-dashboard.component.ts (v2)"></code-example>

Query parameters and fragments are also available through the `ActivatedRoute` service.
Just like route parameters, the query parameters and fragments are provided as an `Observable`.
The updated Crisis Admin component feeds the `Observable` directly into the template using the `AsyncPipe`.

Now, you can clic on the Admin button, which takes you to the Login page with the provided `queryParamMap` and `fragment`.
After you clic the login button, notice that you have been redirected to the `Admin Dashboard` page with the query parameters and fragment still intact in the address bar.

You can use these persistent bits of information for things that need to be provided across pages like authentication tokens or session ids.

<div class="alert is-helpful">

The `query params` and `fragment` can also be preserved using a `RouterLink` with
the `queryParamsHandling` and `preserveFragment` bindings respectively.

</div>

{@a asynchronous-routing}

## Milestone 6: Asynchronous routing

As you've worked through the milestones, the application has naturally gotten larger.
At some point you'll reach a point where the application takes a long time to load.

To remedy this issue, use asynchronous routing, which loads feature modules lazily, on request.
Lazy loading has multiple benefits.

- You can load feature áreas only when requested by the user.
- You can speed up load time for users that only visit certain áreas of the application.
- You can continue expanding lazy loaded feature áreas without increasing the size of the initial load bundle.

You're already part of the way there.
By organizing the application into modules&mdash;`AppModule`,
`HeroesModule`, `AdminModule` and `CrisisCenterModule`&mdash;you
have natural candidates for lazy loading.

Some modules, like `AppModule`, must be loaded from the start.
But others can and should be lazy loaded.
The `AdminModule`, for example, is needed by a few authorized users, so
you should only load it when requested by the right people.

{@a lazy-loading-route-config}
{@a lazy-loading-route-configuration}

### Lazy Loading route configuration

Change the `admin` path in the `admin-routing.module.ts` from `'admin'` to an empty string, `''`, the empty path.

Use empty path routes to group routes together without adding any additional path segments to the URL.
Users will still visit `/admin` and the `AdminComponent` still serves as the Routing Component containing child routes.

Open the `AppRoutingModule` and add a new `admin` route to its `appRoutes` array.

Give it a `loadChildren` property instead of a `children` property.
The `loadChildren` property takes a function that returns a promise using the browser's built-in syntax for lazy loading code using dynamic imports `import('...')`.
The path is the location of the `AdminModule` (relative to the app root).
After the code is requested and loaded, the `Promise` resolves an object that contains the `NgModule`, in this case the `AdminModule`.

<code-example path="router/src/app/app-routing.module.5.ts" region="admin-1" header="app-routing.module.ts (load children)"></code-example>

<div class="alert is-important">

_Note_: When using absolute paths, the `NgModule` file location must begin with `src/app` in order to resolve correctly. For custom [path mapping with absolute paths](https://www.typescriptlang.org/docs/handbook/module-resolution.html#path-mapping), you must configure the `baseUrl` and `paths` properties in the project `tsconfig.json`.

</div>

When the router navigates to this route, it uses the `loadChildren` string to dynamically load the `AdminModule`.
Then it adds the `AdminModule` routes to its current route configuration.
Finally, it loads the requested route to the destination admin component.

The lazy loading and re-configuration happen just once, when the route is first requested; the module and routes are available immediately for subsequent requests.

<div class="alert is-helpful">

Angular provides a built-in module loader that supports SystemJS to load modules asynchronously. If you were
using another bundling tool, such as Webpack, you would use the Webpack mechanism for asynchronously loading modules.

</div>

Take the final step and detach the admin feature set from the main application.
The root `AppModule` must neither load nor reference the `AdminModule` or its files.

In `app.module.ts`, remove the `AdminModule` import statement from the top of the file
and remove the `AdminModule` from the NgModule's `imports` array.

{@a can-load-guard}
{@a can-load-guard-unauthorized-loading-of-feature-modules}

### `CanLoad`: guarding unauthorized loading of feature modules

You're already protecting the `AdminModule` with a `CanActivate` guard that prevents unauthorized users from accessing the admin feature área.
It redirects to the login page if the user is not authorized.

But the router is still loading the `AdminModule` even if the user can't visit any of its components.
Ideally, you'd only load the `AdminModule` if the user is logged in.

Add a `CanLoad` guard that only loads the `AdminModule` once the user is logged in _and_ attempts to access the admin feature área.

The existing `AuthGuard` already has the essential logic in its `checkLogin()` method to support the `CanLoad` guard.

Open `auth.guard.ts`.
Import the `CanLoad` interface from `@angular/router`.
Add it to the `AuthGuard` class's `implements` list.
Then implement `canLoad()` as follows:

<code-example path="router/src/app/auth/auth.guard.ts" header="src/app/auth/auth.guard.ts (CanLoad guard)" region="canLoad"></code-example>

The router sets the `canLoad()` method's `route` parameter to the intended destination URL.
The `checkLogin()` method redirects to that URL once the user has logged in.

Now import the `AuthGuard` into the `AppRoutingModule` and add the `AuthGuard` to the `canLoad`
array property for the `admin` route.
The completed admin route looks like this:

<code-example path="router/src/app/app-routing.module.5.ts" region="admin" header="app-routing.module.ts (lazy admin route)"></code-example>

{@a preloading}
{@a preloading-background-loading-of-feature-areas}

### Preloading: background loading of feature áreas

In addition to loading modules on-demand, you can load modules asynchronously with preloading.

The `AppModule` is eagerly loaded when the application starts, meaning that it loads right away.
Now the `AdminModule` loads only when the user clics on a link, which is called lazy loading.

Preloading allows you to load modules in the background so that the data is ready to render when the user activates a particular route.
Consider the Crisis Center.
It isn't the first view that a user sees.
By default, the Heroes are the first view.
For the smallest initial payload and fastest launch time, you should eagerly load the `AppModule` and the `HeroesModule`.

You could lazy load the Crisis Center.
But you're almost certain that the user will visit the Crisis Center within minutes of launching the app.
Ideally, the app would launch with just the `AppModule` and the `HeroesModule` loaded and then, almost immediately, load the `CrisisCenterModule` in the background.
By the time the user navigates to the Crisis Center, its module will have been loaded and ready.

{@a how-preloading}

#### How preloading works

After each successful navigation, the router looks in its configuration for an unloaded module that it can preload.
Whether it preloads a module, and which modules it preloads, depends upon the preload strategy.

The `Router` offers two preloading strategies:

- No preloading, which is the default. Lazy loaded feature áreas are still loaded on-demand.
- Preloading of all lazy loaded feature áreas.

The router either never preloads, or preloads every lazy loaded module.
The `Router` also supports [custom preloading strategies](#custom-preloading) for fine control over which modules to preload and when.

This section guides you through updating the `CrisisCenterModule` to load lazily by default and use the `PreloadAllModules` strategy to load all lazy loaded modules.

{@a lazy-load-crisis-center}

#### Lazy load the crisis center

Update the route configuration to lazy load the `CrisisCenterModule`.
Take the same steps you used to configure `AdminModule` for lazy loading.

1. Change the `crisis-center` path in the `CrisisCenterRoutingModule` to an empty string.

1. Add a `crisis-center` route to the `AppRoutingModule`.

1. Set the `loadChildren` string to load the `CrisisCenterModule`.

1. Remove all mention of the `CrisisCenterModule` from `app.module.ts`.

Here are the updated modules _before enabling preload_:

<code-tabs>

  <code-pane header="app.module.ts" path="router/src/app/app.module.ts" region="preload">

  </code-pane>

  <code-pane header="app-routing.module.ts" path="router/src/app/app-routing.module.6.ts" region="preload-v1">

  </code-pane>

  <code-pane header="crisis-center-routing.module.ts" path="router/src/app/crisis-center/crisis-center-routing.module.ts">

  </code-pane>

</code-tabs>

You could try this now and confirm that the `CrisisCenterModule` loads after you clic the "Crisis Center" button.

To enable preloading of all lazy loaded modules, import the `PreloadAllModules` token from the Angular router package.

The second argument in the `RouterModule.forRoot()` method takes an object for additional configuration options.
The `preloadingStrategy` is one of those options.
Add the `PreloadAllModules` token to the `forRoot()` call:

<code-example path="router/src/app/app-routing.module.6.ts" header="src/app/app-routing.module.ts (preload all)" region="forRoot"></code-example>

This configures the `Router` preloader to immediately load all lazy loaded routes (routes with a `loadChildren` property).

When you visit `http://localhost:4200`, the `/heroes` route loads immediately upon launch and the router starts loading the `CrisisCenterModule` right after the `HeroesModule` loads.

Currently, the `AdminModule` does not preload because `CanLoad` is blocking it.

{@a preload-canload}

#### `CanLoad` blocks preload

The `PreloadAllModules` strategy does not load feature áreas protected by a [CanLoad](#can-load-guard) guard.

You added a `CanLoad` guard to the route in the `AdminModule` a few steps back to block loading of that module until the user is authorized.
That `CanLoad` guard takes precedence over the preload strategy.

If you want to preload a module as well as guard against unauthorized access, remove the `canLoad()` guard method and rely on the [canActivate()](#can-activate-guard) guard alone.

{@a custom-preloading}

### Custom Preloading Strategy

Preloading every lazy loaded module works well in many situations.
However, in consideration of things such as low bandwidth and user metrics, you can use a custom preloading strategy for specific feature modules.

This section guides you through adding a custom strategy that only preloads routes whose `data.preload` flag is set to `true`.
Recall that you can add anything to the `data` property of a route.

Set the `data.preload` flag in the `crisis-center` route in the `AppRoutingModule`.

<code-example path="router/src/app/app-routing.module.ts" header="src/app/app-routing.module.ts (route data preload)" region="preload-v2"></code-example>

Generate a new `SelectivePreloadingStrategy` service.

<code-example language="none" class="code-shell">
  ng generate service selective-preloading-strategy
</code-example>

Replace the contents of `selective-preloading-strategy.service.ts` with the following:

<code-example path="router/src/app/selective-preloading-strategy.service.ts" header="src/app/selective-preloading-strategy.service.ts"></code-example>

`SelectivePreloadingStrategyService` implements the `PreloadingStrategy`, which has one method, `preload()`.

The router calls the `preload()` method with two arguments:

1. The route to consider.
1. A loader function that can load the routed module asynchronously.

An implementation of `preload` must return an `Observable`.
If the route does preload, it returns the observable returned by calling the loader function.
If the route does not preload, it returns an `Observable` of `null`.

In this sample, the `preload()` method loads the route if the route's `data.preload` flag is truthy.

As a side-effect, `SelectivePreloadingStrategyService` logs the `path` of a selected route in its public `preloadedModules` array.

Shortly, you'll extend the `AdminDashboardComponent` to inject this service and display its `preloadedModules` array.

But first, make a few changes to the `AppRoutingModule`.

1. Import `SelectivePreloadingStrategyService` into `AppRoutingModule`.
1. Replace the `PreloadAllModules` strategy in the call to `forRoot()` with this `SelectivePreloadingStrategyService`.
1. Add the `SelectivePreloadingStrategyService` strategy to the `AppRoutingModule` providers array so you can inject it elsewhere in the app.

Now edit the `AdminDashboardComponent` to display the log of preloaded routes.

1. Import the `SelectivePreloadingStrategyService`.
1. Inject it into the dashboard's constructor.
1. Update the template to display the strategy service's `preloadedModules` array.

Now the file is as follows:

<code-example path="router/src/app/admin/admin-dashboard/admin-dashboard.component.ts" header="src/app/admin/admin-dashboard/admin-dashboard.component.ts (preloaded modules)"></code-example>

Once the application loads the initial route, the `CrisisCenterModule` is preloaded.
Verify this by logging in to the `Admin` feature área and noting that the `crisis-center` is listed in the `Preloaded Modules`.
It also logs to the browser's console.

{@a redirect-advanced}

### Migrating URLs with redirects

You've setup the routes for navigating around your application and used navigation imperatively and declaratively.
But like any application, requirements change over time.
You've setup links and navigation to `/heroes` and `/hero/:id` from the `HeroListComponent` and `HeroDetailComponent` components.
If there were a requirement that links to `heroes` become `superheroes`, you would still want the previous URLs to navigate correctly.
You also don't want to update every link in your application, so redirects makes refactoring routes trivial.

{@a url-refactor}

#### Changing `/heroes` to `/superheroes`

This section guides you through migrating the `Hero` routes to new URLs.
The `Router` checks for redirects in your configuration before navigating, so each redirect is triggered when needed. To support this change, add redirects from the old routes to the new routes in the `heroes-routing.module`.

<code-example path="router/src/app/heroes/heroes-routing.module.ts" header="src/app/heroes/heroes-routing.module.ts (heroes redirects)"></code-example>

Notice two different types of redirects.
The first change is from `/heroes` to `/superheroes` without any parameters.
The second change is from `/hero/:id` to `/superhero/:id`, which includes the `:id` route parameter.
Router redirects also use powerful pattern-matching, so the `Router` inspects the URL and replaces route parameters in the `path` with their appropriate destination.
Previously, you navigated to a URL such as `/hero/15` with a route parameter `id` of `15`.

<div class="alert is-helpful">

The `Router` also supports [query parameters](#query-parameters) and the [fragment](#fragment) when using redirects.

- When using absolute redirects, the `Router` will use the query parameters and the fragment from the `redirectTo` in the route config.
- When using relative redirects, the `Router` use the query params and the fragment from the source URL.

</div>

Currently, the empty path route redirects to `/heroes`, which redirects to `/superheroes`.
This won't work because the `Router` handles redirects once at each level of routing configuration.
This prevents chaining of redirects, which can lead to endless redirect loops.

Instead, update the empty path route in `app-routing.module.ts` to redirect to `/superheroes`.

<code-example path="router/src/app/app-routing.module.ts" header="src/app/app-routing.module.ts (superheroes redirect)"></code-example>

A `routerLink` isn't tied to route configuration, so update the associated router links to remain active when the new route is active.
Update the `app.component.ts` template for the `/heroes` `routerLink`.

<code-example path="router/src/app/app.component.html" header="src/app/app.component.html (superheroes active routerLink)"></code-example>

Update the `goToHeroes()` method in the `hero-detail.component.ts` to navigate back to `/superheroes` with the optional route parameters.

<code-example path="router/src/app/heroes/hero-detail/hero-detail.component.ts" region="redirect" header="src/app/heroes/hero-detail/hero-detail.component.ts (goToHeroes)"></code-example>

With the redirects setup, all previous routes now point to their new destinations and both URLs still function as intended.

{@a inspect-config}

### Inspect the router's configuration

To determine if your routes are actually evaluated [in the proper order](#routing-module-order), you can inspect the router's configuration.

Do this by injecting the router and logging to the console its `config` property.
For example, update the `AppModule` as follows and look in the browser console window
to see the finished route configuration.

<code-example path="router/src/app/app.module.7.ts" header="src/app/app.module.ts (inspect the router config)" region="inspect-config"></code-example>

{@a final-app}

## Final app

For the completed router app, see the <live-example name="router"></live-example> for the final source code.

{@a link-parameters-array}
