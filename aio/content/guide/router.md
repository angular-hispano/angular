# Navegando dentro de la aplicación: enrutando las vistas

En una aplicación de una sola página, se cambia lo que el usuario ve al mostrar u ocultar partes de la pantalla que corresponden a componentes concretos, en lugar de ir al servidor para obtener una nueva página.
A medida que los usuarios realizan las tareas de la aplicación, necesitan moverse entre las diferentes [vistas](guide/glossary#view "Definition of view") que se han definido.

Para manejar la navegación de una [vista](guide/glossary#view) a otra, se utiliza el **`Router`** de Angular.
El **`Router`** permite la navegación interpretando la URL del navegador como una instrucción para cambiar la vista.

Para explorar una aplicación de ejemplo con las principales características del enrutador, consulte <live-example></live-example>.

## Prerrequisitos

Antes de crear una ruta, debería estar familiarizado con lo siguiente:

- [Fundamentos de los componentes](guide/architecture-components)
- [Fundamentos de las plantillas](guide/glossary#template)
- Una aplicación de Angular&mdash;puedes generar una aplicación básica usando el [CLI de Angular](cli).

Para una introducción a Angular con una aplicación ya preparada, consulte [Primeros Pasos](start).
Para una experiencia más profunda en la construcción de una aplicación en Angular, consulte el tutorial [Tour of Heroes](tutorial). Ambos te guían en el uso de clases de componentes y plantillas.

{@a basics}

## Generar una aplicación con el enrutamiento activado

El siguiente comando utiliza el CLI de Angular para generar una aplicación básica de Angular con un módulo de enrutamiento, llamado `AppRoutingModule`, que es un NgModule donde puedes configurar tus rutas.
El nombre de la app en el siguiente ejemplo es `routing-app`.

<code-example language="none" class="code-shell">
  ng new routing-app --routing
</code-example>

Al generar una nueva aplicación, la CLI le pide que seleccione CSS o un preprocesador CSS.
Para este ejemplo, se aceptó el valor predeterminado de `CSS`.

### Agregando componentes al enrutamiento

Para utilizar el enrutador de Angular, una aplicación necesita tener al menos dos componentes para poder navegar de uno a otro. Para crear un componente utilizando la CLI, introduzca lo siguiente en la línea de comandos donde `first` es el nombre del componente:

<code-example language="none" class="code-shell">
  ng generate component first
</code-example>

Repite este paso para un segundo componente pero dale un nombre diferente.
Aquí, el nuevo nombre es `second`.

<code-example language="none" class="code-shell">
  ng generate component second
</code-example>

La CLI añade automáticamente `Component`, por lo que si usted escribe `first-component`, el componente resultante sería `FirstComponentComponent`.

{@a basics-base-href}

<div class="alert is-helpful">

#### `<base href>`

Esta guía funciona con una aplicación Angular generada por el CLI. Si estás trabajando manualmente, asegúrate de tener `<base href="/">` en el `<head>` de tu archivo index.html. Esto asume que la carpeta de la `app` es la raíz de la aplicación, y utiliza `"/"`.

  </code-example>

</div>

### Importando los nuevos componentes

Para utilizar sus nuevos componentes, impórtelos en `AppRoutingModule` en la parte superior del archivo, de la siguiente manera:

<code-example header="AppRoutingModule (excerpt)">

import { FirstComponent } from './first/first.component';
import { SecondComponent } from './second/second.component';

</code-example>

{@a basic-route}

## Definir una ruta básica

Hay tres bloques fundamentales para crear una ruta.

Importar el `AppRoutingModule` en `AppModule` y añadirlo al array de `import`.

La CLI de Angular realiza este paso por ti.
Sin embargo, si estás creando una aplicación manualmente o trabajando con una aplicación existente sin CLI, verifica que las importaciones y la configuración sean correctas.
El siguiente es el `AppModule` por defecto usando el CLI con la bandera `--routing`.

  <code-example path="router/src/app/app.module.8.ts" header="Default CLI AppModule with routing">

  </code-example>

1. Importe `RouterModule` y `Routes` en su módulo de enrutamiento.

La CLI de Angular realiza este paso automáticamente.
La CLI también configura un array de `Routes` para tus rutas y configura los arrays de `imports` y `exports` para el `@NgModule()`.

  <code-example path="router/src/app/app-routing.module.7.ts" header="CLI app routing module">

  </code-example>

2. Defina sus rutas en el array `Routes`.

Cada ruta en este array es un objeto de JavaScript que contiene dos propiedades.
La primer propiedad, `path`, define la dirección URL para la ruta.
La segunda propiedad, `component`, define el componente que Angular debe utilizar para la ruta correspondiente.

  <code-example path="router/src/app/app-routing.module.8.ts" region="routes" header="AppRoutingModule (excerpt)">

  </code-example>

3. Añade tus rutas a tu aplicación.

Ahora que ha definido sus rutas, puede añadirlas a la aplicación.
En primer lugar, añada enlaces a los dos componentes.
Asigne el atributo `routerLink` al enlace donde desea añadir la ruta.
Asigne el valor del atributo al componente que se mostrará cuando un usuario haga clic en cada enlace.
A continuación, actualice la plantilla del componente para incluir `<router-outlet>`.
Este elemento informa a Angular para que actualice la vista de la aplicación con el componente para la ruta seleccionada.

<code-example path="router/src/app/app.component.7.html" header="Template with routerLink and router-outlet"></code-example>

{@a route-order}

### Orden de las rutas

El orden de las rutas es importante porque el `Router` utiliza la estrategia de primer coincidencia cuando enlaza las rutas, por lo que las rutas más específicas deben colocarse por encima de las menos específicas.
Agregue primero las rutas con una dirección estática, seguidas por una ruta vacía, que coincide con la ruta por defecto.
La [ruta comodín](guide/router#setting-up-wildcard-routes) viene en último lugar porque coincide con todas las URL y el enrutador la selecciona sólo si no hay otras rutas que coincidan primero.

{@a getting-route-information}

## Obteniendo información de la ruta

A menudo, cuando un usuario navega por la aplicación, usted quiere pasar información de un componente a otro.
Por ejemplo, considere una aplicación que muestra una lista de compras de artículos de alimentación.
Cada artículo de la lista tiene un `id` único.
Para editar un artículo, los usuarios hacen clic en un botón Editar, que abre un componente `EditGroceryItem`.
Quieres que ese componente recupere el `id` del artículo de la compra para poder mostrar la información correcta al usuario.

Puedes utilizar una ruta para pasar este tipo de información a los componentes de tu aplicación.
Para ello, utiliza la interfaz [ActivatedRoute](api/router/ActivatedRoute).

Para obtener información de una ruta:

1. Importe `ActivatedRoute` y `ParamMap` a su componente.

   <code-example path="router/src/app/heroes/hero-detail/hero-detail.component.ts" region="imports-route-info" header="In the component class (excerpt)">
   </code-example>

   Estas declaraciones `import` añaden varios elementos importantes que su componente necesita.
   Para saber más sobre cada uno de ellos, consulte las siguientes páginas de la API:

   - [`Router`](api/router)
   - [`ActivatedRoute`](api/router/ActivatedRoute)
   - [`ParamMap`](api/router/ParamMap)

2. Inyecta una instancia de `ActivatedRoute` añadiéndola al constructor de tu aplicación:

  <code-example path="router/src/app/heroes/hero-detail/hero-detail.component.ts" region="activated-route" header="In the component class (excerpt)">
  </code-example>

3. Actualizar el método `ngOnInit()` para acceder a la `ActivatedRoute` y rastrear el parámetro `id`:

  <code-example header="In the component (excerpt)">
    ngOnInit() {
      this.route.queryParams.subscribe(params => {
        this.name = params['name'];
      });
    }
  </code-example>

    Nota: El ejemplo anterior utiliza una variable, `name`, y le asigna el valor basado en el parámetro `name`.

{@a setting-up-wildcard-routes}

## Configuración de rutas comodín

Una aplicación que funciona bien debería manejar con elegancia cuando los usuarios intentan navegar a una parte de su aplicación que no existe.
Para añadir esta funcionalidad a tu aplicación, configuras una ruta comodín.
El enrutador de Angular selecciona esta ruta cada vez que la URL solicitada no coincide con ninguna ruta del enrutador.

Para configurar una ruta comodín, añada el siguiente código a su definición de `routes`

<code-example header="AppRoutingModule (excerpt)">

{ path: `**`, component: <component-name> }

</code-example>

Los dos asteriscos, `**`, indican a Angular que esta definición de `routes` es una ruta comodín.
Para la propiedad component, puedes definir cualquier componente de tu aplicación.
Las opciones más comunes incluyen un `PageNotFoundComponent` específico de la aplicación, que puedes definir para [mostrar una página 404](guide/router#404-page-how-to) a tus usuarios; o una redirección al componente principal de tu aplicación.
Una ruta comodín es la última ruta porque coincide con cualquier URL.
Para más detalles sobre por qué es importante el orden de las rutas, consultar [orden de las rutas](guide/router#route-order).

{@a 404-page-how-to}

## Visualización de una página 404

Para mostrar una página 404, configure una [ruta comodín](guide/router#setting-up-wildcard-routes) con la propiedad `component` establecida en el componente que desea utilizar para su página 404 como se muestra a continuación:

<code-example path="router/src/app/app-routing.module.8.ts" region="routes-with-wildcard" header="AppRoutingModule (excerpt)">

</code-example>

La última ruta con el `path` de `**` es una ruta comodín.
El enrutador selecciona esta ruta si la URL solicitada no coincide con ninguna de las rutas anteriores de la lista y envía al usuario al `PageNotFoundComponent`.

## Configuración de los redireccionamientos

Para establecer una redirección, configure una ruta con el atributo `path` desde donde se quiere redireccionar, establezca el atributo `redirectTo` con la dirección hacia donde se quiere redireccionar, y el valor `pathMatch` que indica al router cómo hacer coincidir la URL.

<code-example path="router/src/app/app-routing.module.8.ts" region="redirect" header="AppRoutingModule (excerpt)">

</code-example>

En este ejemplo, la tercera ruta es una redirección para que el enrutador se dirija de la ruta por defecto hacia la ruta `first-component`.
Observe que esta redirección precede a la ruta comodín.
Aquí, `path: ''` significa utilizar la URL relativa inicial (`''`).

Para más detalles sobre `pathMatch`, consulte [Spotlight on pathMatch](guide/router-tutorial-toh#pathmatch).

{@a nesting-routes}

## Rutas anidadas

A medida que su aplicación se hace más compleja, es posible que quiera crear rutas que sean relativas a un componente que no sea el componente raíz.
Este tipo de rutas anidadas son llamadas rutas hijas.
Esto significa que estás añadiendo un segundo `<router-outlet>` a tu aplicación, porque se suma al `<router-outlet>` de `AppComponent`.

En este ejemplo, hay dos componentes hijos adicionales, `child-a`, y `child-b`.
Aquí, `FirstComponent` tiene su propio `<nav>` y un segundo `<router-outlet>` además del de `AppComponent`.

<code-example path="router/src/app/app.component.8.html" region="child-routes" header="In the template">

</code-example>

Una ruta hija es como cualquier otra ruta, ya que necesita tanto un `path` como un `component`.
La única diferencia es que las rutas hijas se colocan en un array `children` dentro de la ruta padre.

<code-example path="router/src/app/app-routing.module.9.ts" region="child-routes" header="AppRoutingModule (excerpt)">

</code-example>

{@a using-relative-paths}

## Uso de rutas relativas

Las rutas relativas permiten la definición de direcciones que son relativas al segmento de la URL actual.
El siguiente ejemplo muestra una ruta relativa a otro componente, `second-component`.
El componente `FirstComponent` y `SecondComponent` están al mismo nivel en el árbol, sin embargo, el enlace al `SecondComponent` esta situado dentro del `FirstComponent`, lo que significa que el router tiene que subir un nivel y luego en el segundo directorio encontrar el `SecondComponent`.
En lugar de escribir toda la ruta para llegar al `Segundo Componente`, se puede utilizar la notación `../` para subir un nivel.

<code-example path="router/src/app/app.component.8.html" region="relative-route" header="In the template">

</code-example>

Además de `../`, se puede utilizar `./` o sin diagonal para especificar el nivel actual.

### Especificar una ruta relativa

Para especificar una ruta relativa, utilice la propiedad `NavigationExtras` `relativeTo`.
En la clase del componente, importa `NavigationExtras` de `@angular/router`.

A continuación, utilice `relativeTo` en su método de navegación.
Después del array de parámetros del enlace, que aquí contiene `items`, añade un objeto con la propiedad `relativeTo` establecida a la `ActivatedRoute`, que es `this.route`.

<code-example path="router/src/app/app.component.4.ts" region="relative-to" header="RelativeTo">

The `navigate()` arguments configure the router to use the current route as a basis upon which to append `items`.

</code-example>

El método `goToItems()` interpreta el URI de destino como relativo a la ruta activada y navega a la ruta `items`.

## Accediendo a los parámetros y fragmentos

A veces, una función de tu aplicación requiere acceder a una parte de una ruta, como un parámetro o un fragmento. La aplicación Tour of Heroes en esta etapa del tutorial utiliza una lista en la que puedes hacer clic en un héroe para ver los detalles. El enrutador utiliza un `id` para mostrar los detalles del héroe correcto.

En primer lugar, importa los siguientes miembros en el componente desde el que quieres navegar.

<code-example header="Component import statements (excerpt)">

import { ActivatedRoute } from '@angular/router';
import { Observable } from 'rxjs';
import { switchMap } from 'rxjs/operators';

</code-example>

A continuación, inyecte el servicio `ActivatedRoute`:

<code-example header="Component (excerpt)">
  constructor(private route: ActivatedRoute) {}
</code-example>

Configura la clase para que tengas un observable, `heroes$`, un `selectedId` para mantener el número `id` del héroe, y los héroes en el `ngOnInit()`, añade el siguiente código para obtener el `id` del héroe seleccionado.
Este fragmento de código asume que tienes una lista de héroes, un servicio de héroes, una función para obtener tus héroes, y el HTML para renderizar tu lista y detalles, como en el ejemplo de Tour of Heroes.

<code-example header="Component 1 (excerpt)">

heroes$: Observable<Hero[]>;
selectedId: number;
heroes = HEROES;

ngOnInit() {
  this.heroes$ = this.route.paramMap.pipe(
    switchMap(params => {
      this.selectedId = Number(params.get('id'));
      return this.service.getHeroes();
    })
  );
}

</code-example>

A continuación, en el componente al que desea navegar, importe los siguientes miembros.

<code-example header="Component 2 (excerpt)">

import { Router, ActivatedRoute, ParamMap } from '@angular/router';
import { Observable } from 'rxjs';

</code-example>

Inyecta `ActivatedRoute` y `Router` en el constructor de la clase del componente para que estén disponibles para este componente:

<code-example header="Component 2 (excerpt)">

hero$: Observable<Hero>;

constructor(
private route: ActivatedRoute,
private router: Router ) {}

ngOnInit() {
  const heroId = this.route.snapshot.paramMap.get('id');
  this.hero$ = this.service.getHero(heroId);
}

gotoItems(hero: Hero) {
  const heroId = hero ? hero.id : null;
  // Pass along the hero id if available
  // so that the HeroList component can select that item.
  this.router.navigate(['/heroes', { id: heroId }]);
}

</code-example>

{@a lazy-loading}

## Carga diferida

Se pueden configurar las rutas para que carguen los módulos de forma diferida, eso significa que Angular sólo carga los módulos cuando es necesario, en lugar de cargar todos los módulos cuando se lanza la aplicación. Además, puedes precargar partes de tu aplicación en segundo plano para mejorar la experiencia del usuario.

Para más información sobre la carga diferida y la precarga, consulte la guía dedicada [Carga Diferida de NgModules](guide/lazy-loading-ngmodules).

{@a preventing-unauthorized-access}

## Prevención de accesos no autorizados

Utilice guards de rutas para evitar que los usuarios naveguen a partes de una aplicación sin autorización.
Los siguientes guards de rutas están disponibles en Angular:

- [`CanActivate`](api/router/CanActivate)
- [`CanActivateChild`](api/router/CanActivateChild)
- [`CanDeactivate`](api/router/CanDeactivate)
- [`Resolve`](api/router/Resolve)
- [`CanLoad`](api/router/CanLoad)

Para usar guards de rutas, considere el uso de [rutas sin componentes](api/router/Route#componentless-routes) ya que esto facilita la protección de las rutas hijas.

Creando un servicio para su guard:

<code-example language="none" class="code-shell">
  ng generate guard your-guard
</code-example>

En su clase de guard, implemente el guard que desea utilizar.
El siguiente ejemplo utiliza el guard `CanActivate` para proteger la ruta.

<code-example header="Component (excerpt)">
export class YourGuard implements CanActivate {
  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot): boolean {
      // your  logic goes here
  }
}
</code-example>

En su módulo de enrutamiento, utilice la propiedad adecuada en la configuración de `routes`.
Aquí, `canActivate` le dice al enrutador que medie la navegación hacia esta ruta en particular.

<code-example header="Routing module (excerpt)">
{
  path: '/your-path',
  component: YourComponent,
  canActivate: [YourGuard],
}
</code-example>

Para obtener más información con un ejemplo práctico, consulte la sección [tutorial de enrutamiento sobre route guards](guide/router-tutorial-toh#milestone-5-route-guards).

{@a link-parameters-array}

## Array de parámetros de enlace

Un array de parámetros enlazados contiene los siguientes ingredientes para la navegación del enrutador:

- La ruta al componente de destino.
- Parámetros de ruta obligatorios y opcionales que van en la URL de la ruta.

Puede vincular la directiva `RouterLink` a un array como este:

<code-example path="router/src/app/app.component.3.ts" header="src/app/app.component.ts (h-anchor)" region="h-anchor"></code-example>

El siguiente es un array de dos elementos dónde se especifica un parámetro de ruta:

<code-example path="router/src/app/heroes/hero-list/hero-list.component.1.html" header="src/app/heroes/hero-list/hero-list.component.html (nav-to-detail)" region="nav-to-detail"></code-example>

Puede proporcionar parámetros de ruta opcionales en un objeto, como en `{ foo: 'foo' }`:

<code-example path="router/src/app/app.component.3.ts" header="src/app/app.component.ts (cc-query-params)" region="cc-query-params"></code-example>

Estos tres ejemplos cubren las necesidades de una aplicación con un nivel de enrutamiento.
Sin embargo, con un enrutador hijo, como en el centro de crisis, se crean nuevas posibilidades de array de enlaces.

El siguiente ejemplo de `RouterLink` se basa en una [ruta hija por defecto](guide/router-tutorial-toh#a-crisis-center-with-child-routes) para el centro de crisis.

<code-example path="router/src/app/app.component.3.ts" header="src/app/app.component.ts (cc-anchor-w-default)" region="cc-anchor-w-default"></code-example>

Tenga en cuenta lo siguiente:

- El primer elemento del array identifica la ruta padre (`/crisis-center`).
- No hay parámetros para esta ruta padre.
- No hay ningún parámetro para la ruta hija, así que tienes que elegir uno.
- Estás navegando al `CrisisListComponent`, cuya ruta es `/`, pero no necesitas añadir explícitamente la barra.

Considere el siguiente enlace de ruta que navega desde la raíz de la aplicación hasta la Crisis del Dragón:

<code-example path="router/src/app/app.component.3.ts" header="src/app/app.component.ts (Dragon-anchor)" region="Dragon-anchor"></code-example>

- El primer elemento del array identifica la ruta padre (`/crisis-center`).
- No hay parámetros para esta ruta padre.
- El segundo elemento identifica la ruta hija detalles sobre una crisis concreta (`/:id`).
- La ruta hija detalles requiere un parámetro de ruta `id`.
- Ha añadido el `id` de la crisis del dragón como segundo elemento del array (`1`).
- La ruta resultante es `/crisis-center/1`.

También podría redefinir la plantilla `AppComponent` con rutas de Crisis Center exclusivamente:

<code-example path="router/src/app/app.component.3.ts" header="src/app/app.component.ts (template)" region="template"></code-example>

En resumen, se pueden escribir aplicaciones con uno, dos o más niveles de enrutamiento.
El array de parámetros de enlace ofrece la flexibilidad de representar cualquier profundidad de enrutamiento y cualquier secuencia de rutas, parámetros de enrutamiento (obligatorios) y objetos de parámetros de ruta (opcionales).

{@a browser-url-styles}

{@a location-strategy}

## `LocationStrategy` y estilos de URL del navegador

Cuando el enrutador navega a una nueva vista del componente, actualiza la ubicación y el historial del navegador con una URL para esa vista. Como se trata de una URL estrictamente local, el navegador no enviará esta URL al servidor y no recargará la página.

Los navegadores HTML5 modernos admiten <a href="https://developer.mozilla.org/en-US/docs/Web/API/History_API#Adding_and_modifying_history_entries" title="HTML5 browser history push-state">history.pushState</a>, una técnica que cambia la ubicación y el historial de un navegador sin provocar una solicitud de página al servidor.
El enrutador puede componer una URL "natural" que no se distingue de una que, de otro modo, requeriría una carga de página.

Aquí está la URL del Centro de Crisis en este estilo "HTML5 pushState":

<code-example format="nocode">

localhost:3002/crisis-center/

</code-example>

Los navegadores más antiguos envían solicitudes de página al servidor cuando la URL cambia, a menos que el cambio se produzca después de un "#" (llamado "hash").
Los enrutadores pueden aprovechar esta excepción componiendo URLs de ruta en la aplicación con hashes.
He aquí una "URL con hash" que dirige al Centro de Crisis.

<code-example format="nocode">

localhost:3002/src/#/crisis-center/

</code-example>

El enrutador soporta ambos estilos con dos proveedores de `LocationStrategy`:

1. `PathLocationStrategy`&mdash;el estilo "HTML5 pushState" por defecto.
2. `HashLocationStrategy`&mdash;el estilo "hash URL".

La función `RouterModule.forRoot()` establece la `LocationStrategy` como `PathLocationStrategy`, lo que la convierte en la estrategia por defecto.
También tienes la opción de cambiar a la `HashLocationStrategy` con un override durante el proceso de arranque.

<div class="alert is-helpful">

Para más información sobre los proveedores y el proceso de arranque, consulte [Inyección de dependencias](guide/dependency-injection#bootstrap).

</div>

## Elegir una estrategia de enrutamiento

Debes elegir una estrategia de enrutamiento al principio del desarrollo de tu proyecto porque una vez que la aplicación está en producción, los visitantes de tu sitio utilizan y dependen de las referencias de la URL de la aplicación.

Casi todos los proyectos de Angular deberían utilizar el estilo HTML5 por defecto.
Produce URLs que son más fáciles de entender para los usuarios y preserva la opción de hacer el renderizado del lado del servidor.

Renderizar las páginas críticas en el servidor es una técnica que puede mejorar mucho la capacidad de respuesta percibida cuando la aplicación se carga por primera vez.
Una aplicación que, de otro modo, tardaría diez o más segundos en iniciarse, podría renderizarse en el servidor y llegar al dispositivo del usuario en menos de un segundo.

Esta opción sólo está disponible si las URLs de la aplicación se parecen a las URLs normales de la web sin hash (#) en el medio.

## `<base href>`

El enrutador utiliza el <a href="https://developer.mozilla.org/en-US/docs/Web/API/History_API#Adding_and_modifying_history_entries" title="HTML5 browser history push-state">history.pushState</a> del navegador para la navegación.
`pushState` permite personalizar las rutas de URL de la aplicación; por ejemplo, `localhost:4200/crisis-center`.
Las URLs dentro de la aplicación pueden ser indistinguibles de las URLs del servidor.

Los navegadores HTML5 modernos fueron los primeros en soportar `pushState`, por lo que mucha gente se refiere a estas URLs como URLs "estilo HTML5".

<div class="alert is-helpful">

El estilo de navegación HTML5 es el predeterminado del enrutador.
En la sección [LocationStrategy y estilos de URL del navegador](#browser-url-styles), aprende por qué es preferible el estilo HTML5, cómo ajustar su comportamiento y cómo cambiar al estilo hash (#) más antiguo, si es necesario.

</div>

Debes añadir un elemento <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base" title="base href">&lt;base href&gt;</a> al `index.html` de la aplicación para que el enrutamiento `pushState` funcione.
El navegador utiliza el valor de `<base href>` para poner un prefijo a las URLs relativas cuando hace referencia a archivos CSS, scripts e imágenes.

Añade el elemento `<base>` justo después de la etiqueta `<head>`.
Si la carpeta `app` es la raíz de la aplicación, como lo es para esta aplicación,
establece el valor de `href` en `index.html` como se muestra aquí.

<code-example path="router/src/index.html" header="src/index.html (base-href)" region="base-href"></code-example>

### URLs de HTML5 y el `<base href>`.

Las directrices que siguen se refieren a diferentes partes de una URL. Este diagrama indica a qué se refieren esas partes:

```
foo://example.com:8042/over/there?name=ferret#nose
\_/   \______________/\_________/ \_________/ \__/
 |           |            |            |        |
scheme    authority      path        query   fragment
```

Aunque el enrutador utiliza el estilo <a href="https://developer.mozilla.org/en-US/docs/Web/API/History_API#Adding_and_modifying_history_entries" title="Browser history push-state">HTML5 pushState</a> por defecto, debes configurar esa estrategia con un `<base href>`.

La forma preferida de configurar la estrategia es añadir la etiqueta <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base" title="base href">&lt;base href&gt;</a> en el `<head>` del `index.html`.

<code-example path="router/src/index.html" header="src/index.html (base-href)" region="base-href"></code-example>

Sin esa etiqueta, es posible que el navegador no pueda cargar recursos
(imágenes, CSS, scripts) cuando se hace un "deep linking" en la aplicación.

Es posible que algunos desarrolladores no puedan añadir el elemento `<base>`, quizás porque no tienen acceso a `<head>` o al `index.html`.

Esos desarrolladores pueden seguir utilizando las URL de HTML5 siguiendo los dos pasos siguientes:

1. Proporcione al enrutador un valor apropiado de `APP_BASE_HREF`.
2. Utilice URLs raíz (URLs con una `authority`) para todos los recursos web: CSS, imágenes, scripts y archivos HTML de la plantilla.

- El `<base href>` `path` debe terminar con un "/", ya que los navegadores ignoran los caracteres del `path` que siguen al "/" del extremo derecho.
- Si la `<base href>` incluye una parte de `query`, la `query` sólo se utiliza si el `path` de un enlace en la página está vacío y no tiene `query`.
  Esto significa que una `query` en la `<base href>` sólo se incluye cuando se utiliza `HashLocationStrategy`.
- Si un enlace de la página es una URL raíz (tiene una `authority`), no se utiliza el valor de `<base href>`. De este modo, un `APP_BASE_HREF` con una autoridad hará que todos los enlaces creados por Angular ignoren el valor de `<base href>`.
- Un fragmento en la `<base href>` no se persiste _nunca_.

Para obtener información más completa sobre cómo se utiliza `<base href>` para construir URIs de destino, consulte la sección [RFC](https://tools.ietf.org/html/rfc3986#section-5.2.2) sobre la transformación de referencias.

{@a hashlocationstrategy}

### `HashLocationStrategy`

Puedes utilizar `HashLocationStrategy` proporcionando el `useHash: true` en un objeto como segundo argumento del `RouterModule.forRoot()` en el `AppModule`.

<code-example path="router/src/app/app.module.6.ts" header="src/app/app.module.ts (hash URL strategy)"></code-example>

{@a router-reference}

## Referencia del enrutador

En las siguientes secciones se destacan algunos conceptos básicos del enrutador.

{@a basics-router-imports}

### Importando el enrutador

El enrutador de Angular es un servicio opcional que presenta una vista de componente particular para una URL dada.
No forma parte del núcleo de Angular y, por tanto, se encuentra en su propio paquete de bibliotecas, `@angular/router`.

Importa lo que necesites de él como lo harías de cualquier otro paquete de Angular.

<code-example path="router/src/app/app.module.1.ts" header="src/app/app.module.ts (import)" region="import-router"></code-example>

<div class="alert is-helpful">

Para más información sobre los estilos de URL del navegador, consulta [`LocationStrategy` and browser URL styles](#locationstrategy-y-estilos-de-url-del-navegador).

</div>

{@a basics-config}
{@a configuration}

### Configuración

Una aplicación Angular enrutada tiene una única instancia del servicio `Router`.
Cuando la URL del navegador cambia, ese enrutador busca una `Ruta` correspondiente a partir de la cual puede determinar el componente a mostrar.

Un router no tiene rutas hasta que lo configuras.
El siguiente ejemplo crea cinco definiciones de ruta, configura el enrutador a través del método `RouterModule.forRoot()` y añade el resultado al array `imports` del `AppModule`.

<code-example path="router/src/app/app.module.0.ts" header="src/app/app.module.ts (excerpt)"></code-example>

{@a example-config}

El array de rutas `appRoutes` describe cómo navegar.
Pásalo al método `RouterModule.forRoot()` del módulo `imports` para configurar el router.

Cada `Route` asigna un `path` URL a un componente.
No hay barras inclinadas en el camino.
El enrutador analiza y construye la URL final por ti, lo que te permite utilizar tanto rutas relativas como absolutas al navegar entre las vistas de la aplicación.

El `:id` en la segunda ruta es un token para un parámetro.
En una URL como `/hero/42`, "42" es el valor del parámetro `id`.
El correspondiente `HeroDetailComponent` utiliza ese valor para encontrar y presentar al héroe cuyo `id` es 42.

La propiedad `data` de la tercera ruta es un lugar para almacenar datos arbitrarios asociados a
esta ruta específica.
La propiedad data es accesible dentro de cada ruta activada. Utilícela para almacenar elementos como los títulos de las páginas, el texto de las migas de pan y otros datos estáticos de sólo lectura.
Puede utilizar la [resolve guard](guide/router-tutorial-toh#resolve-guard) para recuperar datos dinámicos.

La dirección vacía en la cuarta ruta representa la ruta por defecto de la aplicación&mdash;el lugar al que hay que ir cuando la ruta en la URL está vacía, como suele ocurrir al principio.
Esta ruta por defecto redirige a la ruta de la URL `/heroes` y, por tanto, muestra el `HeroesListComponent`.

Si necesitas ver qué eventos están ocurriendo durante el ciclo de vida de la navegación, existe la opción `enableTracing` como parte de la configuración por defecto del router.
Esto muestra cada evento del router que tuvo lugar durante cada ciclo de vida de la navegación en la consola del navegador.
Utiliza `enableTracing` sólo para propósitos de depuración.
Se establece la opción `enableTracing: true` en el objeto que se pasa como segundo argumento al método `RouterModule.forRoot()`.

{@a basics-router-outlet}

### Router outlet

El `RouterOutlet` es una directiva de la librería del enrutador que se utiliza como un componente.
Actúa como un marcador de posición que marca el lugar en la plantilla donde el enrutador debe
mostrar los componentes para esa salida.

<code-example language="html">
  &lt;router-outlet>&lt;/router-outlet>
  &lt;!-- Routed components go here -->

</code-example>

Dada la configuración anterior, cuando la URL del navegador para esta aplicación se convierte en `/heroes`, el enrutador hace coincidir esa URL con la ruta `/heroes` y muestra el `HeroListComponent` como elemento hermano del `RouterOutlet` que has colocado en la plantilla del componente anfitrión.

{@a basics-router-links}

{@a router-link}

### Enlaces del enrutador

Para navegar como resultado de alguna acción del usuario, como el clic de una etiqueta de anclaje, utilice `RouterLink`.

Considere la siguiente plantilla:

<code-example path="router/src/app/app.component.1.html" header="src/app/app.component.html"></code-example>

Las directivas `RouterLink` en las etiquetas de anclaje dan al enrutador el control sobre esos elementos.
Las rutas de navegación son fijas, por lo que se puede asignar una cadena al `RouterLink` (un enlace "único").

Si la ruta de navegación fuera más dinámica, podría haberse vinculado a una expresión de plantilla que devolviera un array de parámetros enlazados; es decir, el [array de parámetros enlazados](guide/router#link-parameters-array).
El enrutador resuelve ese array en una URL completa.

{@a router-link-active}

### Enlaces activos del enrutador

La directiva `RouterLinkActive` cambia las clases CSS de los enlaces `RouterLink` activos en función del `RouterState` actual.

En cada etiqueta de anclaje, se ve una [property binding](guide/property-binding) a la directiva `RouterLinkActive` que se parece a `routerLinkActive="..."`.

La expresión de la plantilla a la derecha del signo igual, `=`, contiene una cadena delimitada por espacios de clases CSS que el Router añade cuando este enlace está activo (y elimina cuando el enlace está inactivo).
Usted establece la directiva `RouterLinkActive` a una cadena de clases como `[routerLinkActive]="'active fluffy'"` o la vincula a una propiedad del componente que devuelve dicha cadena.

Los enlaces de ruta activos descienden en cascada por cada nivel del árbol de rutas, por lo que los enlaces de los enrutadores padre e hijo pueden estar activos al mismo tiempo.
Para anular este comportamiento, puede vincularse a la entrada `[routerLinkActiveOptions]` con la expresión `{ exact: true }`. Al utilizar `{ exact: true }`, un determinado `RouterLink` sólo estará activo si su URL coincide exactamente con la URL actual.

{@a basics-router-state}

### Estado del enrutador

Tras el final de cada ciclo de vida de la navegación con éxito, el enrutador construye un árbol de objetos `ActivatedRoute` que conforman el estado actual del enrutador. Puedes acceder al `RouterState` actual desde cualquier parte de la aplicación utilizando el servicio `Router` y la propiedad `routerState`.

Cada `ActivatedRoute` en el `RouterState` proporciona métodos para recorrer el árbol de rutas hacia arriba y hacia abajo para obtener información de las rutas padre, hijo y hermano.

{@a activated-route}

### Activated route

La ruta y los parámetros están disponibles a través de un servicio de enrutamiento inyectado llamado [ActivatedRoute](api/router/ActivatedRoute).
Tiene una gran cantidad de información útil, incluyendo:

<table>
  <tr>
    <th>
      Propiedad
    </th>
    <th>
      Descripción
    </th>
  </tr>

  <tr>
    <td>
      <code>url</code>
    </td>
    <td>
    Un `Observable` de la(s) ruta(s), representado como un array de cadenas para cada parte de la ruta.
    </td>

  </tr>

  <tr>
    <td>
      <code>data</code>
    </td>
    <td>
    Un `Observable` que contiene el objeto `data` proporcionado para la ruta.
    También contiene cualquier valor resuelto de la  [resolve guard](guide/router-tutorial-toh#resolve-guard).
    </td>
  </tr>

  <tr>
    <td>
      <code>paramMap</code>
    </td>
    <td>
    Un `Observable` que contiene un [mapa](api/router/ParamMap) de los parámetros obligatorios y [opcionales](guide/router-tutorial-toh#optional-route-parameters) específicos de la ruta.
    El mapa admite la recuperación de valores únicos y múltiples del mismo parámetro.
    </td>

  </tr>

  <tr>
    <td>
      <code>queryParamMap</code>
    </td>
    <td>
    Un `Observable` que contiene un [mapa](api/router/ParamMap) de los [parámetros de consulta](guide/router-tutorial-toh#query-parameters) disponibles para todas las rutas.
    El mapa soporta la recuperación de valores simples y múltiples del parámetro de consulta.
    </td>

  </tr>

  <tr>
    <td>
      <code>fragment</code>
    </td>
    <td>
      Un `Observable` de la URL [fragment](guide/router-tutorial-toh#fragment) disponible para todas las rutas.
    </td>
  </tr>

  <tr>
    <td>
      <code>outlet</code>
    </td>
    <td>
    El nombre del `RouterOutlet` utilizado para representar la ruta.
    Para un outlet sin nombre, el nombre de la salida es primario.
    </td>
  </tr>

  <tr>
    <td>
      <code>routeConfig</code>
    </td>
    <td>
    La configuración de la ruta utilizada para la ruta que contiene la dirección de origen.
    </td>
  </tr>

  <tr>
    <td>
      <code>parent</code>
    </td>
    <td>
    El padre de la ruta `ActivatedRoute` cuando esta ruta es un [child route](guide/router-tutorial-toh#child-routing-component).
    </td>
  </tr>

  <tr>
    <td>
      <code>firstChild</code>
    </td>
    <td>
    Contiene el primer `ActivatedRoute` de la lista de rutas hijas de esta ruta.
    </td>
  </tr>

  <tr>
    <td>
      <code>children</code>
    </td>
    <td>
    Contiene todas las [child routes](guide/router-tutorial-toh#child-routing-component) activadas bajo la ruta actual.
    </td>
  </tr>
</table>

<div class="alert is-helpful">

Dos propiedades más antiguas todavía están disponibles, sin embargo, sus reemplazos son preferibles ya que pueden ser obsoletos en una futura versión de Angular.

- `params`: Un `Observable` que contiene los parámetros obligatorios y [opcionales](guide/router-tutorial-toh#optional-route-parameters) específicos de la ruta. Utiliza `paramMap` en su lugar.

- `queryParams`: Un `Observable` que contiene los [query parameters](guide/router-tutorial-toh#query-parameters) disponibles para todas las rutas. Utiliza `queryParamMap` en su lugar.

</div>

{@a router-events}

### Eventos del enrutador

Durante cada navegación, el `Router` emite eventos de navegación a través de la propiedad `Router.events`.
Estos eventos abarcan desde el inicio y el final de la navegación hasta muchos puntos intermedios. La lista completa de eventos de navegación se muestra en la siguiente tabla.

<table>
  <tr>
    <th>
      Evento
    </th>
    <th>
      Descripción
    </th>
  </tr>

  <tr>
    <td>
      <code>NavigationStart</code>
    </td>
    <td>
      Un [evento](api/router/NavigationStart) disparado cuando se inicia la navegación.
    </td>

  </tr>

  <tr>
    <td>
      <code>RouteConfigLoadStart</code>
    </td>
    <td>
      Un [evento](api/router/RouteConfigLoadStart) disparado antes de que el `Router` [cargue](guide/router-tutorial-toh#asynchronous-routing) una configuración de ruta.
    </td>
  </tr>

  <tr>
    <td>
      <code>RouteConfigLoadEnd</code>
    </td>
    <td>
     Un [evento](api/router/RouteConfigLoadEnd) que se dispara después de que una ruta haya sido cargada de forma diferida.
    </td>
  </tr>

  <tr>
    <td>
      <code>RoutesRecognized</code>
    </td>
    <td>
      Un [evento](api/router/RoutesRecognized) disparado cuando el router analiza la URL y las rutas son reconocidas.
    </td>

  </tr>

  <tr>
    <td>
      <code>GuardsCheckStart</code>
    </td>
    <td>
       Un [evento](api/router/GuardsCheckStart) disparado cuando el router comienza la fase de guardias del enrutamiento.
    </td>
  </tr>

  <tr>
    <td>
      <code>ChildActivationStart</code>
    </td>
    <td>
      Un [evento](api/router/ChildActivationStart) disparado cuando el router comienza a activar los hijos de una ruta.
    </td>
  </tr>

  <tr>
    <td>
      <code>ActivationStart</code>
    </td>
    <td>
      Un [evento](api/router/ActivationStart) disparado cuando el router comienza a activar una ruta.
    </td>
  </tr>

  <tr>
    <td>
      <code>GuardsCheckEnd</code>
    </td>
    <td>
      Un [evento](api/router/GuardsCheckEnd) que se dispara cuando el router termina la fase de guardias del enrutamiento con éxito.
    </td>
  </tr>

  <tr>
    <td>
      <code>ResolveStart</code>
    </td>
    <td>
      Un [evento](api/router/ResolveStart) que se activa cuando el router comienza la fase de resolución del enrutamiento.
    </td>
  </tr>

  <tr>
    <td>
      <code>ResolveEnd</code>
    </td>
    <td>
      Un [evento](api/router/ResolveEnd) que se activa cuando el router termina la fase de resolución del enrutamiento con éxito.
    </td>
  </tr>

  <tr>
    <td>
      <code>ChildActivationEnd</code>
    </td>
    <td>
      Un [evento](api/router/ChildActivationEnd) disparado cuando el router termina de activar los hijos de una ruta.
    </td>
  </tr>

  <tr>
    <td>
      <code>ActivationEnd</code>
    </td>
    <td>
      Un [evento](api/router/ActivationStart) disparado cuando el router termina de activar una ruta.
    </td>
  </tr>

  <tr>
    <td>
      <code>NavigationEnd</code>
    </td>
    <td>
      Un [evento](api/router/NavigationEnd) que se dispara cuando la navegación termina con éxito.
    </td>

  </tr>

  <tr>
    <td>
      <code>NavigationCancel</code>
    </td>
    <td>
      Un [evento](api/router/NavigationCancel) se dispara cuando se cancela la navegación.
      Esto puede ocurrir cuando un [Route Guard](guide/router-tutorial-toh#guards) devuelve false durante la navegación,
      o redirecciona devolviendo un `UrlTree`.
    </td>
  </tr>

  <tr>
    <td>
      <code>NavigationError</code>
    </td>
    <td>
      Un [evento](api/router/NavigationError) disparado cuando la navegación falla debido a un error inesperado.
    </td>

  </tr>

  <tr>
    <td>
      <code>Scroll</code>
    </td>
    <td>
      Un [evento](api/router/Scroll) que representa un evento de desplazamiento.
    </td>

  </tr>
</table>

Al activar la opción `enableTracing`, Angular registra estos eventos en la consola.
Para ver un ejemplo de filtrado de eventos de navegación del router, consulta la sección [router](guide/observables-in-angular#router) de la guía [Observables in Angular](guide/observables-in-angular).

### Terminología del enrutador

Estos son los términos clave del `Router` y su significado:

<table>
  <tr>
    <th>
      Pieza del enrutador
    </th>
    <th>
      Significado
    </th>
  </tr>

  <tr>
    <td>
      <code>Router</code>
    </td>
    <td>
      Muestra el componente de la aplicación para la URL activa. Gestiona la navegación de un componente a otro.
    </td>
  </tr>

  <tr>
    <td>
      <code>RouterModule</code>
    </td>
    <td>
      Un NgModule separado que proporciona los proveedores de servicios necesarios y directivas para navegar por las
      vistas de la aplicación.
    </td>
  </tr>

  <tr>
    <td>
      <code>Routes</code>
    </td>
    <td>
      Define un array de rutas, cada una de las cuales asigna una dirección URL a un componente.
    </td>
  </tr>

  <tr>
    <td>
      <code>Route</code>
    </td>
    <td>
      Define cómo el enrutador debe navegar a un componente basado en un patrón de URL.
      La mayoría de las rutas consisten en una ruta y un tipo de componente.
    </td>
  </tr>

  <tr>
    <td>
      <code>RouterOutlet</code>
    </td>
    <td>
      La directiva (<code>&lt;router-outlet></code>) que marca dónde el router muestra una vista.
    </td>
  </tr>

  <tr>
    <td>
      <code>RouterLink</code>
    </td>
    <td>
      La directiva para vincular un elemento HTML en el que se puede hacer clic a una ruta. Al hacer clic en un elemento con una directiva <code>routerLink</code> vinculada a una <i>cadena</i> o a una <i>array de parámetros de enlace</i> se desencadena una navegación.
    </td>
  </tr>

  <tr>
    <td>
      <code>RouterLinkActive</code>
    </td>
    <td>
      La directiva para añadir/eliminar clases de un elemento HTML cuando un <code>routerLink</code> asociado contenido en el elemento o dentro de él se activa/inactiva.
    </td>
  </tr>

  <tr>
    <td>
      <code>ActivatedRoute</code>
    </td>
    <td>
    Un servicio que se proporciona a cada componente de la ruta y que contiene información específica de la ruta, como los parámetros de la ruta, los datos estáticos, los datos de resolución, los parámetros de consulta global y el fragmento global.
    La directiva para añadir/eliminar clases de un elemento HTML cuando un <code>routerLink</code> asociado contenido en el elemento o dentro de él se activa/inactiva.
    </td>
  </tr>

  <tr>
    <td>
      <code>RouterState</code>
    </td>
    <td>
      El estado actual del router, incluyendo un árbol de las rutas actualmente activadas junto con métodos convenientes para recorrer el árbol de rutas.
    </td>
  </tr>

  <tr>
    <td>
      <b><i>Link parameters array</i></b>
    </td>
    <td>
      Una matriz que el enrutador interpreta como una instrucción de enrutamiento.
      Puedes vincular ese array a un <code>RouterLink</code> o pasar el array como argumento al método <code>Router.navigate</code>.
    </td>
  </tr>

  <tr>
    <td>
      <b><i>Routing component</i></b>
    </td>
    <td>
      Un componente de Angular con un <código>RouterOutlet</código> que muestra vistas basadas en las navegaciones del router.
    </td>
  </tr>
</table>
