# Introducción a las animaciones en Angular

La animación proporciona la ilusión de movimiento: los elementos HTML cambian de estilo con el tiempo. 
Las animaciones bien diseñadas pueden hacer que tu aplicación sea más divertida y fácil de usar, pero no son sólo un decorado. Las animaciones pueden mejorar tu aplicación y la experiencia del usuario de varias formas:

* Sin animaciones, las transiciones de páginas web pueden parecer abruptas y discordantes.

* El movimiento mejora en gran medida la experiencia del usuario, por lo que las animaciones brindan a los usuarios la oportunidad de detectar la respuesta de la aplicación a sus acciones.

* Las buenas animaciones llaman la atención del usuario de forma intuitiva hacia donde se necesita.

Normalmente, las animaciones implican múltiples *transformaciones* de estilo a lo largo del tiempo. Un elemento HTML puede moverse, cambiar de color, crecer o encogerse, desvanecerse o deslizarse fuera de la página. Estos cambios pueden ocurrir de forma simultánea o secuencial. Puedes controlar el momento de cada transformación.

El sistema de animación de Angular se basa en las funcionalidades  de CSS, lo que significa que puedes animar cualquier propiedad que el navegador considere animable. Esto incluye posiciones, tamaños, transformaciones, colores, bordes, etc. El W3C mantiene una lista de propiedades animables en tu página
[Transiciones CSS](https://www.w3.org/TR/css-transitions-1/).

## Acerca de esta guía
Esta guía cubre las funcionalidades básicas de animación en Angular para comenzar a agregar animaciones de Angular a su proyecto.

Las funcionalidades descritas en esta guía &mdash; y las funcionalidades más avanzadas descritas en las guías de animaciones en Angular relacionadas &mdash; se muestran en una aplicación de ejemplo disponible como <live-example></live-example>.

#### Prerequisitos
La guía asume que está familiarizado con la creación de aplicaciones básicas en Angular, como se describe en las siguientes secciones:

* [Tutorial](tutorial)
* [Información sobre la arquitectura](guide/architecture)

## Comenzando

Los principales módulos de Angular para animaciones son `@angular/animations` y `@angular/platform-browser`. Cuando crea un nuevo proyecto usando CLI, estas dependencias se agregan automáticamente a su proyecto.

Para comenzar a agregar animaciones de Angular a su proyecto, importe los módulos específicos de animación junto con la funcionalidad estándar de Angular.

### Paso 1: Habilitando del módulo de animaciones

Importa `BrowserAnimationsmodule`, que introduce las capacidades de animación en el módulo raíz de tu aplicación Angular.

<code-example path="animations/src/app/app.module.1.ts" header="src/app/app.module.ts" language="typescript"></code-example>

<div class="alert is-helpful">

**Nota:** Cuando usas CLI para crear tu aplicación, el módulo raíz de la aplicación `app.module.ts` se coloca en la carpeta `src/app`.

</div>

### Paso 2: Importando funciones de animación en componentes

Si planea usar funciones específicas de animación en componentes, importe esas funciones desde `@angular/animations`.

<code-example path="animations/src/app/app.component.ts" header="src/app/app.component.ts" region="imports" language="typescript">
</code-example>

<div class="alert is-helpful">

**Nota:** Ver [resumen de las funciones de animación disponibles](guide/animations#animation-api-summary) al final de esta guía.
</div>

### Paso 3: Agregando la propiedad metadatos a la animación

En el archivo del componente, agrega la propiedad de metadatos llamada `animations:` dentro del decorador `@Component()`. 

Pon este disparador que define una animación dentro de la propiedad de metadatos `animaciones`.

<code-example path="animations/src/app/app.component.ts" header="src/app/app.component.ts" region="decorator" language="typescript">
</code-example>

## Animando una transición simple

Vamos a animar una transición simple que cambia un solo elemento HTML de un estado a otro. Por ejemplo, puede especificar que un botón muestre **Open** o **Closed** basado en la última acción del usuario. Cuando el botón está en el estado "Open", es visible y amarillo. Cuando es el estado "Closed", es transparente y verde.

En HTML, estos atributos se establecen habitualmente mediante estilos CSS, como el color y la opacidad. En Angular, use la función `style()` para especificar un conjunto de estilos CSS para usar con animaciones. Puede recopilar un conjunto de estilos en un estado de animación y darle un nombre al estado, como "open" o "closed".

<div class="lightbox">
  <img src="generated/images/guide/animations/open-closed.png" alt="open and closed states">
</div>

### Animación, estados y estilos

Use la función `state()` de Angular para definir diferentes estados para llamar al final de cada transición. Esta función toma dos argumentos: un nombre único como `open` o `closed` y una función `style()`.

Utilice la función `style()` para definir un conjunto de estilos para asociarlos con un nombre de estado dado. Tenga en cuenta que los atributos de estilo deben estar en[*camelCase*](guide/glossary#case-conventions).

Vamos a ver como funciona la función `state()` de Angular trabajando con la función `style()` para establecer atributos de estilo CSS. En este fragmento de código, se establecen varios atributos de estilo al mismo tiempo para el estado. 
En el estado `open`, el botón tiene una altura de 200 píxeles, una opacidad de 1 y un color de fondo amarillo.

<code-example path="animations/src/app/open-close.component.ts" header="src/app/open-close.component.ts" region="state1" language="typescript">
</code-example>

En el estado `closed`, que se muestra a continuación, el botón tiene una altura de 100 píxeles, una opacidad de 0,5 y un color de fondo verde.

<code-example path="animations/src/app/open-close.component.ts" header="src/app/open-close.component.ts" region="state2" language="typescript">
</code-example>

### Transiciones y sincronización

En Angular, puede establecer varios estilos sin ninguna animación. Sin embargo, sin más refinamiento, el botón se transforma instantáneamente sin desvanecimiento, sin encogimiento u otro indicador visible de que se está produciendo un cambio.

Para que el cambio sea menos abrupto, necesitamos definir una animación *transition* para especificar los cambios que ocurren entre un estado y otro durante un período de tiempo. La función `transition()` acepta dos argumentos: el primer argumento acepta una expresión que define la dirección entre dos estados de transición, 
y el segundo argumento acepta uno o una serie de pasos de `animate()`.


Use la función `animate()` para definir la duración, el retraso y la suavización de una transición, y para designar la función de estilo para definir estilos mientras se realizan las transiciones. También puede utilizar la función `animate()` para definir la función `keyframes()` para animaciones de varios pasos. Estas definiciones se colocan en el segundo argumento de la función `animate()`.


#### Metadatos de animación: duración, retraso y suavizado

La función `animate()` (Segundo argumento de la función transition) acepta los parámetros de entrada `timings` y `styles`.

El parametro `timings` toma un string definido en tres partes.

>`animate('duration delay easing')`

La primera parte, `duration`, es requerrida. La duración se puede expresar en milisegundos como un simple número sin comillas, o en segundos con comillas y un tiempo específico. Por ejemplo, una duración de una décima de segundo se puede expresar de la siguiente manera:

* Como un número plano, en milisegundos: `100`

* En un string, como milisegundos: `'100ms'`

* En un string, como segundos: `'0.1s'`

El segundo argumento, `delay`, tiene la misma sintaxis que `duration`. Por ejemplo:

* Espere 100ms y luego ejecute  por 200ms: `'0.2s 100ms'`

El tercer argumento, `easing`, controla cómo la animación [acelera y desacelera](http://easings.net/) durante su tiempo de ejecución. Por ejemplo, `ease-in` hace que la animación comience lentamente y aumente la velocidad a medida que avanza.

* Esperar por 100ms, correr por 200ms. Use una curva de desaceleración para comenzar rápido y desacelere lentamente hasta un punto de reposo: `'0.2s 100ms ease-out'`

* Corre por 200ms, sin demora. Usa una curva estándar para comenzar lento, acelerar en el medio y luego desacelerar lentamente al final: `'0.2s ease-in-out'`

* Comenzar de inmediato, correr por 200ms. Use una curva de aceleración para comenzar lento y terminar a máxima velocidad: `'0.2s ease-in'`

<div class="alert is-helpful">

**Nota:** Consulte el tema en el sitio web de Material Design como [Curvas de suavizado natural](https://material.io/design/motion/speed.html#easing) para obtener información general sobre curvas de flexión.
</div>

Este ejemplo proporciona una transición de estado de `open` a `closed` con una transición de un segundo entre estados.

<code-example path="animations/src/app/open-close.component.ts" header="src/app/open-close.component.ts" language="typescript"
region="transition1">
</code-example>


En el fragmento de código anterior, el operador `=>` indica transiciones unidireccionales y `<=>` es bidireccional. Dentro de la transición, `animate ()` especifica cuánto tiempo lleva la transición. En este caso, el cambio de estado de `open` a `closed` toma un segundo, expresado aquí como `1s`.

Este ejemplo agrega una transición de estado del estado `closed` al estado `open` con un arco de animación de transición de 0,5 segundos.

<code-example path="animations/src/app/open-close.component.ts" header="src/app/open-close.component.ts" language="typescript"
region="transition2">
</code-example>

<div class="alert is-helpful">

**Nota:** Algunas notas adicionales sobre el uso de estilos dentro de las funciones de `state` y `transition`.

* Use `state()` para definir los estilos que se aplican al final de cada transición, estos persisten después de que se completa la animación.

* Utilice `transition()` para definir estilos intermedios, que crean la ilusión de movimiento durante la animación.

* Cuando las animaciones están deshabilitadas, los estilos `transition()` se pueden omitir, pero los estilos `state()` no.

* Puede incluir varios pares de estados dentro del mismo argumento `transition()`:<br/> `transition( 'on => off, off => void' )`.
</div>

### Activando la animación

Una animación requiere un *disparador*, para que sepa cuándo comenzar. La función `trigger()` recopila los estados y las transiciones, y le da un nombre a la animación, para que pueda adjuntarlo al elemento desencadenante en la plantilla HTML.

La función `trigger()` describe el nombre de la propiedad para observar los cambios. Cuando ocurre un cambio, el disparador inicia las acciones incluidas en su definición. Estas acciones pueden ser transiciones u otras funciones, como veremos más adelante.

En este ejemplo, nombraremos el disparador `openClose` y lo adjuntaremos al elemento `button`. El disparador describe los estados open y closed, y los tiempos para las dos transiciones.

<div class="lightbox">
  <img src="generated/images/guide/animations/triggering-the-animation.png" alt="triggering the animation">
</div>

<div class="alert is-helpful">

**Nota:** Dentro de cada llamada a la función `trigger()`, un elemento solo puede estar en un estado en un momento dado. Sin embargo, es posible que varios desencadenantes estén activos a la vez.
</div>

### Definir animaciones y adjuntarlas a la plantilla HTML

Las animaciones se definen en los metadatos del componente que controla el elemento HTML a animar. Ponga el código que define sus animaciones debajo de la propiedad `animations:` dentro del decorador `@Component()`.

<code-example path="animations/src/app/open-close.component.ts" header="src/app/open-close.component.ts" language="typescript" region="component"></code-example>

Cuando haya definido un disparador de animación para un componente, puede adjuntarlo a un elemento en la plantilla de ese componente colocando el nombre del disparador entre paréntesis y precediéndolo con un símbolo `@`. Luego, puede vincular el disparador a una expresión de plantilla utilizando la sintaxis de enlace estandar en Angular como se muestra a continuación, Donde `triggerName` es el nombre del disparador, y `expression` evalúa un estado definido de la animación.

```
<div [@triggerName]="expression">...</div>;
```

La animación se ejecuta o activa cuando el valor de la expresión cambia a un nuevo estado.

El siguiente fragmento de código vincula el disparador con el el valor de la propiedad `isOpen`.

<code-example path="animations/src/app/open-close.component.1.html" header="src/app/open-close.component.html"
region="compare">
</code-example>

En este ejemplo, cuando la expresión `isOpen` evalúa el estado definido de `open` o `closed`, esto notifica al disparador `openClose` de un cambio de estado. Entonces depende del código `openClose` manejar el cambio de estado e iniciar una animación de cambio de estado.

Para los elementos que entran o salen de una página (insertados o eliminados del DOM), puede hacer que las animaciones sean condicionales. Por ejemplo, use `*ngIf` con el disparador de animación en la plantilla HTML.

<div class="alert is-helpful">

**Nota:** En el componente, establezca el disparador que define las animaciones como el valor de la propiedad `animations:` en el decorador `@Component ()`.

En el archivo de la plantilla HTML, usa el nombre del disparador para adjuntar las animaciones definidas al elemento HTML que se va a animar.

</div>

### Revisión de código

Aquí están los archivos de código tratados en el ejemplo de transición.

<code-tabs>

<code-pane header="src/app/open-close.component.ts" path="animations/src/app/open-close.component.ts" language="typescript"
region="component">
</code-pane>

<code-pane header="src/app/open-close.component.html" path="animations/src/app/open-close.component.1.html"
region="trigger">
</code-pane>

<code-pane header="src/app/open-close.component.css" path="animations/src/app/open-close.component.css">
</code-pane>

</code-tabs>

### Resumen

Aprendiste a agregar una animación a una transición simple entre dos estados, usando `style()` y `state()` junto con `animate()` para la sincronización.

Puedes aprender funciones de animaciones avanzada en Angular en la sección Animación, comenzando con técnicas avanzadas en [Transición y disparadores](guide/transition-and-triggers).

{@a animation-api-summary}
## Resumen de la API de animaciones
La API funcional proporcionada por el módulo `@angular/animations` provee un lenguaje de dominio específico (DSL) para crear y controlar animaciones en aplicaciones Angular. Mire la [API reference](api/animations) o una lista completa y detalles de sintaxis de las funciones principales y estructuras de datos relacionadas.

<table>

<tr>
<th style="vertical-align: top">
Nombre de la función
</th>

<th style="vertical-align: top">
¿Qué hace?
</th>
</tr>

<tr>
<td><code>trigger()</code></td>
<td>Inicia la animación y sirve como contenedor para todas las demás llamadas a funciones de animación. La plantilla HTML se une a <code>triggerName</code>. Utiliza el primer argumento para declarar un nombre de disparador único. Utiliza sintaxis de array.</td>
</tr>

<tr>
<td><code>style()</code></td>
<td>Define uno o más estilos CSS para usar en animaciones. Controla la apariencia visual de los elementos HTML durante las animaciones. Utiliza la sintaxis de Object</td>
</tr>

<tr>
<td><code><a href="api/animations/state" class="code-anchor">state()</a></code></td>
<td>Crea un conjunto con nombre de estilos CSS que se deben aplicar en una transición exitosa a un estado dado. A continuación, se puede hacer referencia al estado por su nombre dentro de otras funciones de animación.</td>
</tr>

<tr>
<td><code>animate()</code></td>
<td>Especifica la información de tiempo para una transición. Valores opcionales para <code>delay</code> y <code>easing</code>. Puede contener llamadas a <code>style()</code> dentro.</td>
</tr>

<tr>
<td><code>transition()</code></td>
<td>Define la secuencia de animación entre dos estados con nombre. Utiliza sintaxis de array</td>
</tr>

<tr>
<td><code>keyframes()</code></td>
<td>Permite un cambio secuencial entre estilos dentro de un intervalo de tiempo específico. Usa dentro <code>animate()</code>. Puede incluir dentro múltiples llamadas <code>style()</code> dentro de cada <code>keyframe()</code>. Usa la sintasis de array.</td>
</tr>

<tr>
<td><code><a href="api/animations/group" class="code-anchor">group()</a></code></td>
<td>Especifica un grupo de animaciones internas de pasos (<em>de animación</em>) que se ejecutarán en paralelo. La animación continúa solo después de que se hayan completado todos los pasos de la animación interna. Usado dentro de <code>sequence()</code> o <code>transition().</code></td>
</tr>

<tr>
<td><code>query()</code></td>
<td>Úselo para buscar uno o más elementos HTML internos dentro del elemento actual.</td>
</tr>

<tr>
<td><code>sequence()</code></td>
<td>Especifica una lista de pasos de animación que se ejecutan secuencialmente, uno por uno.</td>
</tr>

<tr>
<td><code>stagger()</code></td>
<td>Escalona la hora de inicio de las animaciones de varios elementos.</td>
</tr>

<tr>
<td><code>animation()</code></td>
<td>Produce una animación reutilizable que se puede invocar desde otro lugar. Usado junto con <code>useAnimation()</code>.</td>
</tr>

<tr>
<td><code>useAnimation()</code></td>
<td>Activa una animación reutilizable. Usado con <code>animation()</code>.</td>
</tr>

<tr>
<td><code>animateChild()</code></td>
<td>Permite que las animaciones de los componentes secundarios se ejecuten dentro del mismo período de tiempo que el principal.</td>
</tr>

</table>

## Más sobre animaciones Angular

También te puede interesar lo siguiente:

* [Transición y disparadores](guide/transition-and-triggers)
* [Secuencias de animación complejas](guide/complex-animation-sequences)
* [Animaciones reutilizables](guide/reusable-animations)
* [Animaciones de transición de ruta](guide/route-animations)

<div class="alert is-helpful">

Mira esta animación completa [demo](http://animationsftw.in/#/) junto con la [presentación](https://www.youtube.com/watch?v=JhNo3Wvj6UQ&funcionalidad=youtu.be&t=2h47m53s), mostrado en la conferencia AngularConnect en noviembre de 2017.
</div>
