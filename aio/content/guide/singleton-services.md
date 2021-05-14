# Servicios únicos

Un servicio único es un servicio de una única instancia existente en una aplicación.

Para una aplicación de muestra utilizando un servicio singleton en toda la aplicación que describe esta pagina, mira el <live-example name="ngmodules"></live-example> mostrando todas las funcionalidades documentadas de NgModules.

## Proveyendo un servicio único

Hay dos maneras de crear un servicio único en Angular:

* Colocando la propiedad `providedIn` de el `@Injectable()` a `"root"`.
* Incluyendo el servicio en el `AppModule` ó en un modulo que es sólo importado por el `AppModule`.


{@a providedIn}

### Usando `providedIn`

Iniciando con Angular 6.0, la forma preferida de crear un servicio único es colocar `providedIn` a `root` en el decorar de servicios `@Injectable()`. Esto le indica a Angular a proveer el servicio en la aplicación raíz.

<code-example path="providers/src/app/user.service.0.ts"  header="src/app/user.service.ts"></code-example>

Para información más detallada en servicios, mira el capitulo [Servicios](tutorial/toh-pt4) del [Tutorial Excursión de Héroes](tutorial).

### NgModule array `providers`

En aplicaciones creadas con versiones de Angular previas a 6.0, los servicios son registrados en los arrays `providers` de NgModule, como se muestra a continuación:

```ts
@NgModule({
  ...
  providers: [UserService],
  ...
})

```

Si este NgModule fue el `AppModule` raíz, el `UserService` será único y disponible a traves de toda la aplicación. Aunque puedes ver lo codificado de esta manera, usando la propiedad `providedIn` de el decorador `@Injectable()` en el servicio mismo es preferible a partir de Angular 6.0, ya que hace tus servicios tree-shakable.

{@a forRoot}

## The `forRoot()` pattern

Generalmente, solamente necesitaras `providedIn` para proveer servicios y `forRoot()`/`forChild()` para enrutar. Sin embargo, entendiendo como `forRoot()` trabaja para asegurarse que un servicio es único, informará su desarrollo a un nivel profundo.

Si un modulo define ambos, proveedores y declaraciones (componentes, directivas, pipes), luego cargando el modulo en multiples módulos de funcionalidades, duplicaría el registro del servicio. Esto puede resultar en multiples instancias del servicio y el servicio ya no se comportaría cómo un servicio único.

Hay multiples formas de prevenir esto:

* Utilizando la [sintaxis `providedIn`](guide/singleton-services#providedIn) en vez de registrando el servicio en el modulo.
* Separando tus servicios en su propio modulo.
* Definiendo los métodos `forRoot()` y `forChild()` en el modulo.

<div class="alert is-helpful">

**Nota:** Hay dos aplicaciones de ejemplo donde tú puedes ver este escenario; el más avanzado <live-example noDownload name="ngmodules">ejemplo en vivo NgModules</live-example>, el cual contiene `forRoot()` y `forChild()` en los módulos de enrutamiento y el `GreetingModule`, y en el más simple <live-example name="lazy-loading-ngmodules" noDownload>Ejemplo en vivo Carga Diferida</live-example>. Para una explicación introductoria mira la guiá [Módulos de funcionalidad de Carga Diferida](guide/lazy-loading-ngmodules).

</div>


Utilizando `forRoot()` para separar proveedores de un modulo, puedes importar ese modulo dentro del módulo raíz con `providers` y los módulos hijos sin `providers`.

1. Cree el método estático `forRoot()` en el modulo.
2. Coloque los proveedores dentro del método `forRoot()`.

<code-example path="ngmodules/src/app/greeting/greeting.module.ts" region="for-root" header="src/app/greeting/greeting.module.ts"></code-example>


{@a forRoot-router}

### `forRoot()` and the `Router`

`RouterModule` provee el servicio `Router`, así como las directivas de enrutador, tales como `RouterOutlet` y `routerLink`. El modulo raíz de la aplicación importa `RouterModule`, de modo que la aplicación tenga un `Router` y los componentes de la aplicación raíz puedan acceder al enrutador de directivas. Cualquier modulo de funcionalidades debería también importar `RouterModule`, de modo que sus componentes puedan colocar directivas del enrutador dentro de sus plantillas.

Si el `RouterModule` no tuviera `forRoot()`, cada modulo de funcionalidad instanciaría una nueva instancia de `Router`, lo cual rompería la aplicación, ya que solo puede haber un `Router`.

<div class="alert is-helpful">

**Nota:** Si tienes un modulo el cual tiene proveedores y declaraciones, _puedes_ usar esta técnica para separarlos y podrías ver este patrón en aplicaciones legadas. Sin embargo, desde Angular 6.0, la mejor practica para proveer servicios es con la propiedad `providedIn` del decorador `@Injectable()`.

</div>

### Cómo funciona `forRoot()`

`forRoot()` toma un objeto de configuración de servicio y retorna un
[ModuloConProveedores](api/core/ModuleWithProviders), el cual es un simple objeto con las siguientes propiedades:

* `ngModule`: en este ejemplo, la clase `GreetingModule`
* `providers`: los proveedores configurados

En el <live-example name="ngmodules">ejemplo en vivo</live-example>
la raíz `AppModule` importa el `GreetingModule` y añade los `providers` al `AppModule`. Específicamente, Angular acumula todos los proveedores importados antes de añadir los elementos listados en `@NgModule.providers`. Esta secuencia asegura lo que añades específicamente al proveedores `AppModules` toma precedencia sobre los proveedores de módulos importados.

La aplicación de muestra importa `GreetingModule` y usa su método `foorRoot()` una vez, en `AppModule`. Registrando lo una vez para prevenir multiples instancias.

También puedes agregar al método `forRoot()` en el `GreetingModule` que configura el saludo a `UserService`.

En el siguiente ejemplo, el opcional, `UserServiceConfig` inyectado extiende el saludo de `UserService`. Si un `UserServiceConfig` existe, el `UserService` coloca el nombre del usuario desde esa configuración.

<code-example path="ngmodules/src/app/greeting/user.service.ts" region="ctor" header="src/app/greeting/user.service.ts (constructor)"></code-example>

Aquí es donde `forRoot()` toma al objeto `UserServiceConfig`:

<code-example path="ngmodules/src/app/greeting/greeting.module.ts" region="for-root" header="src/app/greeting/greeting.module.ts (forRoot)"></code-example>

Finalmente, llamalo dentro de la lista `imports` de el `AppModule`. En el siguiente fragmento de código, otras partes del archivo se dejan fuera. Para el archivo completo, mira el  <live-example name="ngmodules"></live-example>, ó continua con la siguiente sección de este documento.

<code-example path="ngmodules/src/app/app.module.ts" region="import-for-root" header="src/app/app.module.ts (imports)"></code-example>

La aplicación muestra "Miss Marple" como el usuario en vez del por defecto "Sherlock Holmes".

Recuerda importar `GreetingModule` como una importación de Javascript al inicio del archivo y no añadirlo a más de una lista de `imports` de `@NgModule`.

## Previniendo reimportar el `GreetingModule`

Solo el `AppModule` raíz debería importar el `GreetingModule`. Si el modulo de carga diferida también lo importa, la aplicación puede generar [multiples instancias](guide/ngmodule-faq#q-why-bad) del servicio.

Para prevenir el reimporte de `GreetingModule` de un modulo de carga diferida, agrega el siguiente constructor `GreetingModule`.

<code-example path="ngmodules/src/app/greeting/greeting.module.ts" region="ctor" header="src/app/greeting/greeting.module.ts"></code-example>

El constructor de indica a Angular inyectar el `GreetingModule` dentro de si mismo.
La inyección debería de ser circular si Angular busca `GreetingModule` en el inyector _actual_, pero el decorador `@SkipSelf()` significa "mira por `GreetingModule` en un inyector ancestro, arriba de mí en la jerarquía de inyección."

Por defecto, el inyector arroja un error cuando no puede encontrar el proveedor solicitado.
El decorador `@Optional()` indica que el no encontrar el servicio es OK.
El inyector regresa `null`, el parámetro `parentModule` es null, y el constructor concluye sin mayor inconveniente.

Es una historia diferente, si de manera incorrecta importa `GreetingModule` dentro de un modulo cargado de forma diferida, como `CustomersModule`.

Angular crea un modulo cargado de forma diferida con su propio inyector, un hijo de el inyector raíz.
`@SkipSelf()` causa que Angular busque a `GreetingModule` en el inyector padre, el cual en este momento es el inyector raíz.
Por supuesto, encuentra la instancia importada por el `AppModule` raíz.
Ahora `parentModule` existe y el constructor arroja el error.

Hay dos archivos enteros para su entera referencia:

<code-tabs>
 <code-pane header="app.module.ts" path="ngmodules/src/app/app.module.ts">
 </code-pane>
 <code-pane header="greeting.module.ts" region="whole-greeting-module" path="ngmodules/src/app/greeting/greeting.module.ts">
 </code-pane>
</code-tabs>

<hr />

## Más sobre NgModules

También podrías estar interesado en:

* [Módulos Compartidos](guide/sharing-ngmodules), el cual trabaja en los conceptos cubiertos en esta pagina.
* [Módulos de Carga Diferida](guide/lazy-loading-ngmodules).
* [NgModule FAQ](guide/ngmodule-faq).
