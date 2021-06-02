# Interpolación y expresiones de plantilla

La interpolación te permite incorporar cadenas calculadas en el texto
entre los elementos HTML y dentro de las asignaciones de atributos.
Las expresiones de plantilla es lo que utilizas para calcular esas cadenas.

<div class="alert is-helpful">

Ver el <live-example></live-example> de toda la sintaxis y fragmentos de código utilizados en esta guía

</div>

## Interpolación `{{...}}`

La interpolación se refiere a insertar expresiones en texto de marcado.
Por defecto, la interpolación utiliza como delimitador las llaves dobles, `{{` y `}}`.

El siguiente fragmento, `{{ currentCustomer }}` muestra un ejemplo de interpolación.

<code-example path="interpolation/src/app/app.component.html" region="interpolation-example1" header="src/app/app.component.html"></code-example>

El texto entre llaves comúnmente es el nombre de una propiedad del
componente. Angular reemplaza el nombre con el valor de la cadena
correspondiente a la propiedad.

<code-example path="interpolation/src/app/app.component.html" region="component-property" header="src/app/app.component.html"></code-example>

En el ejemplo de arriba, Angular evalúa las propiedades `title` e `itemImageUrl`
y llena los espacios, primero mostrando el texto del título y luego la imagen.

Generalmente el texto entre llaves es una **expresión de plantilla**
que Angular primero **evalúa** y luego la **convierte a una cadena**.
La siguiente interpolación muestra un ejemplo con la suma de dos números:

<code-example path="interpolation/src/app/app.component.html" region="convert-string" header="src/app/app.component.html"></code-example>

La expresión también puede invocar métodos del componente como `getVal()`
en el siguiente ejemplo:

<code-example path="interpolation/src/app/app.component.html" region="invoke-method" header="src/app/app.component.html"></code-example>

Angular evalúa todas las expresiones que están dentro de las llaves dobles,
convierte el resultado de las expresiones a cadenas y los vincula con cadenas literales cercanas.
Por último, asigna el resultado de la interpolación compuesta a una **propiedad del elemento o de la directiva**.

Parece que estás insertando el resultado entre las etiquetas de los elementos y asignándolos a atributos. Sin embargo,
la interpolación es una sintaxis especial que Angular convierte a una *propiedad enlace* (*property binding*).

<div class="alert is-helpful">

Si prefieres utilizar algo diferente a las `{{` y `}}`, puedes
configurar el delimitador de interpolación en la opción [interpolation](api/core/Component#interpolation)
de los metadatos del `Component`.

</div>

## Expresiones de plantilla
{@a template-expressions}

Una **expresión** de plantilla produce un valor y se muestra dentro de
las llaves dobles, `{{ }}`.
Angular ejecuta la expresión y la asigna a una propiedad de un objetivo de enlace;
el objetivo puede ser un elemento HTML, un componente o una directiva.

Las llaves de interpolación en `{{1 + 1}}` rodean la expresión de platilla `1 + 1`.
En las propiedades de enlace, una expresión de plantilla
aparece entre comillas a la derecha del símbolo `=` como en `[property]="expression"`.

En términos de sintaxis, las expresiones de plantilla son similares a JavaScript.
Muchas expresiones de JavaScript son expresiones de plantilla válidas, con algunas excepciones.

No puedes utilizar expresiones de JavaScript que tengan o promuevan efectos secundarios,
incluyendo:

* Asignaciones (`=`, `+=`, `-=`, `...`)
* Operadores como `new`, `typeof`, `instanceof`, etc.
* Encadenando expresiones con <code>;</code> ó <code>,</code>
* Los operadores de incremento y decremento `++` y `--`
* Algunos de los operadores de ES2015+

Otras diferencias notables de la sintaxis de JavaScript incluyen:

* No hay soporte para los operadores bit a bit como `|` y `&`
<!-- * Nuevos [operadores de expresión de plantilla](guide/template-expression-operators), como `|`, `?.` y `!` -->


## Contexto de la expresión

El *contexto de la expresión* usualmente es la instancia del _componente_.
En los siguientes fragmentos, el `recommended` dentro de las llaves dobles y el
`itemImageUrl2` entre comillas hacen referencia a propiedades del `AppComponent`.

<code-example path="interpolation/src/app/app.component.html" region="component-context" header="src/app/app.component.html"></code-example>

Una expresión también puede hacer referencia a una propiedad del contexto de la _plantilla_
como una variable de plantilla,
<!-- link to built-in-directives#template-input-variables -->
`let customer`, o a una variable de referencia de plantilla, `#customerInput`.
<!-- link to guide/template-ref-variables -->

<code-example path="interpolation/src/app/app.component.html" region="template-input-variable" header="src/app/app.component.html (template input variable)"></code-example>

<code-example path="interpolation/src/app/app.component.html" region="template-reference-variable" header="src/app/app.component.html (template reference variable)"></code-example>

El contexto de los términos en una expresión es una combinación de las _variables de la plantilla_,
el _contexto_ de la directiva (si tiene uno) y los _miembros_ del componente.
Si haces referencia a un nombre que pertenece a más de uno de los conceptos mencionados entonces,
el nombre de la variable de plantilla toma mayor relevancia, seguido del nombre del _contexto_ de la directiva,
y, por último, los nombres de los miembros del componente.

El anterior ejemplo presenta esta colisión de nombres. El componente tiene una propiedad
`customer` y el `*ngFor` define una variable de plantilla `customer`.

<div class="alert is-helpful">

El `customer` en `{{customer.name}}`
se refiere a la variable de plantilla, no a la propiedad del componente.

Las expresiones de plantilla no pueden hacer referencia a nada del
contexto global, a excepción de `undefined`. No pueden hacer referencia
a `window` o `document`. Además, no pueden llamar a `console.log()` o `Math.max()`
y están limitadas a referenciar miembros del contexto de la expresión.

</div>

## Lineamientos para expresiones

Cuando utilices expresiones de plantilla ten en cuenta las siguientes pautas:

* [Simplicidad](guide/interpolation#simplicidad)
* [Rápida ejecución](guide/interpolation#rápida-ejecución)
* [Sin efectos secundarios visibles](guide/interpolation#sin-efectos-secundarios-visibles)

### Simplicidad

Aunque es posible escribir expresiones de plantilla complejas, es una
buena práctica el evitarlas.

Un nombre de propiedad o una llamada a un método debería ser la norma, pero una negación booleana ocasional, `!`, está bien.
De lo contrario, limite la aplicación y la lógica de negocio al componente,
donde es más fácil de desarrollar y probar.

### Rápida ejecución

Angular ejecuta las expresiones de plantilla después de cada ciclo de detección de cambios.
Los ciclos de detección de cambios son desencadenados por muchas actividades asíncronas como
la resolución de promesas, resultados HTTP, eventos temporizados, presión de teclas y movimientos del mouse.

Las expresiones deben terminar rápidamente o la experiencia del usuario puede verse afectada negativamente,
especialmente en dispositivos más lentos.
Considere almacenar en caché los valores cuando su cálculo sea costoso.

### Sin efectos secundarios visibles

Una expresión de plantilla no debe cambiar el estado de la aplicación y limitarse solo al
valor de la propiedad objetivo.

Esta regla es esencial para la política de "flujo de datos unidireccional" de Angular.
Nunca te debe preocupar que la lectura de un valor del componente pueda cambiar otro valor renderizado.
La vista debe ser estable durante todo un pase de renderizado.

Una expresión [idempotente](https://es.wikipedia.org/wiki/Idempotencia) es lo ideal porque
no genera efectos secundarios y mejora el rendimiento de la detección de cambios de Angular.
En términos de Angular, una expresión idempotente siempre regresa
*exactamente el mismo resultado* hasta que algunos de sus valores de los que depende cambie.

Los valores dependientes no deben cambiar durante una vuelta del event loop.
Si una expresión idempotente regresa una cadena o un número, debe regresar la misma cadena o número cuando se llame por segunda ocasión. Si la expresión regresa un objeto, incluyendo un `array`, entonces regresa la misma *referencia* al objeto cuando se llame por segunda ocasión.

<div class="alert is-helpful">

Existe una excepción a este comportamiento presente en el `*ngFor`. `*ngFor` tiene una funcionalidad `trackBy` que puede lidiar
con una desigualdad referencial de los objetos sobre los que se está iterando. Ver [*ngFor con `trackBy`](guide/built-in-directives#ngfor-with-trackby) para más información.

</div>
