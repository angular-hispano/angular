# Actualizando a la versión 11

Esta guía contiene información relacionada para actualizar sus aplicaciones a la versión más reciente de Angular.

## Actualización de aplicaciones usando la CLI

Las instrucciones paso a paso para actualizar sus aplicaciones a la versión más reciente de Angular(aprovechando nuestras herramientas de migración automatizadas), es usando nuestra guía interactiva: [update.angular.io](https://update.angular.io).

## Cambios y obsolescencias en la Versión 11

<div class="alert is-helpful">

   Para más información sobre el proceso de remoción u obsolescencia en Angular, consulte [Prácticas de liberación de Angular](guide/releases#deprecation-practices "Angular Release Practices: Deprecation practices").

</div>

{@a breaking-changes}
### Cambios importantes en la nueva versión

* Se deja de dar soporte a  IE 9, 10, y a IE mobile. Para más detalle, consulte [PR 38931](https://github.com/angular/angular/pull/38931).
* Ya no hay soporte para TypeScript 3.9. Por favor actualice a TypeScript 4.0. Para más detalle, consulte [PR 39313](https://github.com/angular/angular/pull/39313).
* `NavigationExtras#preserveQueryParams` ha sido eliminado de `@angular/router`. Para más detalle, consulte [PR 38762](https://github.com/angular/angular/pull/38762)
* `CollectionChangeRecord` ha sido eliminado de `@angular/core`. Para más detalle consulte [PR 38668](https://github.com/angular/angular/pull/38668).
* Cambiamos el valor predeterminado para `relativeLinkResolution` de `'legacy'` a `'corrected'` de tal modo que las nuevas aplicaciones eligan de forma automática el comportamiento correcto de la navegación de rutas vacías aninadas  [PR 22394](https://github.com/angular/angular/pull/22394). Las aplicaciones que utilizan el valor predeterminado (deafult), se actualizan mediante una migración que especifique "legacy" y así garantice mantener el comportamiento adecuado del _lastPathIndex. Para más detalle, consulte [PR 25609](https://github.com/angular/angular/pull/25609).
* Se corrigió un bug en el enrutador donde los argumentos para las  snapshots `future` y `curr`  se inviertieron en la llamada a `shouldReuseRoute` al procesar rutas secundarias.  Por lo general este error de orden no importa porque la mayoría de las implementaciones de [`shouldReuseRoute`](api/router/RouteReuseStrategy#shouldReuseRoute), solo hacen una comparación de igualdad entre `future` y `curr`. Sin embargo, algunas implementaciones realmente se basan específicamente en el valor de uno de los dos y por tanto, deberá actualizarse. Para más detalle, consulte [PR 26949](https://github.com/angular/angular/pull/26949).
* `ViewEncapsulation.Native` ha sido eliminado. Anteriormente Angular admitía un modo encapsulado de vista `ViewEncapsulaion.Native` que se basaba en la v0 de APIs del Shadow DOM Draft. Estas APIs han sido reemplazadas por las APIs actualizadas del Shadow DOM, y que ahora están habilitadas en el `ViewEncapsulation.ShadowDom`. Para más información sobre este cambio, Consulte [Actualización de Componentes Web: actualice las APIs a la v1 ](https://developers.google.com/web/updates/2019/07/web-components-time-to-upgrade).
* `@angular/platform-webworker` ha sido eliminado, ya no será compatible. Para más detalle, consulte [PR 38846](https://github.com/angular/angular/pull/38846).
* `@angular/platform-webworker` ya no es compatible. No habrá más versiones. Para más detalle, consulte [PR 38846](https://github.com/angular/angular/pull/38846).
* Actualizadas las opciones para `initialNavigation`. Para más información, vea [initialNavigation](api/router/InitialNavigation) en la documentación de la API. Consulte [PR 33128](https://github.com/angular/angular/pull/33128).
* `DatePipe` ya no redondea fracciones de milisegundos. Para más detalle, consulte [PR 38009](https://github.com/angular/angular/pull/38009).
* Los arrays locales, ahora son de solo lectuta. Para más detalle, consulte [PR 30397](https://github.com/angular/angular/pull/30397).
* El `ControlValueAccessor` inyectado para `NG_VALUE_ACCESSOR` ahora es de solo lectura. Para más detalle, consulte [PR 29273](https://github.com/angular/angular/pull/29723).
* El tipo de `AbstractControl#parent` indica ahora que puede ser nulo. Para más detalle, consulte [PR 32671](https://github.com/angular/angular/pull/32671).
* Llamar a  `overrideProvider` antes de inicializar el TestBed, ahora arrojará error. Para más detalle, consulte [PR 38717](https://github.com/angular/angular/pull/38717).
* Muchos de los pipes integrados en Angular se han ampliado o reducido para ser más claros. Para más información, consulte [Pipes](https://angular.io/api?type=pipe) la documentación de la API. Para más detalle, consulte [PR 37447](https://github.com/angular/angular/pull/37447).
* Las directivas del paquete `@angular/forms` solía tener `any[]` como argumentos a los validators y a los asyncValidators en los constructores. Ahora estos argumentos están mejor escritos, por lo que si su código se basa en estos tipos de argumentos en los constructores, ahora se actualizan los tipos para hacerlos coherentes a las directivas. Para más detalle, consulte [PR 38994](https://github.com/angular/angular/pull/38944).
* `routerLink` ahora acepta entradas `undefined`. Para más información, consulte [PR 39151](https://github.com/angular/angular/pull/39151).
* La función `async` de `@angular/core/testing` se renombró a `waitForAsync` para evitar confusiones con la sintaxis nativa `async` de JavaScript. Dicha función se ha vuelto obsoleta y se eliminará en una futura versión. Para más detalle, consulte [PR 37583](https://github.com/angular/angular/pull/37583).

{@a deprecations}
### Obsolescencias

| Area                          | API o Característica                                     | Se elimina en |
| ----------------------------- | -------------------------------------------------- | ----------------- |
| `@angular/core/testing`       | Renombrado `async` a `waitForAsync`                       | <!--v11--> v13 |


{@a removals}
### APIs eliminadas por desuso

Las siguientes APIs se eliminaron a partir de la versión 11.0.0*:

| Paquete          | API            | Reemplazo | Notas |
| ---------------- | -------------- | ----------- | ----- |
| `@angular/router`| `NavigationExtras#preserveQueryParams` | No es necesaria alguna acción | NavigationExtras#preserveQueryParams ha sido eliminado de `@angular/router`.|
| `@angular/core` | `CollectionChangeRecord` | No es necesaria alguna acción | CollectionChangeRecord ha sido eliminado de `@angular/core`.|
| `@angular/core` | `ViewEncapsulation.Native` | No es necesaria alguna acción | Anteriormente Angular admitía un modo encapsulado de vista `ViewEncapsulaion.Native` que se basaba en la v0 de APIs del Shadow DOM Draft. Estas APIs han sido reemplazadas por las APIs actualizadas del Shadow DOM, y que ahora están habilitadas en el `ViewEncapsulation.ShadowDom`. Para más información sobre este cambio, counsulte [Actualización de Componentes Web: actualice las APIs a la v1](https://developers.google.com/web/updates/2019/07/web-components-time-to-upgrade).|

{@a ivy}

## Características compatibles del Ivy

Desde la versíon 9,  Ivy es el motor de renderizado por defecto de Angular. Si no ha escuchado hablar de Ivy, puede leer al respecto en [La guía de Ivy de Angular](guide/ivy).

* Entre otras características, Ivy introduce una verificación de tipo de plantilla más completa. Para más detalle, consulte [Verificación de tipo de plantilla](guide/template-typecheck).

* Para obtener una guía general sobre la depuración y la lista de cambios menores relacionados al Ivy, vea la [Guía de compatibilidad de Ivy](guide/ivy-compatibility).

* Si necesita ayuda para deshabilitar Ivy, consulte las instrucciones [aquí](guide/ivy#opting-out-of-angular-ivy).