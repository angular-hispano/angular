<!-- {@a expression-operators} -->

# Operadores de expresión de plantilla

El lenguaje de expresión de plantillas de Angular emplea un subconjunto de sintaxis JavaScript complementado con algunos operadores especiales para escenarios específicos. La siguiente sección cubre tres de esos operadores:


- [pipe](guide/template-expression-operators#pipe)
- [operador de navegación segura](guide/template-expression-operators#safe-navigation-operator)
- [operador de confirmación de no nulo](guide/template-expression-operators#non-null-assertion-operator)

<div class="alert is-helpful">

Mirar <live-example></live-example> para ver un ejemplo funcional que contiene los fragmentos de código de esta guía.

</div>

{@a pipe}

## El operador pipe (`|`)

El resultado de una expresión puede requerir alguna transformación antes de que esté listo para ser usado en la vista o en un enlace.
Los pipes son funciones simples que aceptan un valor de entrada y retornan ese valor transformado.

Por ejemplo, puedes mostrar un número como moneda, cambiar el texto a mayúsculas o filtrar una lista y ordenarla.
Son fáciles de aplicar dentro de plantillas de expresión, esto se hace usando el operador pipe (`|`):

<code-example path="template-expression-operators/src/app/app.component.html" region="uppercase-pipe" header="src/app/app.component.html"></code-example>

El operador pipe pasa el resultado de una expresión a la izquierda a una función pipe a la derecha. (Expresión izquierda | función derecha)

Puedes encadenar expresiones a través de múltiples pipes:

<code-example path="template-expression-operators/src/app/app.component.html" region="pipe-chain" header="src/app/app.component.html"></code-example>

Y también puedes [aplicar parámetros](guide/pipes#parameterizing-a-pipe) a los pipes:

<code-example path="template-expression-operators/src/app/app.component.html" region="date-pipe" header="src/app/app.component.html"></code-example>

El pipe `json` es particularmente útil para depurar enlaces de interpolación:

<code-example path="template-expression-operators/src/app/app.component.html" region="json-pipe" header="src/app/app.component.html"></code-example>

La salida generada se vería así:

<code-example language="json">
  { "name": "Telephone",
    "manufactureDate": "1980-02-25T05:00:00.000Z",
    "price": 98 }
</code-example>

<div class="alert is-helpful">

El operador pipe tiene una precedencia mas alta que el operador ternario (`?:`),
lo que significa `a ? b : c | x` es transformado a `a ? b : (c | x)`.
Por esta y otras razones,
el operador pipe no se puede utilizar sin paréntesis en el primer y segundo operando de `?:`.
Es una buena práctica utilizar paréntesis también en el tercer operando.

</div>

<hr/>

{@a safe-navigation-operator}

## El operador de navegación segura ( `?` ) y rutas de propiedad nulas

El operador de navegación segura de Angular, `?`, protege contra valores `null` y `undefined`
en las rutas de propiedad nula. Aquí, protege contra una falla en el renderizado de la vista si `item` es `null`.

<code-example path="template-expression-operators/src/app/app.component.html" region="safe" header="src/app/app.component.html"></code-example>

Si `item` es `null`, la vista todavía puede renderizar pero el valor que se muestra es blanco; veras unicamente "El nombre del item es:" sin nada después.

Considera el siguiente ejemplo, con `nullItem`.

<code-example language="html">
  El nombre del item es: {{nullItem.name}}
</code-example>

Dado que no existe un operador de navegación seguro y `nullItem` es `null`, JavaScript y Angular lanzarían un error de referencia `nula`, el cual romperá el proceso de renderización de Angular:

<code-example language="bash">
  TypeError: Cannot read property 'name' of null.
</code-example>

Muchas veces los valores nulos en un acceso a una propiedad pueden deberse a errores pero otras veces,
estos valores nulos pueden estar bien,
especialmente cuando el valor comienza siendo nulo pero los datos llegan eventualmente y deja de ser nulo.

Con el operador de navegación segura, `?`, Angular deja de evaluar la expresión cuando encuentra un valor `null` y renderiza la vista sin errores.

Esto funciona perfectamente con largos caminos de propiedades como `a?.b?.c?.d`.

<hr/>

{@a non-null-assertion-operator}

## El operador de confirmación de no nulo ( `!` )

A partir de Typescript 2.0, puedes forzar un [chequeo estricto de null](http://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html "Chequeo estricto de null en TypeScript") con la bandera `--strictNullChecks`. TypeScript luego se asegura de que ninguna variable sea involuntariamente `null` o `undefined`.

En este modo, las variables escritas no permiten `null` y `undefined` por defecto.
El verificador de tipos arroja un error si dejas una variable sin asignar o intentas asignar `null` o `undefined` a variables cuyo tipo no permite `null` y `undefined`.

El verificador de tipos también arroja un error si no puede determinar si una variable será `nula` o `indefinida` en tiempo de ejecución. Se le puede decir al verificador de tipos que no arroje un error aplicando el sufijo
[operador de confirmación de no nulo, !](http://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#non-null-assertion-operator "Non-null assertion operator").

El operador de confirmación de no nulo de Angular `!`, sirve para el mismo propósito en
la plantilla. Por ejemplo, tu puedes asegurarte que la propiedad `item` esta definida.

<code-example path="template-expression-operators/src/app/app.component.html" region="non-null" header="src/app/app.component.html"></code-example>

Cuando el compilador Angular convierte tu plantilla en código TypeScript,
evita que TypeScript te advierta que `item.color` podría ser `null` o `undefined`.

A diferencia del [_safe navigation operator_](guide/template-expression-operators#safe-navigation-operator "Safe navigation operator (?)"),
el operador de confirmación de no nulo no protege contra `null` o `undefined`.
Más bien, le dice al chequeador de tipos de TypeScript que suspenda el chequeo estricto de `null` para una expresión especifica.

El operador de confirmación de no nulo, `!`, es opcional con la excepción de que debe usarlo cuando activa las comprobaciones estrictas de `null`.

{@a any-type-cast-function}

## La función de casteo de tipo `$any()`

A veces una expresión de enlace puede desencadenar un error de tipo durante la [compilacion AOT](guide/aot-compiler) y no es posible o difícil especificar completamente el tipo.
Para silenciar el error, puede utilizar la función de casteo `$any()` para castear la expresión a [type `any`](http://www.typescriptlang.org/docs/handbook/basic-types.html#any)
como en el siguiente ejemplo:

<code-example path="built-in-template-functions/src/app/app.component.html" region="any-type-cast-function-1" header="src/app/app.component.html"></code-example>

Cuando el compilador Angular convierte esta plantilla en código TypeScript,
evita que TypeScript advierta que `bestByDate` no es miembro del objeto `item`
cuando se ejecuta la verificación de tipos en la plantilla.

La función de casteo `$any()` también funciona con `this` para permitir el acceso a miembros no declarados de
el componente.

<code-example path="built-in-template-functions/src/app/app.component.html" region="any-type-cast-function-2" header="src/app/app.component.html"></code-example>

La función de casteo `$any()` funciona en cualquier lugar de una expresión de enlace donde la llamada al método sea valida.
