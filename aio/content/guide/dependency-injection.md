# Inyección de dependencias en Angular

La inyección de dependencias (ID), es un importante patrón de diseño de aplicaciones.
Angular tiene su propio framework de ID, el cual es típicamente
usado en el diseño de aplicaciones de Angular para incrementar su eficiencia y modularidad.

Las dependencias son servicios u objetos que una clase necesita para llevar a cabo su función.
ID es un patrón de código en el cual una clase pregunta por sus dependencias a fuentes externas en vez de crearlas ella misma.

En Angular, el framework de ID provee dependencias declaradas a una clase cuando la clase es instanciada. Esta guía explica como la ID trabaja en Angular, y como puedes usarla para hacer tus apps flexibles, eficientes, y robustas, así como testeables y mantenibles.

<div class="alert is-helpful">

 Puedes correr el <live-example></live-example> del ejemplo que acompaña esta guía.

</div>

Empieza revisando esta versión simplificada de la funcionalidad de _héroes_
de [El Tour de Héroes](tutorial/). Esta simple versión no hace uso de ID; Veremos como convertirla para que lo haga.

<code-tabs>
  <code-pane header="src/app/heroes/heroes.component.ts" path="dependency-injection/src/app/heroes/heroes.component.1.ts" region="v1">
  </code-pane>

  <code-pane header="src/app/heroes/hero-list.component.ts" path="dependency-injection/src/app/heroes/hero-list.component.1.ts">
  </code-pane>

  <code-pane header="src/app/heroes/hero.ts" path="dependency-injection/src/app/heroes/hero.ts">
  </code-pane>

  <code-pane header="src/app/heroes/mock-heroes.ts" path="dependency-injection/src/app/heroes/mock-heroes.ts">
  </code-pane>

</code-tabs>

`HeroesComponent` es el componente de héroes de nivel superior.
Su único propósito en mostrar `HeroListComponent`, el cual muestra una lista de nombres de héroes.

Esta versión de `HeroListComponent` obtiene los héroes de la lista `HEROES`, una colección en memoria
definida en un archivo `mock-heroes` separado.

<code-example header="src/app/heroes/hero-list.component.ts (class)" path="dependency-injection/src/app/heroes/hero-list.component.1.ts" region="class">
</code-example>

Este enfoque funciona para prototipado, pero no es robusto ni mantenible.
Tan pronto como intentes probar este componente u obtener los héroes de un servidor remoto,
debes cambiar la implementación de `HeroesListComponent` y
cada uso de los datos simulados de `HEROES`.


## Crear y registrar un servicio inyectable

El framework de ID te permite proveer datos a un componente desde una clase de _servicio_ inyectable, definida en su propio archivo. Para demostrarlo, vamos a crear una clase de servicio inyectable que provee una lista de heroes, y registrarla como un proveedor del servicio. 

<div class="alert is-helpful">

Tener múltiples clases en el mismo archivo puede ser confuso. Generalmente recomendamos que definas componentes y servicios en archivos separados.

Si vas a definir un componente y un servicio en el mismo archivo,
es importante definir el servicio primero, y luego el componente. Si defines el componente antes que el servicio, vas a obtener un error de referencia nula (null reference) en tiempo de ejecución.

Es posible definir el componente primero con la ayuda del método `forwardRef()` como hemos explicado en este [blog post](http://blog.thoughtram.io/angular/2015/09/03/forward-references-in-angular-2.html).

También puedes usar reenvío de referencias para romper dependencias circulares.
Mira un ejemplo en la [guía de ID](guide/dependency-injection-in-action#forwardref).

</div>

### Crear una clase de servicio inyectable

El [CLI de Angular](cli) puede generar una nueva clase `HeroService` en la carpeta `src/app/heroes` con este comando.

<code-example language="sh" class="code-shell">
ng generate service heroes/hero
</code-example>

El comando crea la siguiente estructura de `HeroService`.

<code-example path="dependency-injection/src/app/heroes/hero.service.0.ts" header="src/app/heroes/hero.service.ts (CLI-generated)">
</code-example>

`@Injectable()` es un ingrediente esencial en la definición de cada servicio en Angular. El resto de la clase ha sido escrito para exponer el método `getHeroes` que retorna los mismos datos de prueba anteriores. (Una aplicación real probablemente podría obtener estos datos asíncronamente desde un servidor remoto, pero ignoraremos eso para enfocarnos en la mecánica de inyectar el servicio.)

<code-example path="dependency-injection/src/app/heroes/hero.service.3.ts" header="src/app/heroes/hero.service.ts">
</code-example>


{@a injector-config}
{@a bootstrap}

### Configurar un inyector con un proveedor de servicio

La clase que hemos creado provee un servicio. El decorador `@Injectable()` lo marca como un servicio
que puede ser inyectado, pero de hecho, Angular no puede inyectar este hasta que configures
un [inyector de dependencias](guide/glossary#injector) de Angular con un [proveedor](guide/glossary#provider) del servicio.

El inyector es el responsable de crear las instancias de los servicios e inyectarlos en las clases como `HeroListComponent`.
Raramente vas a crear un inyector de Angular tú mismo. Angular crea inyectores para ti a medida que ejecuta la aplicación, empezando por el _inyector raíz_ que es creado durante el [proceso de carga](guide/bootstrapping).

Un proveedor le dice a un inyector _como crear el servicio_.
Debes configurar un inyector con un proveedor antes que el inyector pueda crear un servicio (o proveer cualquier tipo de dependencia).

Un proveedor puede ser la misma clase de servicio, de modo que el inyector puede usar `new` para crear una instancia.
También puedes definir más de una clase que provee el mismo servicio de diferentes maneras,
y configurar diferentes inyectores con diferentes proveedores.

<div class="alert is-helpful">

Los inyectores se heredan, lo que significa que si un determinado inyector no puede resolver una dependencia,
este pregunta al inyector padre para que la resuelva.
Un componente puede obtener servicios de su propio inyector,
de los inyectores de sus componentes padres,
del inyector de su NgModule padre, o del inyector `raíz`.

* Obtén más información de los [diferentes tipos de proveedores](guide/dependency-injection-providers).

* Obtén más información de como la [jerarquía de inyectores](guide/hierarchical-dependency-injection) trabaja.

</div>

Puedes configurar inyectores con proveedores a diferentes niveles en tu aplicación, estableciendo un valor de metadatos en uno de tres lugares:

* En el decorador `@Injectable()` para el servicio mismo.

* En el decorador `@NgModule()` para un NgModule.

* En el decorador `@Component()` para un componente.

El decorador `@Injectable()` tiene la opción del metadato `providedIn`, donde puedes especificar el proveedor de la clase de servicio decorada con el inyector `root`, o con el inyector para el NgModule especifico. 

Los decoradores `@NgModule()` y `@Component()` tienen la opción del metadato `providers`, donde puedes configurar proveedores para los inyectores al nivel del NgModule o del componente.

<div class="alert is-helpful">

Los componentes son directivas, y la opción `providers` es heredada de `@Directive()`. También puedes configurar proveedores para directivas y pipes al mismo nivel que el componente.

Obtén más información sobre [donde configurar los proveedores](guide/hierarchical-dependency-injection).

</div>

{@a injector-config}
{@a bootstrap}

## Inyectando servicios

Para que `HeroListComponent` obtenga los héroes de `HeroService`, este necesita pedir que `HeroService` sea inyectado, en lugar de crear su propia instancia de `HeroService` con `new`.

Puedes decirle a Angular que inyecte una dependencia en el constructor de un componente especificando un **parámetro en el constructor con el tipo de dependencia**. Allí es donde el constructor de `HeroListComponent` pide que `HeroService` sea inyectado

<code-example header="src/app/heroes/hero-list.component (constructor signature)" path="dependency-injection/src/app/heroes/hero-list.component.ts"
region="ctor-signature">
</code-example>

Por supuesto, `HeroListComponent` debería hacer algo con el `HeroService` inyectado.
Aquí está el componente revisado, haciendo uso del servicio inyectado, en paralelo con la versión previa para su comparación .

<code-tabs>
  <code-pane header="hero-list.component (with DI)" path="dependency-injection/src/app/heroes/hero-list.component.2.ts">
  </code-pane>

  <code-pane header="hero-list.component (without DI)" path="dependency-injection/src/app/heroes/hero-list.component.1.ts">
  </code-pane>
</code-tabs>

`HeroService` debe proveerse en algún inyector padre. El código en `HeroListComponent` no depende de donde proviene `HeroService`.
Si decidiste proveer `HeroService` en `AppModule`, `HeroListComponent` no cambiaría.

{@a singleton-services}
{@a component-child-injectors}

### Jerarquía de inyectores e instancias del servicio

Los servicios son singletons _dentro del ámbito de un inyector_. Es decir, hay como máximo una instancia de un servicio en un inyector dado.

Solo hay un inyector raíz para una aplicación. Proporcionar `UserService` en el nivel `root` o `AppModule` significa que está registrado con el inyector raíz. Solo hay una instancia de `UserService` en toda la aplicación y cada clase que inyecta `UserService` obtiene la instancia del servicio _a menos que_  configures otro proveedor con un _inyector hijo_.

Angular ID tiene un [sistema de inyección jerárquico](guide/hierarchical-dependency-injection), lo que significa que los inyectores anidados pueden crear sus propias instancias de servicio.
Angular crea inyectores anidados regularmente. Siempre que Angular crea una nueva instancia de un componente que tiene `proveedores` especificados en `@Component()`, también crea un nuevo _inyector hijo_ para la instancia.
Similarmente, cuando un nuevo NgModule se carga de forma diferida (lazy-loading) en tiempo de ejecución, Angular puede crear un inyector para el con sus propios proveedores.

Los módulos hijos y los inyectores de los componentes son independientes entre sí, y crean sus propias instancias independientes de los servicios proporcionados. Cuando Angular destruye una instancia de un NgModule o de un componente, también destruye el inyector y las instancias de los servicios del inyector.

Gracias a la [herencia del inyector](guide/hierarchical-dependency-injection),
aún puedes inyectar servicios para toda la aplicación en esos componentes.
El inyector de un componente es hijo del inyector de sus componentes padre, y hereda de todos los inyectores padres hasta el inyector _raíz_ de la aplicación. Angular puede inyectar un servicio proporcionado por cualquier inyector de ese linaje.

Por ejemplo, Angular puede inyectar `HeroListComponent` tanto con `HeroService` proporcionado en `HeroComponent`, como `UserService` proporcionado en `AppModule`.

{@a testing-the-component}

## Probando componentes con dependencias

Diseñar una clase con inyección de dependencias hace que la clase sea más fácil de probar.
Listar dependencias como parámetros del constructor puede ser todo lo que necesites para probar partes de la aplicación de manera efectiva.

Por ejemplo, puedes crear un nuevo `HeroListComponent` con un servicio simulado que puedas manipular
en pruebas.

<code-example path="dependency-injection/src/app/test.component.ts" region="spec" header="src/app/test.component.ts"></code-example>

<div class="alert is-helpful">

Obtén más información en la guía de [Testing](guide/testing).

</div>

{@a service-needs-service}

## Servicios que necesitan otros servicios

Los servicios pueden tener sus propias dependencias. `HeroService` es muy simple y no tiene dependencias propias. Supón, sin embargo, que quieres registrar sus actividades a través de un servicio de logging. Puedes aplicar el mismo patrón *inyección de constructor*,
adicionando un constructor que tome un parámetro `Logger`.

Este es el `HeroService` revisado que inyecta `Logger`, paralelo con la versión previa para su comparación.

<code-tabs>

  <code-pane header="src/app/heroes/hero.service (v2)" path="dependency-injection/src/app/heroes/hero.service.2.ts">
  </code-pane>

  <code-pane header="src/app/heroes/hero.service (v1)" path="dependency-injection/src/app/heroes/hero.service.1.ts">
  </code-pane>

  <code-pane header="src/app/logger.service"
  path="dependency-injection/src/app/logger.service.ts">
  </code-pane>

</code-tabs>

El constructor pide una instancia inyectada de `Logger` y lo almacena en un campo privado llamado `logger`. El método `getHeroes()` registra un mensaje cuando es llamado para proveer los héroes.

Observa que el servicio `Logger` también tiene el decorador `@Injectable()`, aunque puede que no necesite sus propias dependencias. De hecho, el decorador `@Injectable()` es **requerido para todos los servicios**.

Cuando Angular crea una clase donde su constructor tiene parámetros, busca el tipo y los metadatos de inyección de esos parámetros para poder inyectar el servicio correcto.
Si Angular no puede encontrar la información para el parámetro, lanza un error.
Angular solo puede buscar la información del parámetro _si la clase tiene un decorador de algún tipo_.
El decorador `@Injectable()` es el decorador estándar para las clases de servicio. 

<div class="alert is-helpful">

 El requerimiento del decorador es impuesto por TypeScript. TypeScript normalmente descarta la información del tipo del parámetro cuando [transpila](guide/glossary#transpile) el código a JavaScript. TypeScript mantiene esta información si la clase tiene un decorador y la opción `emitDecoratorMetadata` del compilador se establece en `true` en el archivo de configuración `tsconfig.json` de TypeScript. El CLI configura `tsconfig.json` con `emitDecoratorMetadata: true`.

 Esto significa que eres el responsable de poner `@Injectable()` en tus clases de servicio. 

</div>

{@a token}

{@a injection-token}

### Tokens en inyección de dependencias

Cuando configuras un inyector con un proveedor, asocias el proveedor con un [token ID](guide/glossary#di-token).
El inyector mantiene un map interno *proveedor-token* al que referencia cuando
pide una dependencia. El token es la llave del map.

De manera simple, el valor de la dependencia es una *instancia*, y
el *tipo* de la clase sirve como su propia llave de búsqueda.
Aquí obtienes un `HeroService` directamente del inyector proporcionando el tipo de `HeroService` como token:

<code-example path="dependency-injection/src/app/injector.component.ts" region="get-hero-service" header="src/app/injector.component.ts"></code-example>

El comportamiento es similar cuando escribes un constructor que necesita una dependencia basada en una clase inyectada.
Cuando defines un parámetro del constructor con la clase tipo `HeroService`,
Angular sabe inyectar el servicio asociado con el token de la clase `HeroService`:

<code-example path="dependency-injection/src/app/heroes/hero-list.component.ts" region="ctor-signature" header="src/app/heroes/hero-list.component.ts">
</code-example>

Muchos valores de dependencias son proporcionados por clases, pero no todos. El objeto *provide* expandido te permite asociar diferentes tipos de proveedores con tokens DI.

* Obtén más información acerca de los [diferentes tipos de proveedores](guide/dependency-injection-providers).

{@a optional}

### Dependencias opcionales

`HeroService` *requiere* un logger, pero ¿y si pudiera funcionar sin
uno?

Cuando un componente o servicio declara una dependencia, el constructor de la clase toma la dependencia como paramero.
Puedes decirle a Angular que la dependencia es opcional anotando el
parámetro del constructor con `@Optional()`.

<code-example path="dependency-injection/src/app/providers.component.ts" region="import-optional">
</code-example>

<code-example path="dependency-injection/src/app/providers.component.ts" region="provider-10-ctor"></code-example>

Cuando usas `@Optional()`, tu código debe estar preparado para un valor null. Si no
registras un proveedor de logger en ningún lugar, el inyector establece el 
valor de `logger` en null.

<div class="alert is-helpful">

`@Inject()` y `@Optional()` son _decoradores de parametros_. Estos alteran la manera en que el framework de ID proporciona una dependencia, al anotar el parámetro de dependencia en el constructor de la clase que requiere la dependencia.

Obtén más información acerca de los decoradores de parámetros en [Jerarquía de inyectores de dependencias](guide/hierarchical-dependency-injection).

</div>

## Resumen

En esta página has aprendido las bases de inyección de dependencias en Angular.
Puedes registrar varios tipos de proveedores, 
y sabes cómo pedir un objeto inyectado (como un servicio) 
agregando un parámetro al constructor

Profundiza en las capacidades y funcionalidades avanzadas del sistema de DI de Angular en las siguientes páginas:

* Obtén más información acerca de inyectores anidados en
[Jerarquía de inyección de dependencias](guide/hierarchical-dependency-injection).

* Obtén más información acerca de [ID tokens y proveedores](guide/dependency-injection-providers).

* [Inyección de dependencias en acción](guide/dependency-injection-in-action) es una guía de algunas de las cosas más interesantes que puedes hacer con ID.
