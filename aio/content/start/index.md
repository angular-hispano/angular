# Parte 1: Empezando con una aplicación Angular básica

¡Bienvenido a Angular!

Este tutorial te presenta los conceptos básicos de Angular y te guía a través de un sitio de comercio electrónico simple con un catálogo, un carrito de compras y un formulario de pago.
Para ayudarte a comenzar de inmediato, esta guía utiliza una aplicación sencilla lista para usar que puedes examinar y modificar de forma interactiva (sin tener que [configurar un entorno de trabajo local] (guía/configuración-local "Guía de configuración")).

<div class="callout is-helpful">
<header>¿Eres nuevo en el desarrollo web?</header>

Hay muchos recursos para complementar la documentación de Angular. Las documentaciones de Mozilla MDN incluyen introducciones tanto de [HTML] (https://developer.mozilla.org/en-US/docs/Learn/HTML "Aprendizaje de HTML: guías y tutoriales") como de [JavaScript] (https://developer.mozilla.org/en-US/docs/Web/JavaScript "JavaScript"). [La documentación de TypeScript] (https://www.typescriptlang.org/docs/home.html "documentación de TypeScript") incluyen un tutorial de 5 minutos. Varias plataformas de cursos en línea, como [Udemy] (http://www.udemy.com "Cursos en línea de Udemy") y [Codecademy] (https://www.codecademy.com/ "Cursos en línea de Codecademy"), también cubren conceptos básicos de desarrollo web.

</div>


{@a nuevo proyecto}
## Crea el proyecto de ejemplo

<h4>
<live-example name="Getting-started-v0" noDownload>Haz clic aquí para crear el proyecto de ejemplo listo para usar en StackBlitz.</live-example>
</h4>

<div class="lightbox">
 <img src="generate/images/guide/start/new-app-all.gif" alt="Aplicación de tienda en línea para principiantes">
</div>

* El panel de vista previa a la derecha muestra el estado inicial de la aplicación Angular de ejemplo.
Define un marco con una barra superior (que contiene el nombre de la tienda y el icono de compra) y el título para una lista de productos (que se completará y actualizará dinámicamente con datos de la aplicación).

* El panel del proyecto de la izquierda muestra los archivos fuente que componen la aplicación, estan incluidos todos los archivos de configuración e infraestructura. El archivo seleccionado actualmente aparece en el panel del editor en el medio.

Antes de entrar en la fuente de la estructura, la siguiente sección muestra cómo completar la *plantilla* HTML para la lista de productos, utilizando los datos proporcionados de ejemplo.
Esto debería darte una idea de lo fácil que es modificar y actualizar la página de forma dinámica.

<div class="el texto destacado es útil">
<header>Consejos de StackBlitz</header>

* Inicia sesión en StackBlitz para que puedas guardar y reanudar tu trabajo.
Si tienes una cuenta de GitHub, puedes iniciar sesión en StackBlitz
con esa cuenta. Para guardar su progreso, primero
bifurque el proyecto usando el botón bifurcación en la parte superior izquierda,
luego podrás guardar tu trabajo en tu propia cuenta de StackBlitz haciendo clic en el botón Guardar.
* Para copiar un código de ejemplo de este tutorial, haz clic en el icono
en la parte superior derecha del cuadro de código de ejemplo y luego pega el
fragmento de código del portapapeles en StackBlitz.
* Si el panel de vista previa de StackBlitz no muestra lo que
esperas, guarda y luego haz clic en el botón actualizar.
* StackBlitz mejora continuamente, por lo que puede haber
ligeras diferencias en el código generado, pero el comportamiento 
de la aplicación será el mismo.
* Cuando generas las aplicaciones de ejemplo que
acompañan los tutoriales de StackBlitz, StackBlitz crea el iniciador de
archivos y datos simulados para tí. Los archivos que usaras en todo
los tutoriales están en la carpeta `src` de StackBlitz
aplicaciones de ejemplo.

</div>

<div class = "la alerta es importante">

Si vas directamente al [entorno de desarrollo en línea de StackBlitz] (https://stackblitz.com/) y eliges [iniciar un nuevo espacio de trabajo angular] (https://stackblitz.com/fork/angular), obtienes una aplicación de código auxiliar, en lugar de esta [muestra ilustrativa] (#nuevo-proyecto). Una vez que se te hayan presentado los conceptos básicos aquí, esto puede ser útil para trabajar de forma interactiva mientras aprendes Angular.

En el desarrollo real, normalmente utilizaras [Angular CLI] (guía/glosario#command-line-interface-cli "Definición de CLI"), una poderosa herramienta de línea de comandos que te permite generar y modificar aplicaciones. Para obtener una guía completa paso a paso que muestra cómo usar la CLI para crear un nuevo proyecto y todas sus partes, consulte [Tutorial: Tour of Heroes] (tutorial).

</div>


{@a plantilla-sintaxis}
## Sintaxis de la plantilla

La sintaxis de la plantilla de Angular extiende HTML y JavaScript.
Esta sección presenta la sintaxis de la plantilla mejorando el área "Productos".

<div class = "la alerta es útil">

Para ayudarte a comenzar, los siguientes pasos utilizan datos de productos predefinidos del archivo `products.ts` (ya creado en el ejemplo de StackBlitz) y métodos del archivo` product-list.component.ts`.

</div>

1. En la carpeta `product-list`, abre el archivo de plantilla `product-list.component.html`.

1. Modifica la plantilla de lista de productos para mostrar una lista de nombres de productos.

1. Todos los productos de la lista se muestran de la misma manera, uno tras otro en la página. Para iterar sobre la lista predefinida de productos, coloque la directiva `*ngFor` en un `<div>`, de la siguiente manera:

 <code-example header="src/app/product-list/product-list.component.html" path="Getting-started/src/app/ product-list/product-list.component.2.html" region="ngfor">
 </code-example>

 Con `*ngFor`, el `<div>` se repite para cada producto de la lista.

 <div class="la alerta es útil">

`*ngFor` es una "directiva estructural". Las directivas estructurales dan forma o remodelan la estructura del DOM, generalmente agregando, quitando y manipulando los elementos a los que están adjuntos. Las directivas con un asterisco, `*`, son directivas estructurales.

 </div>

1. Para mostrar los nombres de los productos, utiliza la sintaxis de interpolación `{{}}`. La interpolación representa el valor de una propiedad como texto. Dentro de `<div>`, agrega un `<h3>` para mostrar la interpolación de la propiedad del nombre del producto:

 <code-example path="Getting-started/src/app/product-list/product-list.component.2.html" header="src/app/ product-list/product-list.component.html" region="interpolación">
 </code-example>

 El panel de vista previa se actualiza inmediatamente para mostrar el nombre de cada producto en la lista.

 <div class="lightbox">
 <img src="generate/images/guide/start/template-syntax-product-names.png" alt="Nombres de productos agregados a la lista">
 </div>

1. Para hacer que el nombre de cada producto sea un enlace a los detalles del producto, agrega el elemento `<a>` y establece su título como el nombre del producto utilizando la sintaxis de enlace de propiedad `[]`, de la siguiente manera:

 <code-example path="Getting-started/src/app/product-list/product-list.component.2.html" header="src/app/ product-list/product-list.component.html">
 </code-example>

 En el panel de vista previa, manten el puntero sobre un nombre de producto
 para ver el valor de la propiedad del nombre enlazado, que es
 el nombre del producto más la palabra "detalles".
 La interpolación `{{}}` le permite renderizar el
valor de propiedad como texto; el enlace de propiedad `[]` le permite
 utilizar el valor de la propiedad en una expresión de plantilla.

 <div class="lightbox">
 <img src="generado/images/guide/start/template-syntax-product-anchor.png" alt="El texto de anclaje del nombre del producto es propiedad del nombre del producto">
 </div>

4. Agrega las descripciones de los productos. En el elemento `<p>`, usa una directiva `*ngIf` para que Angular solo cree el elemento `<p>` si el producto actual tiene una descripción.

 <code-example path="Getting-started/src/app/product-list/product-list.component.3.html" header="src/app/ product-list/product-list.component.html">
 </code-example>

La aplicación ahora muestra el nombre y la descripción de cada producto en la lista. Ten en cuenta que el producto final no tiene un párrafo de descripción. Debido a que la propiedad de descripción del producto está vacía, Angular no crea el elemento `<p>` &mdash; incluida la palabra "Descripción".

 <div class="lightbox">
 <img src="generado/images/guide/start/template-syntax-product-description.png" alt="Se agregaron descripciones de productos a la lista">
 </div>

5. Agrega un botón para que los usuarios puedan compartir un producto con sus amigos. Vincula el evento `click` del botón al método `share()` (en `product-list.component.ts`). El enlace de eventos usa un conjunto de paréntesis, `()`, alrededor del evento, como en el siguiente elemento `<button>`:

 <code-example path="Getting-started/src/app/product-list/product-list.component.4.html" header="src/app/ product-list/product-list.component.html">
 </code-example>

Cada producto tiene ahora un botón "Compartir":

 <div class="lightbox">
 <img src="generado/images/guide/start/template-syntax-product-share-button.png" alt="Se agregó el botón Compartir para cada producto">
 </div>

 Prueba el botón "Compartir":

 <div class="lightbox">
 <img src="generado/images/guide/start/template-syntax-product-share-alert.png" alt="Cuadro de alerta que indica que el producto se ha compartido">
 </div>

La aplicación ahora tiene una lista de productos y una función para compartir.
En el proceso, haz aprendido a usar cinco características comunes de la sintaxis de la plantilla de Angular:
* `*ngFor`
* `*ngIf`
* Interpolación `{{}}`
* Vinculación de propiedad `[]`
* Enlace de eventos `()`


<div class="la alerta es útil">

Para una introducción más completa a la sintaxis de la plantilla de Angular, ve a [Introducción a los componentes y las plantillas] (guía/arquitectura-componentes#plantilla-sintaxis "Sintaxis de la plantilla").

</div>


{@a componentes}
## Componentes

* Componentes * definen áreas de responsabilidad en la interfaz de usuario, o UI,
que le permiten reutilizar conjuntos de funciones de la interfaz de usuario.
Ya ha creado uno con el componente de lista de productos.

Un componente consta de tres cosas:
* ** Una clase de componente ** que maneja datos y funcionalidad. En la sección anterior, los datos del producto y el método `share()` en la clase de componente manejan datos y funcionalidad, respectivamente.
* ** Una plantilla HTML ** que determina la interfaz de usuario. En la sección anterior, la plantilla HTML de la lista de productos muestra el nombre, la descripción y un botón "Compartir" para cada producto.
* ** Estilos de componentes específicos ** que definen la apariencia.
Aunque la lista de productos no define ningún estilo, aquí es donde el CSS del componente
reside.

<! -
### Definición de clase

Echemos un vistazo rápido a la definición de clase del componente de la lista de productos:

1. En el directorio `product-list`, abre `product-list.component.ts`.

1. Observa el decorador `@Component`. Este proporciona metadatos sobre el componente, incluidas sus plantillas, estilos y un selector.

* El `selector` se utiliza para identificar el componente. El selector es el nombre que le da al componente Angular cuando se representa como un elemento HTML en la página. Por convención, los selectores de componentes de Angular comienzan con el prefijo como `app-`, seguido del nombre del componente.

* La plantilla y el nombre del archivo de estilo también se proporcionan aquí. Por convención, cada una de las partes del componente está en un archivo separado, todas en el mismo directorio y con el mismo prefijo.

1. La definición del componente también incluye una clase exportada, que maneja la funcionalidad del componente. Aquí es donde se definen los datos de la lista de productos y el método `Share()`.

### Composición
->

Una aplicación Angular comprende un árbol de componentes, en el que cada componente Angular tiene un propósito y una responsabilidad específicos.

Actualmente, la aplicación de ejemplo tiene tres componentes:

<div class = "lightbox">
 <img src="generate/images/guide/start/app-components.png" alt="Tienda online con tres componentes">
</div>

* `App-root` (cuadro naranja) es el shell de la aplicación. Este es el primer componente que se carga y el padre de todos los demás componentes. Puedes pensar en el como la página base.
* `App-top-bar` (fondo azul) es el nombre de la tienda y el botón de pago.
* `App-product-list` (cuadro violeta) es la lista de productos que modificaste en la sección anterior.

La siguiente sección amplía las capacidades de la aplicación al agregar un nuevo componente &mdash; una alerta de producto &mdash; como elemento secundario del componente de lista de productos.


<div class="la alerta es útil">

Para obtener más información sobre los componentes y cómo interactúan con las plantillas, consulta [Introducción a los componentes] (guía/arquitectura-componentes "Conceptos> Introducción a componentes y plantillas").

</div>


{@a entrada}
## Entrada

Actualmente, la lista de productos muestra el nombre y la descripción de cada producto.
El componente de lista de productos también define una propiedad de `productos` que contiene datos importados para cada producto de la matriz `productos` en `productos.ts`.

El siguiente paso es crear una nueva función de alerta que tome un producto como entrada. La alerta verifica el precio del producto y, si el precio es superior a $700, muestra un botón "Notificarme" que permite a los usuarios registrarse para recibir notificaciones cuando el producto sale a la venta.

1. Crea un nuevo componente de alertas de productos.

 1. Haz clic derecho en la carpeta `app` y usa el `Angular Generator` para generar un nuevo componente llamado `product-alerts`.

 <div class="lightbox">
 <img src="generate/images/guide/start/generate-component.png" alt="Comando StackBlitz para generar componente">
 </div>

 El generador crea archivos de inicio para las tres partes del componente:
 * `product-alerts.component.ts`
 * `product-alerts.component.html`
 * `product-alerts.component.css`

1. Abre `product-alerts.component.ts`.

 <code-example header="src/app/product-alerts/product-alerts.component.ts" ruta="Getting-started/src/app/ product-alerts/product-alerts.component.1.ts" region="como se generó"></code-example>

1. Observa el decorador `@Component()`. Esto indica que la siguiente clase es un componente. Proporciona metadatos sobre el componente, incluido su selector, plantillas y estilos.

* El `selector` identifica el componente. El selector es el nombre que le da al componente Angular cuando se representa como un elemento HTML en la página. Por convención, los selectores de componentes de Angular comienzan con el prefijo `app-`, seguido del nombre del componente.

 * Los nombres de archivo de plantilla y estilo hacen referencia a los archivos HTML y CSS que genera StackBlitz.

 1. La definición del componente también exporta la clase, `ProductAlertsComponent`, que maneja la funcionalidad del componente.

1. Configura el componente de alerta de nuevos productos para recibir un producto como entrada:

 1. Importe `Input` de `@angular/core`.

 <code-example path="Getting-started/src/app/product-alerts/product-alerts.component.1.ts" región= "importaciones" header="src/app/product-alerts/product-alerts.component.ts"></code-example>

1. En la definición de la clase `ProductAlertsComponent`, define una propiedad llamada `producto` con un decorador `@Input()`. El decorador `@Input ()` indica que el valor de la propiedad pasa del padre del componente, el componente de la lista de productos.

 <code-example path="Getting-started/src/app/product-alerts/product-alerts.component.1.ts" region="input-decorator" header="src/app/product-alerts/product-alerts.component.ts"></code-example>

1. Define la vista del componente de alerta de nuevo producto.

 1. Abre la plantilla `product-alerts.component.html` y reemplaza el párrafo de marcador de posición con un botón "Notificarme" que aparece si el precio del producto es superior a $700.

 <code-example header="src/app/product-alerts/product-alerts.component.html" path="Getting-started/src/app/ product-alerts/product-alerts.component.1.html"> </código-ejemplo>

1. Muestra el componente de alerta de nuevo producto como un elemento secundario de la lista de productos.

 1. Abre `product-list.component.html`.

 1. Para incluir el nuevo componente, usa su selector, `app-product-alerts`, como lo haría con un elemento HTML.

 1. Pasa el producto actual como entrada al componente mediante el enlace de propiedad.

 <code-example header = "src/app/product-list/product-list.component.html" path="Getting-started/src/app/ product-list/product-list.component.5.html" region="alertas de producto-aplicación"></code-example>

El componente de alerta de nuevo producto toma un producto como entrada de la lista de productos. Con esa entrada, muestra u oculta el botón "Notificarme", según el precio del producto. El precio del Phone XL es de más de $700, por lo que el botón "Notificarme" aparece en ese producto.

<div class="lightbox">
 <img src="generado/images/guide/start/product-alert-button.png" alt="Botón de alerta de producto agregado a productos de más de $700">
</div>

<div class="la alerta es útil">

Consulta [Interacción de componentes] (guía/interacción de componentes "Componentes y plantillas> Interacción de componentes") para obtener más información sobre cómo pasar datos de un componente principal a secundario, interceptar y actuar sobre un valor del elemento principal y detectar y actuar sobre los cambios en valores de propiedad de entrada.

</div>


{@a salida}
## Salida

Para que funcione el botón "Notificarme", debes configurar dos cosas:

 - El componente de alerta del producto para emitir un evento cuando el usuario hace clic en "Notificarme"
 - El componente de la lista de productos para actuar en ese evento

1. Abre `product-alerts.component.ts`.

1. Importe `Output` y `ventEmitter` de `@angular/core`:

 <code-example header="src/app/product-alerts/product-alerts.component.ts" path="Getting-started/src/app/ product-alerts/product-alerts.component.ts" region="importaciones"></code-example>

1. En la clase de componente, define una propiedad llamada `notificar` con un decorado `@Output()` y una instancia de `EventEmitter()`. Esto permite que el componente de alerta de producto emita un evento cuando cambie el valor de la propiedad de notificación.

<div class="la alerta es útil">

 Cuando Angular CLI genera un nuevo componente, incluye un constructor vacío, la interfaz `OnInit` y el método `ngOnInit()`.
 Dado que el siguiente ejemplo no los usa, se omiten aquí por brevedad.

</div>

 <code-example path="Getting-started/src/app/product-alerts/product-alerts.component.ts" header="src/app/ product-alerts/product-alerts.component.ts" region="entrada -salida"></code-example>

1. En la plantilla de alerta de producto, `product-alerts.component.html`, actualiza el botón "Notificarme "con una vinculación de eventos para llamar al método `notify.emit()`.

 <code-example header="src/app/product-alerts/product-alerts.component.html" path="Getting-started/src/app/ product-alerts/product-alerts.component.html"> </code-example>

1. A continuación, define el comportamiento que debería ocurrir cuando el usuario hace clic en el botón. Recuerda que es el componente principal de la lista de productos &mdash; no el componente de alertas de productos &mdash; el que actúa cuando el hijo genera el evento. En `product-list.component.ts`, defina un método `onNotify()`, similar al método `share()`:

 <code-example header="src/app/product-list/product-list.component.ts" path="Getting-started/src/app/ product-list/product-list.component.ts" region="en-notificar"></code-example>

1. Por último, actualiza el componente de lista de productos para recibir resultados del componente de alertas de productos.

 En `product-list.component.html`, vincula el componente `app-product-alerts` (que es lo que muestra el botón" Notificarme ") al método `onNotify()` del componente de lista de productos.

 <code-example header = "src / app / product-list / product-list.component.html" path = "Getting-started / src / app / product-list / product-list.component.6.html" region = "al notificar"> </code-example>

1. Prueba el botón "Notificarme":

 <div class="lightbox">
 <img src="generado/images/guide/start/product-alert-notification.png" alt="Cuadro de diálogo de confirmación de notificación de alerta de producto">
 </div>


<div class="la alerta es útil">

Consulta [Interacción de componentes] (guía/interacción de componentes "Componentes y plantillas> Interacción de componentes") para obtener más información sobre cómo escuchar eventos de componentes secundarios, leer propiedades secundarias o invocar métodos secundarios y utilizar un servicio para la comunicación bidireccional entre componentes...

</div>


{@a próximos pasos}
## Próximos pasos

¡Felicidades! ¡Has completado tu primera aplicación Angular!

Tienes un catálogo básico de la tienda en línea con una lista de productos, el botón "Compartir" y el botón "Notificarme".
Haz aprendido sobre la base de Angular: componentes y sintaxis de plantilla.
También haz aprendido cómo interactúan la clase de componente y la plantilla, y cómo los componentes se comunican entre sí.

Para continuar explorando Angular, elije cualquiera de las siguientes opciones:
* [Continua con la sección "Navegación en la aplicación"] (inicio/inicio de ruta "Pruébelo: navegación en la aplicación") para crear una página de detalles del producto a la que se puede acceder haciendo clic en el nombre de un producto y que tiene su propia URL patrón.
* [Salta a la sección "Implementación"] (start/start-deployment "Pruébelo: Implementación") para pasar al desarrollo local o implementar su aplicación en Firebase o en tu propio servidor.
