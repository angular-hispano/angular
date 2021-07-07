# Expresiones de interpolación y plantilla

La interpolación le permite incorporar cadenas calculadas en el texto entre etiquetas de elementos HTML y dentro de las asignaciones de atributos. Las expresiones de plantilla son lo que tu usas para calcular esas cadenas.

Consulta para toda la sintáxis y  todos los fragmentos de código en esta guía.

## Interpolación `{{...}}`

La interpolación se refiere a incrustar expresiones en texto marcado.
Por defecto, la interpolación usa si delimitador de doble llaves, `{{` and `}}`.

El siguiente fragmento, `{{ currentCustomer }}` es un ejemplo de interpolación.

El texto entre llaves suele ser el nombre de una propiedad de componente. Angular reemplaza ese nombre con el 
valor de cadena de la propiedad del componente correspondiente.

En el ejemplo anterior, angular evalúa ls propiedades `title` y `itemImageUrl` y llena los espacios en blanco, 
primero mostrando el texto del título y luego una imagen.

De manera más general, el texto entre llaves es una **expresión de plantilla** que Angular primero **evalúa** y luego **se convierte en una cadena**.
La siguiente interpolación ilustra el punto agregando dos números:

La expresión puede invocar métodos del componente de host como `getVal ()` en el siguiente ejemplo:

Angular evalúa todas las expresiones entre llaves dobles,
convierte los resultados de la expresión en cadenas y los vincula con cadenas literales vecinas.
Finalmente, asigna este resultado interpolado compuesto a un **elemento o propiedad directiva**.

Parece que está insertando el resultado entre las etiquetas de los elementos y asignándolo a los atributos. 
Sin embargo, la interpolación es una sintaxis especial que Angular convierte en un *enlace de propiedad*.

Si desea utilizar algo que no sea `{{` y `}}`, puedes configurar el delimitador de interpolación
a través de la opción [interpolación](api/core/Component#interpolation) en la opción en los metadatos de `Component`.

## Expresiones de plantilla

Una plantilla **expresión** produce un valor y aparece dentro de las doble llaves, `{{ }}`.
 
Angular ejecuta la expresión y la asigna a una propiedad de un objetivo de enlace; 
el destino podría ser un elemento HTML, un componente o una directiva.

Las llaves de interpolación en `{{1 + 1}}` rodean la expresión de la plantilla `1 + 1`.
En la vinculación de propiedades, 
una expresión de plantilla aparece entre comillas a la derecha del símbolo `=` como en `[propiedad] ="expresión"`

En términos de sintaxis, las expresiones de plantilla son similares a JavaScript.
Muchas expresiones de JavaScript son expresiones de plantilla legales, con algunas excepciones.

No puede usar expresiones de JavaScript que tengan o promuevan efectos secundarios,
incluyendo:

* Asignaciones (`=`, `+ =`, `- =`, `...`)
* Operadores como `new`,` typeof`, `instanceof`, etc.
* Encadenando expresiones con <code>; </code> o <code>, </code>
* Los operadores de incremento y decremento `++` y `--`
* Algunos de los operadores de ES2015+

Otras diferencias notables de la sintaxis de JavaScript incluyen:

* No hay soporte para los operadores bit a bit como `|` y `&`
* Nuevos [operadores de expresión de plantilla] (operadores de expresión de plantilla / guía), como `|`, `? .` y`! `

## Contexto de expresión

El *contexto de expresión* es típicamente la instancia del _componente_.
En los siguientes fragmentos, el `recommendado` entre llaves dobles y el
`itemImageUrl2` en comilla, se refiere a las propiedades de `AppComponent`.

Una expresión también puede referirse a propiedades del contexto de _template_
como una variable de entrada de plantilla,

`let customer`, o una variable de referencia de plantilla,` #customerInput`.

El contexto de los términos de una expresión es una combinación de _variables de plantilla_,
el objeto _context_ de la directiva (si tiene uno) y los _members_ del componente.
Si hace referencia a un nombre que pertenece a más de uno de estos espacios de nombres,
el nombre de la variable de la plantilla tiene prioridad, seguido de un nombre en el _context_ de la directiva,
y, por último, los nombres de los miembros del componente.

El ejemplo anterior presenta tal colisión de nombres. El componente tiene una propiedad `customer` y el `* ngFor` define una variable de plantilla` customer`.

El `customer` en `{{customer.name}}` se refiere a la variable de entrada de la plantilla, no a la propiedad del componente.

Las expresiones de plantilla no pueden hacer referencia a nada en el espacio de nombres global, excepto `undefined`. No pueden referirse a `window` o `document`. Además, ellos no pueden llamar a `console.log ()` o `Math.max ()` y están restringidos a hacer referencia miembros del contexto de expresión.

## Normas de expresión

Cuando use expresiones de plantilla, siga estas pautas:

* [Simplicidad](guide/interpolation#simplicity)
* [Ejecución Rápida](guide/interpolation#quick-execution)
* [Sin efectos secundarios visibles](guide/interpolation#no-visible-side-effects)

### Simplicidad

Aunque es posible escribir expresiones de plantilla complejas, es mejor practica evitarlos.

Un nombre de propiedad o una llamada a un método debería ser la norma, pero una negación booleana ocasional, `!`, está bien.
De lo contrario, limite la aplicación y la lógica de negocios al componente,
donde es más fácil de desarrollar y probar.

### Ejecución rápida

Angular ejecuta expresiones de plantilla después de cada ciclo de detección de cambios.
Los ciclos de detección de cambios se desencadenan por muchas actividades asincrónicas, como
promesas de resoluciones, resultados HTTP, eventos de temporizador, pulsaciones de teclas y movimientos del ratón.

Las expresiones deben terminar rápidamente o la experiencia del usuario puede arrastrarse, especialmente en dispositivos más lentos.
Considere almacenar en caché los valores cuando su cálculo sea costoso.

### Sin efectos secundarios visibles

Una expresión de plantilla no debe cambiar ningún estado de la aplicación que no sea el valor del propiedad de destino.

Esta regla es esencial para la política de "flujo de datos unidireccional" de Angular.
Nunca debe preocuparse de que la lectura del valor de un componente pueda cambiar algún otro valor mostrado.
La vista debe ser estable durante una sola pasada de renderizado.

Una expresión [idempotente](https://en.wikipedia.org/wiki/Idempotence) es ideal porque
está libre de efectos secundarios y mejora el rendimiento de detección de cambios de Angular.
En términos de Angular, una expresión idempotente siempre devuelve
*exactamente lo mismo* hasta que cambie uno de sus valores dependientes.

Los valores dependientes no deben cambiar durante un solo turno del bucle de eventos.
Si una expresión idempotente devuelve una cadena o un número, devuelve la misma cadena o número cuando se llama dos veces seguidas. Si la expresión devuelve un objeto, incluida una "matriz", devuelve el mismo objeto * referencia * cuando se llama dos veces seguidas.

Hay una excepción a este comportamiento que se aplica a `*ngFor`. `*ngFor` tiene la funcionalidad `trackBy` que puede lidiar con la desigualdad referencial de los objetos cuando se itera sobre ellos. Consulte [*ngFor con `trackBy`](guide/built-in-directives#ngfor-with-trackby) para obtener más detalles.
