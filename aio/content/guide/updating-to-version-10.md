# Actualizando a Angular versión 10

Esta guía contiene información relacionada con la actualización a la versión 10 de Angular.

<div class="alert is-helpful">

Para obtener información sobre cómo actualizar a la versión 9 de Angular, ver [Actualizando a Angular versión 9](https://v9.angular.io/guide/updating-to-version-9).

</div>

## Actualizando CLI Apps

Para obtener instrucciones paso a paso sobre cómo actualizar a la última versión de Angular (y aproveche nuestras herramientas de migración automatizadas para hacerlo), use la guía de actualización interactiva en [update.angular.io](https://update.angular.io).

Si tiene curiosidad acerca de las migraciones específicas que ejecuta la CLI, consulte  [sección de migraciones automatizadas](#migrations) para obtener detalles sobre qué código está cambiando y por qué.

## Cambios y Obsoletas en Versión 10

<div class="alert is-helpful">

   Para obtener información sobre las prácticas de depreciación y eliminación de Angular, consulte [Prácticas de liberación de Angular](guide/releases#deprecation-practices "Prácticas de liberación de Angular: Depreciación practicas").

</div>

{@a breaking-changes}
### Nuevos cambios importantes

* Typescript 3.6, 3.7, y 3.8 ya no son compatibles. Por favor actualice a Typescript 3.9.
* Campos de entrada de tipo `number` dispara el `valueChanges` evento  de solo una vez por cambio de valor (en lugar de dos veces en algunos casos). Consulte [PR 36087](https://github.com/angular/angular/pull/36087).
* El `minLength` y `maxLength` los validadores solo validan valores que tienen un valor numérico  `length` propiedad. Consulte [PR 36157](https://github.com/angular/angular/pull/36157).
* Las plantillas con enlaces de propiedad desconocidos o nombres de elementos desconocidos ahora registran errores en lugar de advertencias.Consultar [PR 36399](https://github.com/angular/angular/pull/36399).
* `UrlMatcher` ahora puede regresar `null` valores. Consultar [PR 36402](https://github.com/angular/angular/pull/36402).
* Las vistas trasplantadas ahora se actualizan solo en el punto de inserción. Consultar [PR 35968](https://github.com/angular/angular/pull/35968).
* Formateos de tiempos con el `b` o `B` Los códigos de formato ahora admiten períodos de tiempo que cruzan la medianoche. Consultar [PR 36611](https://github.com/angular/angular/pull/36611).
* La navegación se cancela para rutas con al menos un solucionador vacío. Consultar [PR 24621](https://github.com/angular/angular/pull/24621).

{@a deprecations}
### Nuevas Obsoletas

| Area                          | API o Característica                                                                 | Puede ser eliminado |
| ----------------------------- | ---------------------------------------------------------------------------    | ----------------- |
| `@angular/core`               | [`WrappedValue`](guide/deprecations#wrapped-value)                                     | <!--v10--> v12 |
| navegador soportado               | [`IE 9, 10, and IE Mobile`](guide/deprecations#ie-9-10-and-ie-mobile-support) | <!--v10--> v11 |


{@a removals}
### Nuevas Eliminaciones de APIs Obsoletas

Las siguientes APIs han sido eliminadas a partir de la versión 10.0.0*:

| Paquete          | API            | Sustitución | Notas |
| ---------------- | -------------- | ----------- | ----- |
| `@angular/core`  | Clases base no decoradas que utilizan características de Angular | Añadir el decorador Angular | Consultar [guía de migracion](guide/migration-undecorated-classes) Para más información |
| `@angular/core`  | `ModuleWithProviders` sin un genérico             | `ModuleWithProviders` con un genérico | Consultar [guía de migración](guide/migration-module-with-providers) para más información |
| `@angular/core`  | Saneamiento de estilo | no es necesario hacer nada | Consultar [saneamiento de estilo API retirada](/guide/deprecations#style-sanitization) para más información
| `@angular/bazel` | [`Bazel builder and schematics`](guide/deprecations#bazelbuilder) | `bazelbuild/rules_nodejs` | [Para más información](https://github.com/angular/angular/tree/10.0.x/packages/bazel/src/schematics) |


*Para ver APIs eliminadas en la versión 9, consulte esta guía en el [versión 9 sitio de documentos](https://v9.angular.io/guide/deprecations#removed).

{@a ivy}

## Ivy características y compatibilidad

Desde la versión 9, Angular Ivy es el motor de renderizado por defecto. Si no ha escuchado hablar de Ivy, puede leer más sobre ello en el [Guía Angular Ivy](guide/ivy).

* Entre otras características, Ivy introduce una mayor amplitud tipo-verificación dentro de las plantillas. Para detalles, Consultar [Plantillas Tipo-verificación](guide/template-typecheck).

* Para una orientación general sobre la depuración y una lista de cambios menores asociados a Ivy, consulte en [Ivy guia de compatibilidad](guide/ivy-compatibility).

* Para obtener ayuda sobre la exclusión de Ivy, ver las instrucciones [aquí](guide/ivy#opting-out-of-angular-ivy).

{@a migrations}
## Migraciones Automatizadas para la Versión 10

Lea sobre las migraciones que el CLI maneja para usted automáticamente:

* [Migraciones perdidas `@Directive()`/`@Component()` decoradores](guide/migration-undecorated-classes)
* [Migraciones `ModuleWithProviders`](guide/migration-module-with-providers)
* [Solución-estilos `tsconfig.json` migration](guide/migration-solution-style-tsconfig)
* [`tslib` migración de dependencia directa](guide/migration-update-libraries-tslib)
* [Actualizar `module` y `target` migración de opciones del compilador](guide/migration-update-module-and-target-compiler-options)
