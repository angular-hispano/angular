# Construyendo formularios dinámicos

Muchos formularios, como cuestionarios, pueden ser muy similares entre sí en formato e intención.
Para que sea más rápido y fácil generar diferentes versiones de dicho formulario, puedes crear una _plantilla de un formulario dinámico_ basado en metadatos que describen el modelo de negocio.

Después puedes usar la plantilla para generar nuevos formularios automáticamente, acorde a los cambios en los datos del modelo.

La técnica es particularmente útil cuando tienes un tipo de formularios cuyo contenido debe cambiar con frecuencia para cumplir con los requisitos y cambios del negocio que están en constante evolución.

Un típico caso de uso es un cuestionario. Puede que necesites obtener el input de los usuarios en diferentes contextos.

El formato y estilo de los formularios que ve un usuario deben permanecer constantes, mientras que las preguntas reales que debes preguntar varían según el contexto.

En este tutorial construirás un formulario dinámico que representa un cuestionario básico.
Construirás una aplicación online para héroes en búsqueda de empleo.
La agencia está jugando constantemente con el proceso de aplicación, pero usando formularios dinámicos puedes crear nuevos formularios sobre la marcha sin cambiar el código de la aplicación.

El tutorial se desarrolla a través de los siguientes pasos.

1. Habilita los formularios reactivos para un proyecto.
2. Estabiliza un modelo de datos para representar los controles del formulario.
3. Poblar el modelo con datos de muestra.
4. Desarrolla un componente para crear dinámicamente los controles del formulario.

El formulario a crear usa validaciones en los inputs y estilos para mejorar la experiencia de usuario. Tiene un botón de envío que solo es habilitado cuando todos los inputs del usuario son válidos, remarca los inputs inválidos con un color y muestra mensajes de error.

La versión básica puede evolucionar para dar soporte a una variedad más rica de preguntas, una representación más elegante y una experiencia de usuario superior.

<div class="alert is-helpful">

Ver el <live-example name="dynamic-form"></live-example>.

</div>

## Prerrequisitos

Antes de comenzar con este tutorial, deberías tener conocimiento básico de lo siguiente:

- Programación con [TypeScript](https://www.typescriptlang.org/docs/home.html "El lenguaje TypeScript") y HTML5.

- Conceptos fundamentales de [diseño de aplicaciones con Angular](guide/architecture "Introducción a los conceptos de Angular").

- Conocimiento básico de [formularios reactivos](guide/reactive-forms "Guía de formularios reactivos").

## Activar formularios reactivos para tu proyecto

Los formularios dinámicos están basados en formularios reactivos. Para que la aplicación tenga acceso a las directivas de los formularios reactivos, el [módulo raíz](guide/bootstrapping "Aprende sobre bootstrapping una aplicación desde el módulo raíz.") importa `ReactiveFormsModule` desde la librería `@angular/forms`.

El siguiente código de ejemplo muestra la configuración del módulo raíz.

<code-tabs>

  <code-pane header="app.module.ts" path="dynamic-form/src/app/app.module.ts">

  </code-pane>

  <code-pane header="main.ts" path="dynamic-form/src/main.ts">

  </code-pane>

</code-tabs>

{@a object-model}

## Crear un objeto modelo de formulario

Un formulario dinámico requiere un objeto modelo que puede describir todos los escenarios que se necesitan para la funcionalidad del formulario.
El formulario para el ejemplo de la solicitud del héroe es un conjunto de preguntas&mdash;en el cual, cada control en el formulario debe hacer una pregunta y aceptar una respuesta.

El modelo de datos para este tipo de formulario debe representar una pregunta.
El ejemplo incluye el `DynamicFormQuestionComponent`, el cual define una pregunta como el objeto fundamental en el modelo.

El siguiente `QuestionBase` es una clase base para un conjunto de controles que pueden representar la pregunta y su respuesta en el formulario.

<code-example path="dynamic-form/src/app/question-base.ts" header="src/app/question-base.ts">

</code-example>

### Define clases de control

De esta base, el ejemplo deriva dos nuevas clases, `TextboxQuestion` y `DropdownQuestion`, que representa diferentes tipos de controles.
Cuando crees la plantilla del formulario en el siguiente paso, crearás una instancia de estos tipos de preguntas específicas para representar los controles apropiados de forma dinámica.

- El tipo de control `TextboxQuestion` presenta una pregunta y permite a los usuarios ingresar datos.

  <code-example path="dynamic-form/src/app/question-textbox.ts" header="src/app/question-textbox.ts"></code-example>

  El tipo de control `TextboxQuestion` será representado en una plantilla de formulario usando un elemento `<input>`.
  El atributo `type` del elemento será definido con base en el campo `type` especificado en el argumento `options` (por ejemplo `text`, `email`, `url`).

- El control `DropdownQuestion` presenta una lista de opciones en una caja de selección.

  <code-example path="dynamic-form/src/app/question-dropdown.ts" header="src/app/question-dropdown.ts"></code-example>

### Crear grupos de formularios

Un formulario dinámico usa un servicio para crear un conjunto de controles de entrada agrupados, basados en el modelo del formulario.
El siguiente `QuestionControlService` toma un conjunto de instancias de `FormGroup` que consumen metadatos desde el modelo de pregunta. Puedes especificar valores por defecto y reglas de validación.

<code-example path="dynamic-form/src/app/question-control.service.ts" header="src/app/question-control.service.ts"></code-example>

{@a form-component}

## Crear contenidos de formularios dinámicos

El formulario dinámico en sí será representado por un componente contenedor, el cual agregarás en un paso más adelante.
Cada pregunta es representada en la plantilla del componente del formulario mediante una etiqueta `<app-question>`, la cual coincide con una instancia de `DynamicFormQuestionComponent`.

El `DynamicFormQuestionComponent` es responsable de renderizar los detalles de una pregunta individual con base en los datos del objeto asociados a la pregunta.
El formulario utiliza una [directiva `[formGroup]`](api/forms/FormGroupDirective "referencia a la API") para conectar la plantilla HTML a los objetos de control subyacentes.
El `DynamicFormQuestionComponent` crea grupos de formularios y los completa con controles definidos en el modelo de preguntas, especificando reglas de visualización y validación.

<code-tabs>

  <code-pane header="dynamic-form-question.component.html" path="dynamic-form/src/app/dynamic-form-question.component.html">

  </code-pane>

  <code-pane header="dynamic-form-question.component.ts" path="dynamic-form/src/app/dynamic-form-question.component.ts">

  </code-pane>

</code-tabs>

El objetivo del `DynamicFormQuestionComponent` es presentar tipos de preguntas definidas en tu modelo.
Solo tienes dos tipos de preguntas hasta este punto, pero puedes imaginar muchas más.
La declaración `ngSwitch` en la plantilla determina qué tipo de pregunta mostrar.
El switch utiliza las directivas con los selectores [`formControlName`](api/forms/FormControlName "referencia a la API de la directiva FormControlName") y [`formGroup`](api/forms/FormGroupDirective "reerencia a la API de la directiva FormGroupDirective"). Ambas directivas son definidas en el módulo `ReactiveFormsModule`.

{@a questionnaire-data}

### Suministrar datos

Se necesita de otro servicio para suministrar un conjunto específico de preguntas a través de las cuales construir un formulario individual.
Para este ejercicio crearás un servicio llamado `QuestionService` para proporcionar un array de preguntas a partir de los datos de muestra codificados de forma rígida.
En una aplicación del mundo real, el servicio debe traer los datos desde un backend.
El punto clave, sin embargo, es que tú controlas las preguntas de la solicitud de empleo del héroe por completo a través de los objetos devueltos por `QuestionService`.
Para mantener el cuestionario a medida que cambian los requisitos, solo necesitas agregar, actualizar y eliminar objetos desde el array de `questions`.

El `QuestionService` proporciona un conjunto de preguntas en forma de un array vinculado a las preguntas de `@Input()`

<code-example path="dynamic-form/src/app/question.service.ts" header="src/app/question.service.ts">

</code-example>

{@a dynamic-template}

## Crea una plantilla para el formulario dinámico

El componente `DynamicFormComponent` es el punto de entrada y el contenedor principal para el formulario, el cual es representado con la etiqueta `<app-dynamic-form>`.

El componente `DynamicFormComponent` presenta una lista de preguntas vinculada cada una a un elemento `<app-question>` que coincide con el componente `DynamicFormQuestionComponent`.

<code-tabs>

  <code-pane header="dynamic-form.component.html" path="dynamic-form/src/app/dynamic-form.component.html">

  </code-pane>

  <code-pane header="dynamic-form.component.ts" path="dynamic-form/src/app/dynamic-form.component.ts">

  </code-pane>

</code-tabs>

### Visualizar el formulario

Para visualizar una instancia del formulario dinámico, la plantilla del `AppComponent` pasa el array de `questions` retornado por el servicio `QuestionService` al componente contenedor del formulario, `<app-dynamic-form>`.

<code-example path="dynamic-form/src/app/app.component.ts" header="app.component.ts">

</code-example>

El ejemplo proporciona un modelo para una solicitud de empleo para héroes, pero no hay referencias a ninguna pregunta específica de algún héroe que no sean los objetos devueltos por `QuestionService`.
Esta separación de datos y modelos te permite reutilizar los componentes para cualquier tipo de encuesta siempre y cuando sea compatible con el objeto modelo _question_.

### Asegurando datos válidos

La plantilla del formulario utiliza el enlace de datos dinámicos de metadatos para renderizar el formulario sin hacer suposiciones con datos en duro sobre preguntas específicas.
Agrega tanto metadatos de control como criterios de validación de forma dinámica.

Para asegurar que los inputs sean válidos, el botón de _Save_ está deshabilitado hasta que el formulario esté en un estado válido. Cuando el formulario es válido, puedes dar click en _Save_ y la aplicación renderizará los actuales valores del formulario en formato JSON.

La siguiente imagen muestra el formulario final.

<div class="lightbox">
  <img src="generated/images/guide/dynamic-form/dynamic-form.png" alt="Dynamic-Form">
</div>

## Pasos siguientes

- **Diferentes tipos de formularios y colecciones de controles**
  Este tutorial muestra como construir un cuestionario, el cual es solo un tipo de formulario dinámico.
  Para un ejemplo de un formulario dinámico diferente, ve la sección [Crear formularios dinámicos](guide/reactive-forms#creating-dynamic-forms "Crear formularios dinámicos con arrays") en la guía de formularios reactivos.
  El ejemplo también muestra como usar `FormArray` en lugar de `FormGroup` para recopilar un conjunto de controles.

- **Validando el input de usuario**
  La sección [Validando el input de un formulario](guide/reactive-forms#validating-form-input "Validación básica de input") introduce las bases de como funcionan las validaciones en los inputs para los formularios reactivos.

  La [Guía de validación de formularios](guide/form-validation "Guía de validación de formularios") toca el tema más a profundidad.
