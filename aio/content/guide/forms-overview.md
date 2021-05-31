# Introducción a los formularios en Angular

El manejo de los input con formularios es la piedra angular de muchas aplicaciones comunes. Las aplicaciones utilizan formularios para permitir a los usuarios iniciar sesión, actualizar un perfil, introducir información sensible y realizar muchas otras tareas de entrada de datos.

Angular proporciona dos enfoques diferentes para manejar los input del usuario a través de los formularios: reactivo y basado en plantillas. Ambos capturan los eventos input del usuario desde la vista, validan la entrada del usuario, crean un modelo de formulario y un modelo de datos para actualizar, y proporcionan una forma de rastrear los cambios.

Esta guía proporciona información que ayuda a decidir qué tipo de formulario es el más adecuado para su situación. Presenta los bloques de construcción comunes utilizados por ambos enfoques. También resume las diferencias clave entre los dos enfoques y demuestra esas diferencias en el contexto de la configuración, el flujo de datos y las pruebas. 

## Pre requisitos

 Esta guía asume que tienes un entendimiento básico de lo siguiente:

* [TypeScript](https://www.typescriptlang.org/docs/home.html "The TypeScript language") y HTML5.

* Fundamentos de diseño de aplicaciones de Angular, como se describe en los [Conceptos de Angular](guide/architecture "Introduction to Angular concepts.").

* Los fundamentos de la [sintaxis de las plantillas de Angular](guide/architecture-components#template-syntax "Template syntax intro").

## Elección de un enfoque

Los formularios reactivos y los formularios basados en plantillas procesan y gestionan los datos del formulario de forma diferente. Cada enfoque ofrece diferentes ventajas.

* **Formularios reactivos** proporcionan un acceso directo y explícito al modelo de objetos de los formularios subyacentes. En comparación con los formularios basados en plantillas, son másfuertes: son más escalables, reutilizables y comprobables. Si los formularios son una parte clave de su aplicación, o si ya está utilizando patrones reactivos para construir su aplicación, utilizar formularios reactivos.

* **Formularios basados en plantillas** se basan en las directivas de la plantilla para crear y manipular el modelo de objetos subyacente. Son útiles para añadir un formulario sencillo a una aplicación, como un formulario de inscripción a una lista de correo electrónico. Son fáciles de añadir a una aplicación, pero no escalan tan bien como los formularios reactivos. Si tienes unos requisitos de formulario muy básicos y una lógica que puede gestionarse únicamente en la plantilla, los formularios impulsados por plantillas podrían ser una buena opción.

### Diferencias clave

La siguiente tabla resume las principales diferencias entre los formularios reactivos y los basados en plantillas.

<style>
  table {width: 100%};
  td, th {vertical-align: top};
</style>

||Reactivo|Formulario basado en plantillas|
|--- |--- |--- |
|[Configuración del modelo de formulario](#setup) | Explícita, creada en la clase del componente	 | Implícita, creada por directivas |
|[Data model](#data-flow-in-forms) | Estructurado e inmutable | No estructurado y mutable |
|Predecible | Sincrónico | Asíncrono |
|[Validación de formularios](#validation) | Funciones | Directivas |

### Escalabilidad

Si los formularios son una parte central de su aplicación, la escalabilidad es muy importante. Poder reutilizar los modelos de formularios en todos los componentes es fundamental. 

Los formularios reactivos son más escalables que los formularios basados en plantillas. Proporcionan acceso directo a la API subyacente del formulario y acceso sincrónico al modelo de datos del formulario, lo que facilita la creación de formularios a gran escala. Los formularios reactivos requieren menos configuración para las pruebas, y las pruebas no requieren un conocimiento profundo de la detección de cambios para probar adecuadamente las actualizaciones y la validación de los formularios.

Los formularios basados en plantillas se centran en escenarios simples y no son tan reutilizables. Abstraen la API subyacente del formulario y sólo proporcionan acceso asíncrono al modelo de datos del formulario. La abstracción de los formularios basados en plantillas también afecta a las pruebas. Las pruebas dependen en gran medida de la ejecución manual de la detección de cambios para ejecutarse correctamente, y requieren más configuración.

{@a setup}

## Configuración del modelo de formulario

Tanto los formularios reactivos como los impulsados por plantillas hacen un seguimiento de los cambios de valor entre los elementos input del formulario con los que interactúan los usuarios y los datos del formulario en su modelo de componentes. Los dos enfoques comparten bloques de construcción subyacentes, pero difieren en la forma de crear y gestionar las instancias comunes de control del formulario.

### Clases base de formularios comunes

Tanto los formularios reactivos como los impulsados por plantillas están construidos sobre las siguientes clases base.

* `FormControl` registra el valor y el estado de validación de un control de formulario individual.

* `FormGroup` registra los mismos valores y estados de una colección de controles de formulario.

* `FormArray` registra los mismos valores y estados para un array de controles de formulario.

* `ControlValueAccessor` crea un puente entre las instancias de Angular `FormControl` y los elementos nativos del DOM.

{@a setup-the-form-model}

### Configuración en los formularios reactivos

Con los formularios reactivos, se define el modelo de formulario directamente en la clase del componente. La directiva `[formControl]` vincula la instancia de `FormControl` creada explícitamente a un elemento de formulario específico en la vista, utilizando un accesorio de valor interno.

El siguiente componente implementa un campo input para un solo control, utilizando formularios reactivos. En este ejemplo, el modelo de formulario es la instancia `FormControl`.

<code-example path="forms-overview/src/app/reactive/favorite-color/favorite-color.component.ts">
</code-example>

La figura 1 muestra cómo, en los formularios reactivos, el modelo de formulario es la fuente de la verdad; proporciona el valor y el estado del elemento del formulario en cualquier momento, a través de la directiva `[formControl]` en el elemento input.

**Figura 1.** *Acceso directo al modelo de formularios en un formulario reactivo.*

<div class="lightbox">
  <img src="generated/images/guide/forms-overview/key-diff-reactive-forms.png" alt="Formularios reactivos key differences">
</div>

### Configuración en formularios basados en plantillas

En los formularios basados en plantillas, el modelo de formulario es implícito, en lugar de explícito. La directiva `NgModel` crea y gestiona una instancia de `FormControl` para un elemento de formulario determinado.

El siguiente componente implementa el mismo campo input para un único control, utilizando formularios basados en plantillas.

<code-example path="forms-overview/src/app/template/favorite-color/favorite-color.component.ts">
</code-example>

En un formulario dirigido por una plantilla, la fuente de verdad es la plantilla. No tienes acceso programático directo a la instancia `FormControl`, como se muestra en la Figura 2.

**Figura 2.** *Acceso indirecto al modelo de formularios en un formulario basado en plantillas.*

<div class="lightbox">
  <img src="generated/images/guide/forms-overview/key-diff-td-forms.png" alt="Formularios basados en plantillas key differences">
</div>

{@a data-flow-in-forms}

## Flujo de datos en los formularios

Cuando una aplicación contiene un formulario, Angular debe mantener la vista sincronizada con el modelo de componentes y el modelo de componentes sincronizado con la vista. A medida que los usuarios cambian valores y hacen selecciones a través de la vista, los nuevos valores deben reflejarse en el modelo de datos. Del mismo modo, cuando la lógica del programa cambia los valores en el modelo de datos, esos valores deben reflejarse en la vista.

Los formularios reactivos y los impulsados por plantillas difieren en cómo manejan los datos que fluyen desde el usuario o desde los cambios programáticos. Los siguientes diagramas ilustran ambos tipos de flujo de datos para cada tipo de formulario, utilizando el campo input de color favorito definido anteriormente.

### Flujo de datos en los formularios reactivos

En los formularios reactivos cada elemento del formulario en la vista está directamente vinculado al modelo del formulario (una instancia de `FormControl`). Las actualizaciones de la vista al modelo y del modelo a la vista son síncronas y no dependen de cómo se renderice la UI.

El diagrama de la vista al modelo muestra cómo fluyen los datos cuando se cambia el valor de un campo input desde la vista a través de los siguientes pasos.

1. El usuario escribe un valor en el elemento input, en este caso el color favorito *Azul*.
1. El elemento input del formulario emite un evento "input" con el último valor.
1. El accesorio de control de valores que escucha los eventos del elemento input del formulario transmite inmediatamente el nuevo valor a la instancia de `FormControl`
1. La instancia de `FormControl` emite el nuevo valor a través del observable `valueChanges`.
1. Cualquier suscriptor del observable `valueChanges` recibe el nuevo valor.

<div class="lightbox">
  <img src="generated/images/guide/forms-overview/dataflow-reactive-forms-vtm.png" alt="Formularios reactivos data flow - view to model">
</div>

El diagrama modelo-vista muestra cómo un cambio programático en el modelo se propaga a la vista a través de los siguientes pasos.

1. El usuario llama al método `favoriteColorControl.setValue()`, que actualiza el valor del `FormControl`.
1. La instancia `FormControl` emite el nuevo valor a través del observable `valueChanges`.
1. Los suscriptores del observable `valueChanges` reciben el nuevo valor.
1. El accesor del valor del control en el elemento input del formulario actualiza el elemento con el nuevo valor.

<div class="lightbox">
  <img src="generated/images/guide/forms-overview/dataflow-reactive-forms-mtv.png" alt="Formularios reactivos data flow - model to view">
</div>

### Flujo de datos en formularios basados en plantillas

En los formularios basados en plantillas, cada elemento del formulario está vinculado a una directiva que gestiona internamente el modelo del formulario.

El diagrama de la vista al modelo muestra cómo fluyen los datos cuando se cambia el valor de un campo input desde la vista a través de los siguientes pasos.

1. El usuario escribe *Azul* en el elemento input.
1. El elemento input emite un evento "input" con el valor *Azul*.
1. El accesorio de valor del control adjunto al input activa el método `setValue()` en la instancia `FormControl`.
1. La instancia `FormControl` emite el nuevo valor a través del observable `valueChanges`.
1. Los suscriptores del observable `valueChanges` reciben el nuevo valor.
1. El accesor del valor de control también llama al método `NgModel.viewToModelUpdate()` que emite un evento `ngModelChange`.
1. Dado que la plantilla del componente utiliza un enlace de datos bidireccional para la propiedad `favoriteColor`, la propiedad `favoriteColor` del componente
se actualiza con el valor emitido por el evento `ngModelChange` (*Azul*).

<div class="lightbox">
  <img src="generated/images/guide/forms-overview/dataflow-td-forms-vtm.png" alt="Formularios basados en plantillas data flow - view to model" width="100%">
</div>

El diagrama modelo-vista muestra cómo fluyen los datos del modelo a la vista cuando el `ColorFavorito` cambia de *Azul* a *Rojo*, a través de los siguientes pasos

1. El valor de `ColorFavorito` se actualiza en el componente.
1. Comienza la detección de cambios.
1. Durante la detección de cambios, el hook del ciclo de vida `ngOnChanges` es llamado en la instancia de la directiva `NgModel` porque el valor de una de sus input han cambiado.
1. El método `ngOnChanges()` pone en cola una tarea asíncrona para establecer el valor de la instancia interna `FormControl`.
1. La detección de cambios se ha completado.
1. En el siguiente tick, se ejecuta la tarea para establecer el valor de la instancia `FormControl`.
1. La instancia `FormControl` emite el último valor a través del observable `valueChanges`.
1. Los suscriptores del observable `valueChanges` reciben el nuevo valor.
1. El accesorio de valor del control actualiza el elemento input del formulario en la vista con el último valor de `favoriteColor`.

<div class="lightbox">
  <img src="generated/images/guide/forms-overview/dataflow-td-forms-mtv.png" alt="Formularios basados en plantillas data flow - model to view" width="100%">
</div>

### Mutabilidad del modelo de datos

El método de seguimiento de los cambios influye en la eficacia de su aplicación.

* **Formularios reactivos** mantener el modelo de datos puro proporcionándolo como una estructura de datos inmutable.
Cada vez que se activa un cambio en el modelo de datos, la instancia `FormControl` devuelve un nuevo modelo de datos en lugar de actualizar el modelo de datos existente.
Esto le da la capacidad de rastrear cambios únicos en el modelo de datos a través del observable del control.
La detección de cambios es más eficiente porque sólo necesita actualizar los cambios únicos.
Dado que las actualizaciones de datos siguen patrones reactivos, puede integrarse con operadores observables para transformar los datos.

* **Formularios basados en plantillas** Los formularios se basan en la mutabilidad con enlace de datos bidireccional para actualizar el modelo de datos en el componente a medida que se realizan cambios en el modelo.
Debido a que no hay cambios únicos que rastrear en el modelo de datos cuando se utiliza la vinculación de datos bidireccional, la detección de cambios es menos eficiente para determinar cuándo se requieren actualizaciones.

La diferencia se demuestra en los ejemplos anteriores que utilizan el elemento input favorite-color.

* Con los formularios reactivos, la instancia **`FormControl`** siempre devuelve un nuevo valor cuando se actualiza el valor del control.

* En los formularios basados en plantillas, la propiedad **color favorito** siempre se modifica a su nuevo valor.

{@a validation}
## Validación de formularios

La validación es una parte integral de la gestión de cualquier conjunto de formularios. Ya sea que esté comprobando los campos obligatorios o consultando una API externa para un nombre de usuario existente, Angular proporciona un conjunto de validadores incorporados, así como la capacidad de crear validadores personalizados.

* **Formularios reactivos** definir validadores personalizados como **funciones** que reciben un control para validar.
* **Formularios basados en plantillas** están ligados a las **directivas** de las plantillas, y deben proporcionar directivas de validación personalizadas que envuelvan las funciones de validación.

Para más información, consulte [Validación de Formularios](guide/form-validation).

## Pruebas

Las pruebas juegan un papel importante en las aplicaciones complejas. Una estrategia de pruebas más simple es útil cuando se valida que sus formularios funcionan correctamente.
Los Formularios reactivos y los Formularios basados en plantillas tienen diferentes niveles de dependencia de la renderización de la UI para realizar aserciones basadas en el control del formulario y en los cambios del campo del formulario.
Los siguientes ejemplos demuestran el proceso de prueba de formularios reactivos y Formularios basados en plantillas.

### Probando Formularios reactivos

Los formularios reactivos proporcionan una estrategia de pruebas relativamente fácil porque proporcionan acceso sincrónico a los modelos de formulario y de datos, y pueden ser probados sin renderizar la UI.
En estas pruebas, el estado y los datos se consultan y manipulan a través del control sin interactuar con el ciclo de detección de cambios.

Las siguientes pruebas utilizan los componentes de color favorito de los ejemplos anteriores para verificar los flujos de datos de vista a modelo y de modelo a vista para un formulario reactivo.

**Verificación del flujo de datos de la vista al modelo**

El primer ejemplo realiza los siguientes pasos para verificar el flujo de datos de la vista al modelo.

1. Consulta la vista para el elemento input del formulario, y crea un evento "input" personalizado para la prueba.
1. Establezca el nuevo valor del input como *Rojo*, y envíe el evento "input" en el elemento input del formulario.
1. Comprueba que el valor del componente `favoriteColorControl` coincide con el valor del input.

<code-example path="forms-overview/src/app/reactive/favorite-color/favorite-color.component.spec.ts" region="view-to-model" header="Favorite color test - view to model">
</code-example>

El siguiente ejemplo realiza los siguientes pasos para verificar el flujo de datos del modelo a la vista.

1. Utilice el `favoriteColorControl`, una instancia de `FormControl`, para establecer el nuevo valor.
1. Consulte la vista para el elemento input del formulario.
1. Compruebe que el nuevo valor establecido en el control coincide con el valor de la entrada.

<code-example path="forms-overview/src/app/reactive/favorite-color/favorite-color.component.spec.ts" region="model-to-view" header="Favorite color test - model to view">
</code-example>

### Testing Formularios basados en plantillas

Escribir pruebas con Formularios basados en plantillas requiere un conocimiento detallado del proceso de detección de cambios y una comprensión de cómo se ejecutan las directivas en cada ciclo para asegurar que los elementos son consultados, probados o cambiados en el momento correcto.

Las siguientes pruebas utilizan los componentes de color favoritos mencionados anteriormente para verificar los flujos de datos de la vista al modelo y del modelo a la vista para un formulario basado en plantillas.

La siguiente prueba verifica el flujo de datos de la vista al modelo.

<code-example path="forms-overview/src/app/template/favorite-color/favorite-color.component.spec.ts" region="view-to-model" header="Favorite color test - view to model">
</code-example>

Estos son los pasos que se realizan en la prueba de la vista al modelo.

1. Consultar la vista para el elemento input del formulario, y crear un evento "input" personalizado para la prueba.
1. Establezca el nuevo valor del input como *Rojo*, y envíe el evento "input" en el elemento input del formulario.
1. Ejecute la detección de cambios a través del dispositivo de prueba.
1. Compruebe que el valor de la propiedad `favoriteColor` del componente coincide con el valor del input.

La siguiente prueba verifica el flujo de datos del modelo a la vista.

<code-example path="forms-overview/src/app/template/favorite-color/favorite-color.component.spec.ts" region="model-to-view" header="Favorite color test - model to view">
</code-example>

Estos son los pasos que se realizan en la prueba del modelo a la vista.

1. Utilice la instancia del componente para establecer el valor de la propiedad `favoriteColor`.
1. Ejecutar la detección de cambios a través del test fixture.
1. Utilice el método `tick()` para simular el paso del tiempo dentro de la tarea `fakeAsync()`.
1. Consultar la vista para el elemento input del formulario.
1. Asegura que el valor de la entrada coincide con el valor de la propiedad `favoriteColor` en la instancia del componente.


## Próximos pasos

Para saber más sobre los Formularios reactivos, consulte las siguientes guías:

* [Formularios reactivos](guide/reactive-forms)
* [Validación de formularios](guide/form-validation#reactive-form-validation)
* [Formas dinámicas](guide/dynamic-form)

Para saber más sobre Formularios basados en plantillas, consulte las siguientes guías:

* [Construir un formulario basado en una plantilla](guide/forms) tutorial
* [Validación de formularios](guide/form-validation#template-driven-validation)
* `NgForm` directive API reference
