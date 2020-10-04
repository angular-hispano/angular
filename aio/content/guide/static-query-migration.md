# Guía de migración de consultas estáticas
​
**Nota importante para los autores de bibliotecas: esta migración es especialmente crucial para que los autores de bibliotecas faciliten a sus usuarios la actualización a la versión 9 cuando esté disponible.**

En la versión 9, la configuración predeterminada para las consultas `@ViewChild` y` @ContentChild` está cambiando para corregir el comportamiento defectuoso y sorprendente en las consultas (lea más sobre eso [aquí](#que-significa-esta-bandera)).

En preparación para este cambio, en la versión 8, estamos migrando todas las aplicaciones y bibliotecas para especificar explícitamente la estrategia de resolución para las consultas `@ViewChild` y` @ContentChild`.

Específicamente, esta migración agrega un indicador "estático" explícito que dicta cuándo se deben asignar los resultados de esa consulta.
Agregar esta bandera asegurará que tu código funcione de la misma manera cuando actualice a la versión 9.

Antes:

```
// los resultados de la consulta a veces están disponibles en `ngOnInit`, a veces en `ngAfterViewInit` (basado en la plantilla)
@ViewChild('foo') foo: ElementRef;
```

Después:

```
// resultados de la consulta disponibles en ngOnInit
@ViewChild('foo', {static: true}) foo: ElementRef;

O

// resultados de la consulta disponibles en ngAfterViewInit
@ViewChild('foo', {static: false}) foo: ElementRef;
```
A partir de la versión 9, la marca `estática` será falsa por defecto.
En ese momento, cualquier indicador `{static: false}` se puede eliminar de forma segura, y tendremos un esquema que actualizará tu código.

Nota: esta marca solo se aplica a las consultas `@ViewChild` y `@ContentChild` específicamente, ya que las consultas `@ViewChildren` y `@ContentChildren` no tienen un concepto de estática y dinámica (siempre se resuelven como si fueran "dinámicas").

## FAQ

{@a que-hacer-con-hacer}
### ¿Qué debo hacer si veo un comentario `/ * TODO: add static flag * /` impreso por el esquema?

Si ves este comentario, significa que el esquema no pudo determinar estáticamente el indicador correcto. En este caso, deberás agregar la marca correcta según el comportamiento de tu aplicación.
Para obtener más información sobre cómo elegir, consulte la [siguiente pregunta](#cómo-elegir).

{@a cómo-elegir}
### ¿Cómo elijo qué valor de marca "estático" usar: "verdadero" o "falso"?

En los documentos oficiales de la API, siempre hemos recomendado recuperar los resultados de la consulta en [`ngAfterViewInit` para consultas de vista](api/core/ViewChild#description) y [`ngAfterContentInit` para consultas de contenido](api/core/ContentChild#description).
Esto se debe a que para cuando se ejecutan esos enlaces de ciclo de vida, la detección de cambios se ha completado para los nodos relevantes y podemos garantizar que hemos recopilado todos los resultados de consultas posibles.

La mayoría de las aplicaciones querrán usar `{static: false}` por la misma razón. Esta configuración garantizará que la consulta encuentre coincidencias de consultas que dependen de la resolución de la vinculación (por ejemplo, los resultados dentro de `*ngIf`s o `*ngFor`s).

Hay casos más raros en los que la marca `{static: true}` podría ser necesaria (ver [respuesta aquí](#debería-usar-static-true)).

{@a debería-usar-static-true}
### ¿Hay algún caso en el que deba usar `{static: true}`?

Esta opción se introdujo para admitir la creación de vistas incrustadas sobre la marcha.
Si necesitas acceder a un `TemplateRef` en una consulta para crear una vista dinámicamente, no podrás hacerlo en` ngAfterViewInit`.
La detección de cambios ya se ha ejecutado en esa vista, por lo que la creación de una nueva vista con la plantilla provocará que se genere un error `ExpressionHasChangedAfterChecked`.
En este caso, querrás establecer la marca `static` en` true` y crear su vista en `ngOnInit`.
En la mayoría de los otros casos, la mejor práctica es usar `{static: false}`.

Sin embargo, para facilitar la migración a la versión 8, es posible que también deseas establecer la marca `static` en` true` si el código de su componente ya depende de que los resultados de la consulta estén disponibles algún tiempo **antes de** `ngAfterViewInit` (para ver consultas) o `ngAfterContentInit` (para consultas de contenido).
Por ejemplo, si tu componente se basa en que los resultados de la consulta se completen en el gancho `ngOnInit` o en los definidores de `@Input`, deberás establecer la marca en `true` o volver a trabajar en tu componente para ajustarlo a la sincronización posterior.

Nota: Seleccionar la opción estática significa que los resultados de la consulta anidados en `*ngIf` o `*ngFor` no serán encontrados por la consulta.
Estos resultados solo se pueden recuperar después de ejecutar la detección de cambios.

{@a que-significa-esta-bandera}
### ¿Qué significa esta bandera y por qué es necesaria?

El comportamiento predeterminado de las consultas ha sido históricamente indocumentado y confuso, y también ha dado lugar a problemas que son difíciles de depurar.
En la versión 9, nos gustaría que el comportamiento de las consultas sea más coherente y fácil de entender.

Para explicar por qué, primero es importante comprender cómo han funcionado las consultas hasta ahora.

Sin el indicador `static`, el compilador decidía cuándo se resolvería cada consulta, caso por caso.
Todas las consultas `@ViewChild`/`@ContentChild` se categorizaron en uno de dos depósitos en tiempo de compilación: "estático" o "dinámico".
Esta clasificación determina cuándo los resultados de la consulta estarán disponibles para los usuarios.

- **Las consultas estáticas** eran consultas en las que el resultado se podía determinar de forma estática porque el resultado no dependía de los valores de tiempo de ejecución como los enlaces.
Los resultados de las consultas clasificadas como estáticas estaban disponibles antes de que se ejecutara la detección de cambios para esa vista (accesible en `ngOnInit`).

- **Las consultas dinámicas** eran consultas en las que el resultado NO se podía determinar de forma estática porque el resultado dependía de los valores de tiempo de ejecución (también conocidos como enlaces).
Los resultados de las consultas clasificadas como dinámicas no estaban disponibles hasta que se ejecutó la detección de cambios para esa vista (accesible en `ngAfterContentInit` para consultas de contenido o `ngAfterViewInit` para consultas de vista).

Por ejemplo, digamos que tenemos un componente, `Comp`. En su interior, tenemos esta consulta:

```
@ViewChild(Foo) foo: Foo;
```

y esta plantilla:

```
<div foo></div>
```

Esta consulta `Foo` se categorizaría como estática porque en el tiempo de compilación se sabe que la instancia `Foo` en el `<div>` es el resultado correcto para la consulta.
Debido a que el resultado de la consulta no depende de los valores de tiempo de ejecución, no tenemos que esperar a que se ejecute la detección de cambios en la plantilla antes de resolver la consulta.
En consecuencia, los resultados pueden estar disponibles en `ngOnInit`.

Digamos que la consulta es la misma, pero la plantilla del componente se ve así:

```
<div foo *ngIf="showing"></div>
```

Con esa plantilla, la consulta se categorizaría como una consulta dinámica.
Necesitaríamos saber el valor de tiempo de ejecución de "mostrar" antes de determinar cuáles son los resultados correctos para la consulta.
Como resultado, la detección de cambios debe ejecutarse primero y los resultados solo pueden estar disponibles en `ngAfterViewInit` o en un establecedor para la propiedad de consulta.

El efecto de esta implementación es que agregar un `*ngIf` o `*ngFor` en cualquier lugar por encima de la coincidencia de una consulta puede cambiar cuando los resultados de esa consulta estén disponibles.

Tenga en cuenta que estas categorías solo se aplican específicamente a las consultas `@ViewChild` y `@ContentChild`.
Las consultas `@ViewChildren` y `@ContentChildren` no tenían un concepto de estática y dinámica, por lo que siempre se resolvían como si fueran "dinámicas".

Esta estrategia de resolver consultas en diferentes momentos según la ubicación de posibles coincidencias de consultas ha causado mucha confusión. A saber:

* A veces, los resultados de las consultas están disponibles en `ngOnInit`, pero a veces no lo están y no está claro por qué (consulte [21800](https://github.com/angular/angular/issues/21800) o [19872](https://github.com/angular/angular/issues/19872)).

* Las consultas `@ViewChild` se resuelven en un momento diferente de las consultas de `@ViewChildren`, y las consultas de `@ContentChild` se resuelven en un momento diferente de las consultas de `@ContentChildren`.
Si un usuario convierte una consulta `@ViewChild` en una consulta `@ViewChildren`, su código puede romperse repentinamente porque el tiempo ha cambiado.

* El código que depende del resultado de una consulta puede dejar de funcionar repentinamente tan pronto como se agregue un `*ngIf` o un `*ngFor` a una plantilla.

* Una consulta `@ContentChild` para el mismo componente se resolverá en diferentes momentos del ciclo de vida para cada uso del componente.
Esto conduce a un comportamiento defectuoso en el que el uso de un componente con `*ngIf` se rompe de formas sutiles que no son obvias para el autor del componente.

En la versión 9, planeamos simplificar el comportamiento para que todas las consultas se resuelvan después de que se ejecute la detección de cambios de forma predeterminada.
La ubicación de las coincidencias de consultas en la plantilla no puede afectar cuando el resultado de la consulta estará disponible y romperá repentinamente su código, y el comportamiento predeterminado es siempre el mismo.
Esto hace que la lógica sea más consistente y predecible para los usuarios.

Dicho esto, si una aplicación necesita resultados de la consulta antes (por ejemplo, el resultado de la consulta es necesario para crear una vista incrustada), es posible agregar la marca `{static: true}` para solicitar explícitamente una resolución estática.
Con esta marca, los usuarios pueden indicar que solo se preocupan por los resultados que están disponibles estáticamente y los resultados de la consulta se completarán antes de `ngOnInit`.

{@a ver-niños-y-contenido-niños}
### ¿Este cambio afecta las consultas `@ViewChildren` o `@ContentChildren`?

No, este cambio solo afecta específicamente a las consultas `@ViewChild` y `@ContentChild`.
Las consultas `@ViewChildren` y `@ContentChildren` ya son "dinámicas" de forma predeterminada y no admiten la resolución estática.

{@a por-qué-especificar-static-false}
### ​¿Por qué tengo que especificar "{static: false}"? ¿No es eso lo predeterminado?

El objetivo de esta migración es realizar la transición de aplicaciones que aún no están en la versión 9 a un patrón de consulta que sea compatible con la versión 9.
Sin embargo, la mayoría de las aplicaciones usan bibliotecas, y es probable que algunas de estas bibliotecas aún no se hayan actualizado a la versión 8 (y, por lo tanto, es posible que no tengan los indicadores adecuados).
Dado que la versión de Angular de la aplicación se usará para la compilación, si cambiamos el valor predeterminado, el comportamiento de las consultas en los componentes de la biblioteca cambiará a la versión 8 predeterminada y posiblemente se rompa.
De esta manera, las dependencias de una aplicación se comportarán de la misma manera durante la transición que en la versión anterior.

En la versión 9 de Angular y posteriores, será seguro eliminar cualquier indicador `{static: false}` y haremos esta limpieza por usted en un esquema.

{@a bibliotecas}
### ¿Puedo seguir usando bibliotecas de Angular que aún no se han actualizado a la versión 8?

¡Si, absolutamente!
Debido a que no hemos cambiado el comportamiento de consulta predeterminado en la versión 8 (es decir, el compilador aún elige un tiempo si no se establece ningún indicador), cuando tu aplicación se ejecuta con una biblioteca que no se ha actualizado a la versión 8, la biblioteca se ejecutará de la misma manera hizo en la versión 7.
Esto garantiza que tu aplicación funcionará en la versión 8 incluso si las bibliotecas tardan más en actualizar su código.

{@a actualizar-biblioteca-para-usar-bandera-estática}
### ¿Puedo actualizar mi biblioteca a la versión 8 agregando el indicador `static` para ver consultas, sin dejar de ser compatible con las aplicaciones de la versión 7 de Angular?

Sí, la recomendación del equipo de Angular para las bibliotecas es actualizar a la versión 8 y agregar el indicador `static`. Las aplicaciones de la versión 7 de Angular seguirán funcionando con las bibliotecas que tengan esta marca.

Sin embargo, si actualizas tu biblioteca a la versión 8 de Angular y deseas aprovechar las API de la nueva versión 8, o si deseas dependencias más recientes (como Typecript o RxJS), tu biblioteca será incompatible con las aplicaciones de la versión 7 de Angular. Si tu objetivo es hacer que tu biblioteca sea compatible con las versiones 7 y 8 de Angular, no debes actualizar tu biblioteca en absoluto, excepto por `peerDependencies` en `package.json`.

En general, el plan más eficiente es que las bibliotecas adopten un programa de versiones principales de 6 meses y aumenten la versión principal después de cada actualización de Angular. De esa manera, las bibliotecas permanecen en la misma cadencia de lanzamiento que Angular.
