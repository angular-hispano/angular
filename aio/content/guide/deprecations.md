# APIs y funcionalidades obsoletas

Angular hace esfuerzos por equilibrar innovación y estabilidad. En ocasiones API’s y funcionalidades se vuelven obsoletas y necesitan ser eliminadas o reemplazadas para que Angular pueda mantenerse actualizado con nuevas mejores prácticas, cambios en las dependencias o cambios en la plataforma (web) en sí.

Para hacer la transición lo más fácil posible, API’s y funcionalidades se vuelven obsoletas por un periodo de tiempo antes de eliminarlas. Esto da tiempo para que actualice sus aplicaciones a las nuevas API’s y a las mejores prácticas más recientes.

Esta guía contiene un resumen de todas las API’s y funcionalidades de Angular que están en desuso actualmente.

<div class="alert is-helpful">

Las API’s y funcionalidades que fueron creadas en la versión 6 o anterior son candidatas para eliminarse en la versión 9 o cualquier otra versión posterior. Para información sobre las deprecaciones de Angular y la eliminación de prácticas, visita [Angular Release Practices](guide/releases#deprecation-practices "Angular Release Practices: Deprecation practices").

Para información acerca de cómo actualizar Angular a la última versión, visita la guía interactiva en [update.angular.io](https://update.angular.io).

</div>
{@a index}
## Indice

Para ayudarte a preparar tus apps para el futuro, la siguiente tabla contiene todas las API’s y funcionalidades obsoletas organizadas por la fecha en que serán eliminadas. Cada elemento está ligado a la sección posterior en esa guía la cual explica la razón de porque se volvieron obsoletos y opciones de reemplazo.

<!--
deprecation -> removal cheat sheet
v4 - v7
v5 - v8
v6 - v9
v7 - v10
v8 - v11
v9 - v12
-->

| Área                          | API o Funcionalidad                                                                           | Probable Eliminación      |
| ----------------------------- | --------------------------------------------------------------------------------------------- | ------------------------- |
| `@angular/bazel`              | [`Bazel builder and schematics`](#bazelbuilder)                                               | v10                       |
| `@angular/common`             | [`ReflectiveInjector`](#reflectiveinjector)                                                   | <!--v8--> v11             |
| `@angular/common`             | [`CurrencyPipe` - `DEFAULT_CURRENCY_CODE`](api/common/CurrencyPipe#currency-code-deprecation) | <!--v9--> v11             |
| `@angular/core`               | [`CollectionChangeRecord`](#core)                                                             | <!--v7--> v11             |
| `@angular/core`               | [`DefaultIterableDiffer`](#core)                                                              | <!--v7--> v11             |
| `@angular/core`               | [`ReflectiveKey`](#core)                                                                      | <!--v8--> v11             |
| `@angular/core`               | [`RenderComponentType`](#core)                                                                | <!--v7--> v11             |
| `@angular/core`               | [`ViewEncapsulation.Native`](#core)                                                           | <!--v6--> v11             |
| `@angular/core`               | [`WrappedValue`](#core)                                                                       | <!--v10--> v12            |
| `@angular/forms`              | [`ngModel` with reactive forms](#ngmodel-reactive)                                            | <!--v6--> v11             |
| `@angular/router`             | [`preserveQueryParams`](#router)                                                              | <!--v7--> v11             |
| `@angular/upgrade`            | [`@angular/upgrade`](#upgrade)                                                                | <!--v8--> v11             |
| `@angular/upgrade`            | [`getAngularLib`](#upgrade-static)                                                            | <!--v8--> v11             |
| `@angular/upgrade`            | [`setAngularLib`](#upgrade-static)                                                            | <!--v8--> v11             |
| `@angular/platform-webworker` | [All entry points](api/platform-webworker)                                                    | <!--v8--> v11             |
| template syntax               | [`<template`>](#template-tag)                                                                 | <!--v7--> v11             |
| polyfills                     | [reflect-metadata](#reflect-metadata)                                                         | <!--v8--> v11             |
| npm package format            | [`esm5` and `fesm5` entry-points in @angular/\* npm packages](guide/deprecations#esm5-fesm5)  | <!-- v9 --> v11           |
| `@angular/core`               | [`defineInjectable`](#core)                                                                   | <!--v8--> v11             |
| `@angular/core`               | [`entryComponents`](api/core/NgModule#entryComponents)                                        | <!--v9--> v11             |
| `@angular/core`               | [`ANALYZE_FOR_ENTRY_COMPONENTS`](api/core/ANALYZE_FOR_ENTRY_COMPONENTS)                       | <!--v9--> v11             |
| `@angular/router`             | [`loadChildren` string syntax](#loadChildren)                                                 | <!--v9--> v11             |
| `@angular/core/testing`       | [`TestBed.get`](#testing)                                                                     | <!--v9--> v12             |
| `@angular/core/testing`       | [`async`](#testing)                                                                           | <!--v9--> v12             |
| `@angular/router`             | [`ActivatedRoute` params and `queryParams` properties](#activatedroute-props)                 | sin especificar           |
| template syntax               | [`/deep/`, `>>>`, and `::ng-deep`](#deep-component-style-selector)                            | <!--v7--> sin especificar |
| browser support               | [`IE 9 and 10, IE mobile`](#ie-9-10-and-mobile)                                               | <!--v10--> v11            |

Para información acerca de Angular CDK y deprecaciones de Angular Material, visita el [historial de cambios](https://github.com/angular/components/blob/master/CHANGELOG.md).

## APIs obsoletas

Esta sección contiene un listado completo de todas las APIs que están actualmente obsoletas, con detalles que le ayudarán en su plan de migración a su reemplazo.

<div class="alert is-helpful">

Tip: En la [Sección de referencia de APIs](api) de este documento, las APIs obsoletas se muestran ~~tachadas.~~ Puedes filtrar la lista de APIs por [**Estado: en desuso**](api?status=deprecated).

</div>

{@a common}

### @angular/common

| API                                                                                           | Remplazo                                            | Anuncio de su desuso | Notas                                                                                                                                          |
| --------------------------------------------------------------------------------------------- | --------------------------------------------------- | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| [`CurrencyPipe` - `DEFAULT_CURRENCY_CODE`](api/common/CurrencyPipe#currency-code-deprecation) | `{provide: DEFAULT_CURRENCY_CODE, useValue: 'USD'}` | v9                   | A partir de v11, el código predeterminado se extraerá de los datos de configuración regional proporcionados por `LOCAL_ID`, en lugar de `USD`. |

{@a core}

### @angular/core

| API                                                                     | Remplazo                                                              | Anuncio de su desuso | Notas                                                                                                                                                                                                     |
| ----------------------------------------------------------------------- | --------------------------------------------------------------------- | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`CollectionChangeRecord`](api/core/CollectionChangeRecord)             | [`IterableChangeRecord`](api/core/IterableChangeRecord)               | v4                   | ninguna                                                                                                                                                                                                   |
| [`DefaultIterableDiffer`](api/core/DefaultIterableDiffer)               | n/a                                                                   | v4                   | No forma parte de la API pública                                                                                                                                                                          |
| [`ReflectiveInjector`](api/core/ReflectiveInjector)                     | [`Injector.create`](api/core/Injector#create)                         | v5                   | Ver [`ReflectiveInjector`](#reflectiveinjector)                                                                                                                                                           |
| [`ReflectiveKey`](api/core/ReflectiveKey)                               | ninguna                                                               | v5                   | ninguna                                                                                                                                                                                                   |
| [`ViewEncapsulation.Native`](api/core/ViewEncapsulation#Native)         | [`ViewEncapsulation.ShadowDom`](api/core/ViewEncapsulation#ShadowDom) | v6                   | Utilice el mecanismo de encapsulación nativo del renderizador. Visita [view.ts](https://github.com/angular/angular/blob/3e992e18ebf51d6036818f26c3d77b52d3ec48eb/packages/core/src/metadata/view.ts#L32). |
| [`defineInjectable`](api/core/defineInjectable)                         | `ɵɵdefineInjectable`                                                  | v8                   | Usado solo en código generado. Ningún código fuente debería depender de esta API.                                                                                                                         |
| [`entryComponents`](api/core/NgModule#entryComponents)                  | ninguna                                                               | v9                   | Ver [`entryComponents`](#entryComponents)                                                                                                                                                                 |
| [`ANALYZE_FOR_ENTRY_COMPONENTS`](api/core/ANALYZE_FOR_ENTRY_COMPONENTS) | ninguna                                                               | v9                   | Visita [`ANALYZE_FOR_ENTRY_COMPONENTS`](#entryComponents)                                                                                                                                                 |
| [`WrappedValue`](api/core/WrappedValue)                                 | ninguna                                                               | v10                  | Visita [removing `WrappedValue`](#wrapped-value)                                                                                                                                                          |

{@a testing}

### @angular/core/testing

| API                                           | Remplazo                                            | Anuncio de su desuso | Notas                                                               |
| --------------------------------------------- | --------------------------------------------------- | -------------------- | ------------------------------------------------------------------- |
| [`TestBed.get`](api/core/testing/TestBed#get) | [`TestBed.inject`](api/core/testing/TestBed#inject) | v9                   | Mismo comportamiento, pero con tipado más seguro.                   |
| [`async`](api/core/testing/async)             | [`waitForAsync`](api/core/testing/waitForAsync)     | v10                  | Mismo comportamiento pero ha sido renombrado para evitar confusión. |

{@a forms}

### @angular/forms

| API                                                | Remplazo                                                 | Anuncio de su desuso | Notas   |
| -------------------------------------------------- | -------------------------------------------------------- | -------------------- | ------- |
| [`ngModel` with reactive forms](#ngmodel-reactive) | [`FormControlDirective`](api/forms/FormControlDirective) | v6                   | ninguna |

{@a router}

### @angular/router

| API                                                                      | Remplazo                                                                 | Anuncio de su desuso | Notas   |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------ | -------------------- | ------- |
| [`preserveQueryParams`](api/router/NavigationExtras#preserveQueryParams) | [`queryParamsHandling`](api/router/NavigationExtras#queryParamsHandling) | v4                   | ninguna |

{@a platform-webworker}

### @angular/platform-webworker

| API                                        | Remplazo | Anuncio de su desuso | Notas                                      |
| ------------------------------------------ | -------- | -------------------- | ------------------------------------------ |
| [All entry points](api/platform-webworker) | ninguna  | v8                   | Ver [platform-webworker](#webworker-apps). |

{@a platform-webworker-dynamic}

### @angular/platform-webworker-dynamic

| API                                                | Remplazo | Anuncio de su desuso | Notas                                      |
| -------------------------------------------------- | -------- | -------------------- | ------------------------------------------ |
| [All entry points](api/platform-webworker-dynamic) | ninguna  | v8                   | Ver [platform-webworker](#webworker-apps). |

{@a upgrade}

### @angular/upgrade

| API                             | Remplazo                                        | Anuncio de su desuso | Notas                                               |
| ------------------------------- | ----------------------------------------------- | -------------------- | --------------------------------------------------- |
| [All entry points](api/upgrade) | [`@angular/upgrade/static`](api/upgrade/static) | v5                   | Ver [Actualización desde AngularJS](guide/upgrade). |

{@a upgrade-static}

### @angular/upgrade/static

| API                                                 | Remplazo                                                      | Anuncio de su desuso | Notas                                               |
| --------------------------------------------------- | ------------------------------------------------------------- | -------------------- | --------------------------------------------------- |
| [`getAngularLib`](api/upgrade/static/getAngularLib) | [`getAngularJSGlobal`](api/upgrade/static/getAngularJSGlobal) | v5                   | Ver [Actualización desde AngularJS](guide/upgrade). |
| [`setAngularLib`](api/upgrade/static/setAngularLib) | [`setAngularJSGlobal`](api/upgrade/static/setAngularJSGlobal) | v5                   | Ver [Actualización desde AngularJS](guide/upgrade). |

{@a deprecated-features}

## Funcionalidades obsoletas

Esta sección contiene un listado de todas las funcionalidades que están en desuso actualmente en la cual se incluyen plantillas de sintaxis, opciones de configuración y otro tipo de deprecaciones no listadas en la sección anterior [APIs Obsoletas](#funcionalidades-obsoletas) . También complementa la sección anterior incluyendo los escenarios de uso o combinaciones de APIs obsoletas.

{@a bazelbuilder}

### Constructor de Bazel y esquemas

El constructor de bazel y esquemas fueron introducidos en Angular Labs para permitir a los usuarios probar Bazel sin tener que manejar versiones de Bazel y archivos BUILD. Esta característica se ha vuelto obsoleta. Para más información, por favor consulta el [documento de migración](https://github.com/angular/angular/blob/master/packages/bazel/src/schematics/README.md).

{@a wtf}

### Web Tracing Framework integration

Angular anteriormente soportaba la integración con [Web Tracing Framework (WTF)](https://google.github.io/tracing-framework/) para pruebas de rendimiento en Aplicaciones Angular. Esta integración dejó de ser mantenida y ya no existe. Como resultado la integración se ha vuelto obsoleta. en la versión de Angular 8 y debido a la falta de evidencia de cualquier uso existente será removida en la versión 9.

{@a deep-component-style-selector}

### `/deep/`, `>>>` and `:ng-deep` component style selectors

El combinado de descendientes shadow-dom-piercing está obsoleto y su soporte está siendo [retirado de la mayoría de los navegadores y herramientas.](https://developers.google.com/web/updates/2017/10/remove-shadow-piercing). Como resultado, en la versión 4 se volvió obsoleto el soporte in Angular para todos los 3 `/deep/`, `>>>` and `::ng-deep`. Hasta su eliminación, es preferido usar `::ng-deep` para mayor compatibilidad con las herramientas.

Para más información, visita [/deep/, >>>, and ::ng-deep](guide/component-styles#deprecated-deep--and-ng-deep "Component Styles guide, Deprecated deep and ngdeep")
en la guía de Estilos de Componentes.

{@a template-tag}

### etiqueta &lt;template&gt;

La etiqueta `<template>` quedó obsoleta en la versión 4 para evitar colisionar con el elemento DOM del mismo nombre (como cuando se usan componentes web). Utiliza `<ng-template>` en su lugar. Para más información visita la guía de [Compilación anticipada](guide/angular-compiler-options#enablelegacytemplate).

{@a ngmodel-reactive}

### ngModel with reactive forms

El soporte para usar la propiedad de entrada `ngModel` y el evento `ngModelChange` con directivas de formulario reactivo ha quedado obsoleto en la versión 6 de Angular y se eliminará en una versión futura de Angular.

Ahora en desuso:

```html
<input [formControl]="control" [(ngModel)]="value" />
```

```ts
this.value = "some value";
```

Esto ha quedado obsoleto por varias razones. Primero, los desarrolladores han encontrado este patrón
confuso. Parece que se está utilizando la directiva `ngModel` real, pero de hecho es
una propiedad de entrada / salida llamada `ngModel` en la directiva de forma reactiva que
se aproxima algo, pero no del todo, al comportamiento de la directiva.
Permite obtener y establecer un valor e interceptar eventos de valor, pero
algunas de las otras funcionalidades de `ngModel`, como
retrasar las actualizaciones con`ngModelOptions` o exportar la directiva, no funcionan.

Además, este patrón combina estrategias de formas reactivas y basadas en plantillas, que
impiden aprovechar todos los beneficios de cualquiera de las estrategias.
Establecer el valor en la plantilla viola los principios de la plantilla agnóstica
que esta detrás de las formas reactivas, y agregar una capa `FormControl`/` FormGroup` en
la clase elimina la conveniencia de definir formularios en la plantilla.

Para actualizar su código antes de que se elimine el soporte, querrá decidir si se queda
con directivas de formas reactivas (y obtener/establecer valores usando patrones de formas reactivas) o
cambiar a directivas basadas en plantillas.

Después (opción 1 - usar formas reactivas):

```html
<input [formControl]="control" />
```

```ts
this.control.setValue("some value");
```

Después (opción 2: usar formularios basados en plantillas):

```html
<input [(ngModel)]="value" />
```

```ts
this.value = "some value";
```

De forma predeterminada, cuando use este patrón, verá una advertencia de su desuso una vez en modo de desarrollo. Puede optar por silenciar esta advertencia proporcionando una configuración en`ReactiveFormsModule` en tiempo de impota:

```ts
imports: [
  ReactiveFormsModule.withConfig({warnOnNgModelWithFormControl: 'never'});
]
```

Alternativamente, puede optar por mostrar una advertencia separada para cada instancia de este
patrón con un valor de configuración `"always"`. Esto puede ayudar a localizar en qué parte del código
el patrón se está utilizando a medida que se actualiza el código.

{@a reflectiveinjector}

### ReflectiveInjector

En la versión 5, Angular remplazó `ReflectiveInjector` por `StaticInjector`. El inyector ya no requiere el polyfill Reflect, lo que reduce el tamaño de la aplicación para la mayoría de los desarrolladores.

Antes:

```
ReflectiveInjector.resolveAndCreate(providers);
```

Después:

```
Injector.create({providers});
```

{@a loadChildren}

### loadChildren string sintaxis

Cuando Angular introdujo por primera vez las rutas diferidas, el navegador no era compatible con la carga dinámica de JavaScript adicional. Angular creó nuestro propio esquema usando la sintaxis `loadChildren: './Lazy/lazy.module#LazyModule'` y construyó herramientas para soportarlo. Ahora que muchos navegadores admiten la importación dinámica de ECMAScript, Angular se esta moviendo hacia esta nueva sintaxis.

En la versión 8, la sintaxis de string para la especificación de ruta [`loadChildren`](api/router/LoadChildren) quedó obsoleta, a favor de una nueva sintaxis que usa la sintaxis `import()`.

Antes:

```
const routes: Routes = [{
  path: 'lazy',
  // La siguiente sintaxis de string para loadChildren está obsoleta
  loadChildren: './lazy/lazy.module#LazyModule'
}];
```

Después:

```
const routes: Routes = [{
  path: 'lazy',
  // La nueva sintaxis import()
  loadChildren: () => import('./lazy/lazy.module').then(m => m.LazyModule)
}];
```

<div class="alert is-helpful">

**Actualización a la versión 8**: cuando actualiza a la versión 8, el comando [`ng update`](cli/update) realiza la transformación automáticamente. Antes de la versión 7, la sintaxis `import ()` solo funcionaba en modo JIT (con motor de visualización).

</div>

<div class="alert is-helpful">

**Sintaxis de declaración**: Es importante seguir la sintaxis de declaración de ruta `loadChildren: () => import ('...'). Luego (m => m.ModuleName)` para permitir que `ngc` descubra el módulo de carga diferida y el` NgModule` asociado. Puede encontrar la lista completa de construcciones de sintaxis permitidas [aqui](https://github.com/angular/angular-cli/blob/a491b09800b493fe01301387fa9a025f7c7d4808/packages/ngtools/webpack/src/transformers/import_factory.ts#L104-L113). Estas restricciones se hacen menos con el lanzamiento de Ivy, ya que ya no usará `NgFactories`.

</div>

{@a activatedroute-props}

### Parametros de ActivatedRoute y propiedades de queryParams

[ActivatedRoute](api/router/ActivatedRoute) contiene dos [propiedades](api/router/ActivatedRoute#properties) que son menos capaces que sus reemplazos y pueden quedar obsoletos en una futura versión de Angular.

| Propiedad     | Remplazo        |
| ------------- | --------------- |
| `params`      | `paramMap`      |
| `queryParams` | `queryParamMap` |

Para mayor información visita [Obtener información de ruta](guide/router#activated-route) sección de [Guía del Enrutador](guide/router).

{@a reflect-metadata}

### Dependencia en una polyfill reflect-metadata en modo JIT

Las aplicaciones de Angular, y específicamente las aplicaciones que se basaban en el compilador JIT, solían requerir un polyfill para las APIs [reflect-metadata](https://github.com/rbuckton/reflect-metadata).

La necesidad de este polyfill fue removida en la versión de Angular 8.0 ([ver #14473](https://github.com/angular/angular-cli/pull/14473)), haciendo innecesaria la presencia de poylfill en la mayoría de las aplicaciones de Angular. Debido a que las bibliotecas de terceros pueden depender del polyfill, en lugar de eliminarlo de todos los proyectos de Angular, estamos desaprobando el requisito de este polyfill a partir de la versión 8.0. Esto debería dar a los autores de bibliotecas y desarrolladores de aplicaciones tiempo suficiente para evaluar si necesitan el polyfill y realizar cualquier refactorización necesaria para eliminar su dependencia.
En un proyecto tipico de Angular, el polyfill no se usa en compilaciones de producción, por lo que eliminarlo no debería afectar las aplicaciones de producción. El objetivo detrás de esta eliminación es la simplificación general de la configuración de la compilación y la disminución del número de dependencias externas.

{@a static-query-resolution}

### `@ViewChild()` / `@ContentChild()` resolución estática por defecto

Visita la [guía de migración dedicada para queries estaticas](guide/static-query-migration).

{@a contentchild-input-together}

### `@ContentChild()` / `@Input()` usado en conjunto.

El siguiente patron ha quedado en desuso:

```ts
@Input() @ContentChild(TemplateRef) tpl !: TemplateRef<any>;
```

En lugar de utilizar este patrón, separe a los dos decoradores con sus propias propiedades y agregue lógica de respaldo como en el siguiente ejemplo:

```ts
@Input() tpl !: TemplateRef<any>;
@ContentChild(TemplateRef) inlineTemplate !: TemplateRef<any>;
```

{@a cant-assign-template-vars}

### No se puede asignar a variables de plantilla

En el siguiente ejemplo, el enlace bidireccional significa que `optionName`
debe escribirse cuando se dispara el evento `valueChange`.

```html
<option *ngFor="let optionName of options" [(value)]="optionName"></option>
```

Sin embargo, en la práctica, Angular simplemente ignora los enlaces bidireccionales a las variables de plantilla. A partir de la versión 8, el intento de escribir en variables de plantilla ha quedado obsoleto. En una versión posterior, se lanzará una alerta para indicar que la escritura no es compatible.

```html
<option *ngFor="let optionName of options" [value]="optionName"></option>
```

{@a binding-to-innertext}

### Enlazando `innerText` a `platform-server`

El [Domino](https://github.com/fgnass/domino), que se usa en la representación del lado del servidor, no admite `innerText`, por lo que en el "adaptador domino" de la plataforma-servidor, había un código especial para volver a `textContent` si se intentaba enlazar a `innerText`.

Estas dos propiedades tienen diferencias sutiles, por lo que cambiar a `textContent` bruscamente puede resultar sorprendente para los usuarios. Por esa razón, ese comportamiento está quedando en desuso. En el futuro, los usuarios deben vincularse explícitamente a `textContent` cuando utilicen Domino.

{@a wtf-apis}

### `wtfStartTimeRange` y todas las APIs `wtf*`

Todo acerca de las APIs `wtf*` ha quedado obsoleto y será eliminado en la versión posterior.

{@a webworker-apps}

### Ejecución de aplicaciones de Angular con platform-webworker

Los paquetes `@angular/platform-*` permiten que Angular se ejecute en diferentes contextos. Por ejemplo,
`@angular/platform-server` permite que Angular se ejecute en el servidor y `@angular/platform-browser`
permite que Angular se ejecute en un navegador web.

`@angular/platform-webworker` se introdujo en la versión 2 de Angular como un experimento del aprovechamiento de la Arquitectura de renderizado de Angular para ejecutar una aplicación web completa en un [web worker](https://developer.mozilla.org/es/docs/Web/API/Web_Workers_API). Hemos aprendido bastante
de este experimento y hemos llegado a la conclusión de que ejecutar toda la aplicación en un web
broker no es la mejor estrategia para la mayoría de las aplicaciones.

En el futuro, centraremos nuestros esfuerzos relacionados con los web brokers en torno a su caso de uso principal de aminorar el trabajo intensivo y no crítico de la CPU necesario para la representación inicial (como la búsqueda en memoria y procesamiento de imágenes). Obtenga más información en la [guía para usar Web Brokers con Angular CLI](guide/web-worker).

A partir de la versión 8 de Angular, todas las APIs de `platform-webworker` son obsoletas.
Esto incluye ambos paquetes: `@angular/platform-webworker` y
`@angular/platform-webworker-dynamic`.

{@a entryComponents}

### `entryComponents` and `ANALYZE_FOR_ENTRY_COMPONENTS` ya no son requeridos

Anteriormente, el array `entryComponents` en la definición de` NgModule` se usaba para decirle al compilador qué componentes se crearían e insertarían dinámicamente. Con Ivy, esto ya no es un requisito y el array `entryComponents` se puede eliminar de las declaraciones de módulos existentes. Lo mismo se aplica al token de inyección `ANALYZE_FOR_ENTRY_COMPONENTS`.

{@a modulewithproviders-type-without-a-generic}

### `ModuleWithProviders` tipo sin generico

Algunas bibliotecas de Angular, como `@angular/router` y `@ngrx/store`, implementan una API que devuelve un tipo llamado `ModuleWithProviders` (normalmente a través de un método llamado `forRoot()`).
Este tipo representa un `NgModule` junto con proveedores adicionales.
La versión 9 de Angular ha dejado en desuso `ModuleWithProviders` sin un tipo explícitamente genérico, donde el tipo genérico se refiere al tipo de `NgModule`.
En una versión futura de Angular, el genérico ya no será opcional.
Más sobre el texto fuente
Si está utilizando el CLI, "ng update" debería [migrar el código automáticamente](guide/migration-module-with-providers).
Si no está utilizando el CLI, puede agregar los tipos genéricos que faltan a su aplicación manualmente.
Por ejemplo:

**Antes**

```ts
@NgModule({...})
export class MiModulo {
  static forRoot(config: AlgunaConfig): ModuleWithProviders {
    return {
      ngModule: AlgunModulo,
      providers: [
        {provide: AlgunaConfig, useValue: config}
      ]
    };
  }
}
```

**Después**

```ts
@NgModule({...})
export class MiModulo {
  static forRoot(config: AlgunaConfig): ModuleWithProviders<AlgunModulo> {
    return {
      ngModule: AlgunModulo,
      providers: [
        {provide: AlgunaConfig, useValue: config }
      ]
    };
  }
}
```

{@a ie-9-10-and-mobile}

### IE 9, 10, and IE mobile support

La compatibilidad con IE 9 y 10 ha quedado obsoleta, así como la compatibilidad con IE Mobile. Estas se eliminarán en unasupport versión futura.
La compatibilidad con navegadores obsoletos como estos aumenta el tamaño del paquete, la complejidad del código y la carga de prueba, y también requiere tiempo y esfuerzo que podrían dedicarse a mejorar el marco.
Por ejemplo, solucionar problemas puede ser más difícil, ya que una solución sencilla para los navegadores modernos podría romper los antiguos que tienen peculiaridades debido a que no reciben actualizaciones de los proveedores.

La decisión final se tomó sobre tres puntos clave:

- **Soporte de proveedores**: Microsoft eliminó el soporte de IE 9 y 10 el 1/12/16, lo que significa que ya no brindan actualizaciones de seguridad ni soporte técnico. Además, Microsoft abandonó el soporte para Windows 10 Mobile en diciembre de 2019.
- **Estadísticas de uso**: Analizamos las tendencias de uso de IE 9 y 10 (así como de IE Mobile) de varias fuentes y todos indicaron que los porcentajes de uso eran extremadamente pequeños (fracciones acerca del 1%).
- **Retroalimentación de los socios**: También nos comunicamos con algunos de nuestros clientes de Angular y ninguno expresó su preocupación por eliminar el soporte de IE 9, 10 o IE Mobile.

{@a wrapped-value}

### `WrappedValue`

El propósito de "WrappedValue" es permitir, a la misma instancia del objeto ser tratada como diferente para efectos en la detección de cambios.
Se usa comúnmente con el pipe `async` en el caso donde el `Observable` produce la misma instancia del valor.

Dado que este caso de uso es relativamente raro y el manejo especial afecta el rendimiento de la aplicación, se ha vuelto obsoleto en la versión 10.
No se prevé ningún reemplazo para este caso.

Si confía en el comportamiento de que la misma instancia del objeto debería causar la detección de cambios, existen dos opciones:

- Clonar el valor resultante para que tenga una nueva identidad.
- Llamar explícitamente a [`ChangeDetectorRef.detectChanges()`](api/core/ChangeDetectorRef#detectchanges) para forzar la actualización.

{@a deprecated-cli-flags}

## APIs del CLI y Opciones Obsoletas

Esta sección contiene una lista completa de todos flags del CLI actualmente en desuso.

### @angular-devkit/build-angular

| Opción/API                  | Probable Eliminación | Notas                                                                                      |
| --------------------------- | -------------------- | ------------------------------------------------------------------------------------------ |
| `i18nFile`                  | <!--v9--> v11        | Se especifica en la configuración regional del proyecto en la versión 9 y posteriores.     |
| `i18nFormat`                | <!--v9--> v11        | El formato ahora se detecta automáticamente.                                               |
| `i18nLocale`                | <!--v9--> v11        | Nueva [opción de localización](/guide/i18n#localize-config) en la versión 9 y posteriores. |
| `lazyModules`               | <!--v9--> v11        | Utilizado con SystemJsNgModuleLoader en desuso.                                            |
| `rebaseRootRelativeCssUrls` | <!--v8--> v11        | Destinado solo para ayudar con problemas de migración específicos.                         |
| `scripts[].lazy`            | <!--v8--> v11        | Renombrado a `scripts[].inject`.                                                           |
| `styles[].lazy`             | <!--v8--> v11        | Renombrado a `styles[].inject`.                                                            |
| `i18nFormat`                | <!--v9--> v11        | Renombrado a `format` para simplificar la experiencia de usuario.                          |
| `i18nLocale`                | <!--v9--> v11        | Redundante con la configuración regional de origen del proyecto.                           |
| `scripts[].lazy`            | <!--v8--> v11        | Renombrado a `scripts[].inject`.                                                           |
| `styles[].lazy`             | <!--v8--> v11        | Renombrado a `styles[].inject`.                                                            |
| `i18nFile`                  | <!--v9--> v11        | Se especifica en la configuración regional del proyecto en la versión 9 y posteriores.     |
| `i18nFormat`                | <!--v9--> v11        | El formato ahora se detecta automáticamente.                                               |
| `i18nLocale`                | <!--v9--> v11        | Nueva [opción de localización](/guide/i18n#localize-config) en la versión 9 y posteriores  |
| `lazyModules`               | <!--v9--> v11        | Utilizado con SystemJsNgModuleLoader en desuso.                                            |

### @angular-devkit/core

| API/Option                | Probable Eliminación | Notas                                                                                                                                                                                                                                                         |
| ------------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ModuleNotFoundException` | <!--v8--> v10        | No se utiliza en proyectos. Se utiliza solo con las APIs de herramientas. No es compatible con Yarn PnP y no se utiliza en el CLI de Angular. Utilice Node.js [require.resolve](https://nodejs.org/api/modules.html#modules_require_resolve_request_options). |
| `resolve`                 | <!--v8--> v10        | No se utiliza en proyectos. Se utiliza solo con las APIs de herramientas. No es compatible con Yarn PnP y no se utiliza en el CLI de Angular. Utilice Node.js [require.resolve](https://nodejs.org/api/modules.html#modules_require_resolve_request_options). |
| `setResolveHook`          | <!--v8--> v10        | No se utiliza en proyectos. Se utiliza solo con las APIs de herramientas. No es compatible con Yarn PnP y no se utiliza en el CLI de Angular. [require.resolve](https://nodejs.org/api/modules.html#modules_require_resolve_request_options).                 |
| `ResolveOptions`          | <!--v8--> v10        | No se utiliza en proyectos. Se utiliza solo con las APIs de herramientas. No es compatible con Yarn PnP y no se utiliza en el CLI de Angular. [require.resolve](https://nodejs.org/api/modules.html#modules_require_resolve_request_options).                 |
| `terminal`                | <!--v8--> v10        | Implementación no utilizada de códigos de terminal (color).                                                                                                                                                                                                   |
| `isObservable`            | <!--v8--> v10        | No se utiliza en proyectos. Se utiliza solo con la API de herramientas. Utilice la función `isObservable` del paquete` rxjs`.                                                                                                                                 |

### @ngtools/webpack

| [](https://dle.rae.es/opci%C3%B3n)

| Opción/API                      | Probable Eliminación | Notas                                           |
| ------------------------------- | -------------------- | ----------------------------------------------- |
| `discoverLazyRoutes`            | <!--v9--> TBD        | Utilizado con SystemJsNgModuleLoader en desuso. |
| `additionalLazyModules`         | <!--v9--> TBD        | Utilizado con SystemJsNgModuleLoader en desuso. |
| `additionalLazyModuleResources` | <!--v9--> TBD        | Utilizado con SystemJsNgModuleLoader en desuso. |

### @schematics/angular

| O                | Probablemente removido en | Notas                   |
| ---------------- | ------------------------- | ----------------------- |
| `entryComponent` | <!--v9--> TBD             | Ivy ya no es requerido. |

{@a removed}

## APIs Eliminadas

Las siguientes API se han eliminado a partir de la versión 10.0.0 \*:

| Paquete         | API                                                          | Reemplazo                               | Notas                                                                                              |
| --------------- | ------------------------------------------------------------ | --------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `@angular/core` | Clases base no decoradas que usan funcionalidades de Angular | Agregar decorador Angular               | Ver [guía de migración](guide/migration-undecorated-classes) para más info.                        |
| `@angular/core` | `ModuleWithProviders` sin un generico                        | `ModuleWithProviders` con un generico   | Ver [guía de migración](guide/migration-module-with-providers) para más info.                      |
| `@angular/core` | Sanitización de estilo                                       | No es necesario realizar ninguna acción | Ver [eliminación de la API de sanitización de estilo](#style-sanitization) para mayor información. |

\*Para ver las API eliminadas en la versión 9, consulte esta guía en la [documentación de la versión 9](https://v9.angular.io/guide/deprecations#removed).

{@a esm5-fesm5}

### Formatos de código `esm5` y `fesm5` en paquetes @angular/\*npm

A partir de Angular v8, el CLI consume principalmente la variante `fesm2015` del código distribuido a través de paquetes npm`@angular/*`.
Esto hace que las distribuciones `esm5` y` fesm5` sean obsoletas e innecesarias, lo que aumenta el tamaño del paquete y ralentiza las instalaciones de npm.

Esta eliminación no tiene ningún impacto en los usuarios del CLI, a menos que hayan modificado su configuración de compilación para consumir explícitamente estas distribuciones de código.

Cualquier aplicación que todavía dependa de `esm5` y` fesm5` como entrada a su sistema de compilación deberá asegurarse de que el pipeline de compilación sea capaz de aceptar código JavaScript conforme a la especificación del lenguaje ECMAScript 2015 (ES2015).

Tenga en cuenta que este cambio no hace que las bibliotecas existentes distribuidas en este formato sean incompatibles con el CLI de Angular.
El CLI recurrirá y consumirá bibliotecas en formatos menos deseables si no hay otros disponibles.
Sin embargo, recomendamos que las bibliotecas envíen su código en formato ES2015 para que las compilaciones sean más rápidas y el resultado de la compilación sea más pequeño.

En términos prácticos, el `package.json` de todos los paquetes `@angular` han cambiado de la siguiente manera:

**Antes**:

```
{
  "name": "@angular/core",
  "version": "9.0.0",
  "main": "./bundles/core.umd.js",
  "module": "./fesm5/core.js",
  "es2015": "./fesm2015/core.js",
  "esm5": "./esm5/core.js",
  "esm2015": "./esm2015/core.js",
  "fesm5": "./fesm5/core.js",
  "fesm2015": "./fesm2015/core.js",
  ...
}
```

**Después**:

```
{
  "name": "@angular/core",
  "version": "10.0.0",
  "main": "./bundles/core.umd.js",
  "module": "./fesm2015/core.js",
  "es2015": "./fesm2015/core.js",
  "esm2015": "./esm2015/core.js",
  "fesm2015": "./fesm2015/core.js",
  ...
}
```

Para obtener más información sobre el formato de paquete npm, consulte la [Especificación de formato de paquete Angular](https://goo.gl/jB3GVv).

{@a removed}

## APIs Eliminadas

Las siguientes APIs han sido eliminadas a partir de la versión 10.0.0 \*:

| Paquete         | API                                                          | Reemplazo                             | Notas                                                                         |
| --------------- | ------------------------------------------------------------ | ------------------------------------- | ----------------------------------------------------------------------------- |
| `@angular/core` | Clases base no decoradas que usan funcionalidades de Angular | Agregar decorador Angular             | Ver [guía de migración](guide/migration-undecorated-classes) para más info.   |
| `@angular/core` | `ModuleWithProviders` sin un generico                        | `ModuleWithProviders` con un generico | Ver [guía de migración](guide/migration-module-with-providers) para más info. |

- Para ver las API eliminadas en la versión 9, consulte esta guía en la [documentación de la versión 9](https://v9.angular.io/guide/deprecations#removed).

{@a style-sanitization}

### Sanitizacion de estilo los enlaces de `[style]` y `[style.prop]`

Angular solía sanitizar los enlaces de `[style]` y `[style.prop]` para evitar que se inserte código malicioso a través de las expresiones `javascript:` en las entradas `url ()` de CSS. Sin embargo, la mayoría de los navegadores modernos ya no admiten el uso de estas expresiones, por lo que la sanitización solo se mantuvo por el bien de IE 6 y 7. Dado que Angular no admite IE 6 o 7 y la sanitización tiene un costo de rendimiento, ya no sanitize los enlaces de estilo a partir de la versión 10 de Angular.
