
# Migración de indicadores dinámicos de consultas

## ¿Qué hace esta migración?

En la versión 8 de Angular, un esquema agregó el indicador `static` a todas las consultas `@ViewChild()` y `@ContentChild()`.
Este fue el primer paso para cambiar el comportamiento por defecto.
En la versión 9, el valor por defecto cambió a `static: false` y el indicador se volvió opcional.

Este esquema escanea las clases en compilación y por cada clase revisa si los miembros tienen una consulta `@ViewChild()` o `@ContentChild()` con el indicador `static` fijado en `false`
Si es así, el esquema remueve el indicador para que tome el valor por defecto.

**Antes:**
```ts
@ViewChild('foo', {static: false}) foo: ElementRef;

@ViewChild('bar', {static: true}) bar: ElementRef;
```


**Después:**
```ts
@ViewChild('foo') foo: ElementRef;

// esta consulta no cambia porque el valor de static es true
@ViewChild('bar', {static: true}) bar: ElementRef;
```

Note que el indicador no está soportado en las consultas `@ViewChildren()` o `@ContentChildren()`, entonces el esquema no revisará esas propiedades.


## ¿Por qué es necesaria esta migración?

Este esquema realiza una limpieza de código al remover el indicador `static` que coincide con el predeterminado, ya que no son necesarios.
Funcionalmente, el cambio de código debería afectar su comportamiento.

Antes de la versión 9, Angular descubrió la naturaleza estática o dinámica de una consulta automáticamente, basada en como fue escrita la plantilla.
Sin embargo, mirar las plantillas de esta manera, provocó un comportamiento defecuoso sorprendente (ver más sobre esto en [Guía para la migración de consultas estáticas](guide/static-query-migration#what-does-this-flag-mean)).
A partir de la versión 9, Angular usa consultas dinámicas (`static: false`) por defecto, lo cuál simplifica las consultas.
Pero los desarrolladores aún pueden fijar una consulta como `static: true` si es necesario.


<div class=" alert is-helpful">

### ¿Cuál es la diferencia entre consultas estáticas y dinámicas?

La opción `static` para las consultas `@ViewChild()` y `@ContentChild()` determina cuando los resultados de la consulta están disponibles.

Con las consultas estáticas (`static: true`), la consulta se resuelve una vez la vista ha sido creada, pero antes de que se ejecute la detección de cambios.
Sin embargo, el resultado nunca se actualizará para reflejar los cambios en tu vista, por ejemplo para cambios en bloques `ngIf` y `ngFor`.

Con las consultas dinámicas (`static: false`), la consulta se resuelve después de `ngAfterViewInit()` o `ngAfterContentInit()` para `@ViewChild()` y `@ContentChild()` respectivamente.
El resultado estará actualizado para los cambios en tu vista, por ejemplo para cambios en bloques `ngIf` y `ngFor`.

Para más información, mira las siguientes entradas en
[Guía para la migración de consultas estáticas](guide/static-query-migration):

* [¿Cómo elegir qué valor de indicador `static` usar: `true` o `false`?](guide/static-query-migration#how-do-i-choose-which-static-flag-value-to-use-true-or-false)

* [¿Hay un caso donde debería usar `{static: true}`?](guide/static-query-migration#is-there-a-case-where-i-should-use-static-true)

</div>


## ¿Qué significa esto para las librerías?

Para apoyar las aplicaciones que aún están funcionando con la versión 8, la opción más segura para las librerías es conservar el indicador `static` para mantener consistencia en el tiempo de resolución.

- *Librerías en la versión 9 con aplicaciones funcionando en la versión 8: *

  El esquema no se ejecutará en librerías.
  Mientras las librerías conserven sus indicadores `static` de la versión 8, podrán funcionar con aplicaciones en la versión 8.

- *Librerías en la versión 8 con aplicaciones funcionando en la versión 9: *

  Las librerías tendrán que definir explícitamente los indicadores.
  El comportamiento con los indicadores explícitos no ha cambiado.


### ¿Qué pasa con las aplicaciones que están usando librerías que no han migrado?

Como esta es una limpieza de código no afecta el comportamiento, las librerías que no han migrado funcionarán igual de cualquier forma.
