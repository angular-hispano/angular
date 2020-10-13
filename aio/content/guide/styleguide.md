# Guía de estilo de codificación angular

¿Busca una guía obstinada sobre la sintaxis, las convenciones y la estructura de la aplicación de Angular?
¡Entra directamente!
Esta guía de estilo presenta las convenciones preferidas y, lo que es más importante, explica por qué.

{@a toc}

## Vocabulario de estilo

Cada guía describe una buena o una mala práctica, y todas tienen una presentación coherente.

La redacción de cada directriz indica qué tan fuerte es la recomendación.

<div class="s-rule do">

**Realizar:** es uno que siempre debe seguirse.
_Siempre_ puede ser una palabra demasiado fuerte.
Las pautas que deben seguirse literalmente siempre son extremadamente raras.
Por otro lado, necesita un caso realmente inusual para romper una pauta de _Realizar_

</div>

<div class="s-rule consider">

**Considerar:** que se deben seguir las pautas.
Si comprende completamente el significado detrás de la directriz y tiene una buena razón para desviarse, hágalo. Por favor, esfuércese por ser coherente.

</div>

<div class="s-rule avoid">

**Evitar:** indica algo que casi nunca debes hacer. Los ejemplos de código que se deben _evitar_ tienen un encabezado rojo inconfundible.

</div>

<div class="s-why">

**¿Por qué?:** da razones para seguir las recomendaciones anteriores.

</div>

## Convenciones de estructuras de archivos

Algunos ejemplos de código muestran un archivo que tiene uno o más archivos complementarios con nombres similares.
Por ejemplo, `hero.component.ts` y `hero.component.html`.

La guía usa atajos `hero.component.ts|html|css|spec` para representar esos diversos archivos. El uso de este acceso directo hace que las estructuras de archivos de esta guía sean más fáciles de leer y concisas.

{@a single-responsibility}

## Responsabilidad única

Aplica el
<a href="https://wikipedia.org/wiki/Single_responsibility_principle"><i>principio de responsabilidad única</i> (SRP)</a> a todos los componentes, servicios y otros símbolos.
Esto ayuda a que la aplicación sea más limpia, más fácil de leer y mantener y más comprobable.

{@a 01-01}

### Regla de uno

#### Estilo 01-01

<div class="s-rule do">

**Realizar:** defina una cosa, como un servicio o componente, por archivo.

</div>

<div class="s-rule consider">

**Considerar:** limitando archivos a 400 líneas de código.

</div>

<div class="s-why">

**¿Por qué?:** Un componente por archivo hace que sea mucho más fácil de leer, mantener y evitar colisiones con equipos de control de fuentes.

</div>

<div class="s-why">

**¿Por qué?:** Un componente por archivo evita errores ocultos que a menudo surgen al combinar componentes en un archivo donde pueden compartir variables, crear cierres no deseados o acoplamientos no deseados con dependencias.

</div>

<div class="s-why-last">

**¿Por qué?:** Un solo componente puede ser la exportación predeterminada para su archivo, lo que facilita la carga diferida con el enrutador.

</div>

La clave es hacer que el código sea más reutilizable, más fácil de leer y menos propenso a errores.

El siguiente ejemplo _negativo_ define el `AppComponent`, inicia la aplicación, define el objeto modelo `Hero` y carga héroes desde el servicio en el mismo archivo.
_No hagas esto_.

<code-example path="styleguide/src/01-01/app/heroes/hero.component.avoid.ts" header="app/heroes/hero.component.ts">

</code-example>

Es una mejor práctica redistribuir el componente y su
clases de apoyo en sus propios archivos dedicados.

<code-tabs>

  <code-pane header="main.ts" path="styleguide/src/01-01/main.ts">

  </code-pane>

  <code-pane header="app/app.module.ts" path="styleguide/src/01-01/app/app.module.ts">

  </code-pane>

  <code-pane header="app/app.component.ts" path="styleguide/src/01-01/app/app.component.ts">

  </code-pane>

  <code-pane header="app/heroes/heroes.component.ts" path="styleguide/src/01-01/app/heroes/heroes.component.ts">

  </code-pane>

  <code-pane header="app/heroes/shared/hero.service.ts" path="styleguide/src/01-01/app/heroes/shared/hero.service.ts">

  </code-pane>

  <code-pane header="app/heroes/shared/hero.model.ts" path="styleguide/src/01-01/app/heroes/shared/hero.model.ts">

  </code-pane>

  <code-pane header="app/heroes/shared/mock-heroes.ts" path="styleguide/src/01-01/app/heroes/shared/mock-heroes.ts">

  </code-pane>

</code-tabs>

A medida que la aplicación crece, esta regla se vuelve aún más importante.
<a href="#toc">Volver arriba</a>

{@a 01-02}

### Pequeñas funciones

#### Estilo 01-02

<div class="s-rule do">

**Realizar:** definir pequeñas funciones

</div>

<div class="s-rule consider">

**Considerar:** limitando a no más de 75 líneas.

</div>

<div class="s-why">

**¿Por qué?:** Las funciones pequeñas son más fáciles de probar, especialmente cuando hacen una cosa y tienen un propósito.

</div>

<div class="s-why">

**¿Por qué?:** Las pequeñas funciones promueven la reutilización.

</div>

<div class="s-why">

**¿Por qué?:** Las funciones pequeñas son más fáciles de leer.

</div>

<div class="s-why">

**¿Por qué?:** Las funciones pequeñas son más fáciles de mantener.

</div>

<div class="s-why-last">

**¿Por qué?:** Las funciones pequeñas ayudan a evitar errores ocultos que vienen con funciones grandes que comparten variables con alcance externo, crean cierres no deseados o acoplamientos no deseados con dependencias.

</div>

<a href="#toc">Volver arriba</a>

## Nombrar

Las convenciones de nomenclatura son muy importantes para la facilidad de mantenimiento y la legibilidad. Esta guía recomienda convenciones de nomenclatura para el nombre del archivo y el nombre del símbolo.

{@a 02-01}

### Pautas generales de nomenclatura

#### Estilo 02-01

<div class="s-rule do">

**Realizar:** utilizar nombres consistentes para todos los símbolos.

</div>

<div class="s-rule do">

**Realizar:** siga un patrón que describa la característica de los símbolos y luego su tipo. El patrón recomendado es `feature.type.ts`.

</div>

<div class="s-why">

**¿Por qué?:** Las convenciones de nomenclatura ayudan a proporcionar una forma coherente de buscar contenido de un vistazo. La coherencia dentro del proyecto es vital. La coherencia con un equipo es importante. La coherencia en una empresa proporciona una eficiencia tremenda.

</div>

<div class="s-why">

**¿Por qué?:** Las convenciones de nomenclatura deberían simplemente ayudar a encontrar el código deseado más rápido y hacerlo más fácil de entender.

</div>

<div class="s-why-last">

**¿Por qué?:** Los nombres de carpetas y archivos deben transmitir claramente su intención. Por ejemplo, `app/heroes/hero-list.component.ts` puede contener un componente que gestiona una lista de héroes.

</div>

<a href="#toc">Volver arriba</a>

{@a 02-02}

### Separe los nombres de los archivos con puntos y guiones.

#### Estilo 02-02

<div class="s-rule do">

**Realizar:** utilice guiones para separar palabras en el nombre descriptivo.

</div>

<div class="s-rule do">

**Realizar:** use puntos para separar el nombre descriptivo del tipo.

</div>

<div class="s-rule do">

**Realizar:** utilice nombres de tipo coherentes para todos los componentes siguiendo un patrón que describa la característica de los componentes y luego su tipo. Un patrón recomendado es `feature.type.ts`.

</div>

<div class="s-rule do">

**Realizar:** utilice nombres de los tipos convencionales incluyendo `.service`, `.component`, `.pipe`, `.module`, y `.directive`.
Invente nombres de tipos adicionales si es necesario, pero tenga cuidado de no crear demasiados.

</div>

<div class="s-why">

**¿Por qué?:** Los nombres de tipo proporcionan una forma coherente de identificar rápidamente lo que hay en el archivo.

</div>

<div class="s-why">

**¿Por qué?:** Los nombres de los tipos facilitan la búsqueda de un tipo de archivo específico mediante un editor o técnicas de búsqueda difusa IDE.

</div>

<div class="s-why">

**¿Por qué?:** Nombres de tipos no abreviados como `.service` son descriptivos e inequívocos.
Abreviaturas como `.srv`, `.svc`, y `.serv` puede ser confuso.

</div>

<div class="s-why-last">

**¿Por qué?:** Los nombres de tipo proporcionan una coincidencia de patrones para cualquier tarea automatizada.

</div>

<a href="#toc">Volver arriba</a>

{@a 02-03}

### Símbolos y nombres de archivos

#### Estilo 02-03

<div class="s-rule do">

**Realizar:** use nombres consistentes para todos los activos con el nombre de lo que representan.

</div>

<div class="s-rule do">

**Realizar:** use mayúsculas/minúsculas en frases o palabras compuestas para los nombres de las clases.

</div>

<div class="s-rule do">

**Realizar:** Haga coincidir el nombre del símbolo con el nombre del archivo.

</div>

<div class="s-rule do">

**Realizar:** agregue el nombre del símbolo con el sufijo convencional (como `Component`,
`Directive`, `Module`, `Pipe`, o `Service`) por algo de ese tipo.

</div>

<div class="s-rule do">

**Realizar:** dar al nombre de archivo el sufijo convencional (como `.component.ts`, `.directive.ts`,
`.module.ts`, `.pipe.ts`, or `.service.ts`) para un archivo de ese tipo.

</div>

<div class="s-why">

**¿Por qué?:** Las convenciones coherentes facilitan la identificación rápida y activos de referencia de diferentes tipos.

</div>

<table width="100%">

  <col width="50%">

  </col>

  <col width="50%">

  </col>

  <tr>

    <th>
      Nombre del símbolo
    </th>

    <th>
      Nombre del archivo
    </th>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Component({ ... })
        export class AppComponent { }
      </code-example>

    </td>

    <td>
      app.component.ts
    </td>

  </tr>

  <tr style=top>

    <td>
      <code-example hideCopy class="no-box">
        @Component({ ... })
        export class HeroesComponent { }
      </code-example>

    </td>

    <td>
      heroes.component.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Component({ ... })
        export class HeroListComponent { }
      </code-example>

    </td>

    <td>
      hero-list.component.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Component({ ... })
        export class HeroDetailComponent { }
      </code-example>

    </td>

    <td>
      hero-detail.component.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Directive({ ... })
        export class ValidationDirective { }
      </code-example>

    </td>

    <td>
      validation.directive.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @NgModule({ ... })
        export class AppModule
      </code-example>

    </td>

    <td>
      app.module.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Pipe({ name: 'initCaps' })
        export class InitCapsPipe implements PipeTransform { }
      </code-example>

    </td>

    <td>
      init-caps.pipe.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Injectable()
        export class UserProfileService { }
      </code-example>

    </td>

    <td>
      user-profile.service.ts
    </td>

  </tr>

</table>

<a href="#toc">Volver arriba</a>

{@a 02-04}

### Nombres de servicios

#### Estilo 02-04

<div class="s-rule do">

**Realizar:** use nombres consistentes para todos los servicios nombrados según su función.

</div>

<div class="s-rule do">

**Realizar:** sufijo un nombre de clase de servicio con `Service`.
Por ejemplo, algo que obtiene datos o héroes.
debería llamarse un `DataService` o un `HeroService`.

Algunos términos son inequívocamente servicios. Normalmente
indique la agencia terminando en "-er". Puede preferir nombrar
un servicio que registra mensajes `Logger` más bien que `LoggerService`.
Decida si esta excepción es aceptable en su proyecto.
Como siempre, esfuércese por la coherencia.

</div>

<div class="s-why">

**¿Por qué?:** Proporciona una forma coherente de identificar y hacer referencia a los servicios rápidamente.

</div>

<div class="s-why">

**¿Por qué?:** Nombres de servicios claros como `Logger` no requiere un sufijo.

</div>

<div class="s-why-last">

**¿Por qué?:** Nombres de servicios como `Credit` son sustantivos y requieren un sufijo y deben nombrarse con un sufijo cuando no es obvio si se trata de un servicio o de otra cosa.

</div>

<table width="100%">

  <col width="50%">

  </col>

  <col width="50%">

  </col>

  <tr>

    <th>
      Nombre del símbolo
    </th>

    <th>
      Nombre del archivo
    </th>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Injectable()
        export class HeroDataService { }
      </code-example>

    </td>

    <td>
      hero-data.service.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Injectable()
        export class CreditService { }
      </code-example>

    </td>

    <td>
      credit.service.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Injectable()
        export class Logger { }
      </code-example>

    </td>

    <td>
      logger.service.ts
    </td>

  </tr>

</table>

<a href="#toc">Volver arriba</a>

{@a 02-05}

### Bootstrapping

#### Estilo 02-05

<div class="s-rule do">

**Realizar:** poner bootstrapping y lógica de plataforma para la aplicación en un archivo llamado `main.ts`.

</div>

<div class="s-rule do">

**Realizar:** incluir el manejo de errores en la lógica de arranque.

</div>

<div class="s-rule avoid">

**Evitar:** incluir la lógica de la aplicación en `main.ts`. En su lugar, considere colocarlo en un componente o servicio.

</div>

<div class="s-why">

**¿Por qué?:** Sigue una convención coherente para la lógica de inicio de una aplicación.

</div>

<div class="s-why-last">

**¿Por qué?:** Sigue una convención familiar de otras plataformas tecnológicas.

</div>

<code-example path="styleguide/src/02-05/main.ts" header="main.ts">

</code-example>

<a href="#toc">Volver arriba</a>

{@a 05-02}

### Selectores de componentes

#### Estilo 05-02

<div class="s-rule do">

**Realizar:** use _dashed-case_ o _kebab-case_ para nombrar los selectores de elementos de los componentes.

</div>

<div class="s-why-last">

**¿Por qué?:** Mantiene los nombres de los elementos coherentes con la especificación de [Elementos personalizados](https://www.w3.org/TR/custom-elements/).

</div>

<code-example path="styleguide/src/05-02/app/heroes/shared/hero-button/hero-button.component.avoid.ts" region="example" header="app/heroes/shared/hero-button/hero-button.component.ts">

</code-example>

<code-tabs>

  <code-pane header="app/heroes/shared/hero-button/hero-button.component.ts" path="styleguide/src/05-02/app/heroes/shared/hero-button/hero-button.component.ts" region="example">

  </code-pane>

  <code-pane header="app/app.component.html" path="styleguide/src/05-02/app/app.component.html">

  </code-pane>

</code-tabs>

<a href="#toc">Volver arriba</a>

{@a 02-07}

### Prefijo personalizado de componente

#### Estilo 02-07

<div class="s-rule do">

**Realizar:** utilice un valor selector de elementos en minúsculas y con guión; por ejemplo, `admin-users`.

</div>

<div class="s-rule do">

**Realizar:** utilice un prefijo personalizado para un selector de componentes.
Por ejemplo, el prefijo `toh` representa **T**our **o**f **H**eroes y el prefijo `admin` representa un área de funciones de administración.

</div>

<div class="s-rule do">

**Realizar:** utilice un prefijo que identifique el área de funciones o la propia aplicación.

</div>

<div class="s-why">

**¿Por qué?:** Evita las colisiones de nombres de elementos con componentes de otras aplicaciones y con elementos HTML nativos.

</div>

<div class="s-why">

**¿Por qué?:** Hace que sea más fácil promocionar y compartir el componente en otras aplicaciones.

</div>

<div class="s-why-last">

**¿Por qué?:** Los componentes son fáciles de identificar en el DOM.

</div>

<code-example path="styleguide/src/02-07/app/heroes/hero.component.avoid.ts" region="example" header="app/heroes/hero.component.ts">

</code-example>

<code-example path="styleguide/src/02-07/app/users/users.component.avoid.ts" region="example" header="app/users/users.component.ts">

</code-example>

<code-example path="styleguide/src/02-07/app/heroes/hero.component.ts" region="example" header="app/heroes/hero.component.ts">

</code-example>

<code-example path="styleguide/src/02-07/app/users/users.component.ts" region="example" header="app/users/users.component.ts">

</code-example>

<a href="#toc">Volver arriba</a>

{@a 02-06}

### Selectores de directivas

#### Estilo 02-06

<div class="s-rule do">

**Realizar:** Utilice mayúsculas y minúsculas para nombrar los selectores de directivas.

</div>

<div class="s-why">

**¿Por qué?:** Mantiene los nombres de las propiedades definidas en las directivas que están vinculadas a la vista de acuerdo con los nombres de los atributos.

</div>

<div class="s-why-last">

**¿Por qué?:** El analizador de HTML angular distingue entre mayúsculas y minúsculas y reconoce las minúsculas en camel.

</div>

<a href="#toc">Volver arriba</a>

{@a 02-08}

### Prefijo personalizado de directiva

#### Estilo 02-08

<div class="s-rule do">

**Realizar:** utilizar un prefijo personalizado para el selector de directivas (por ejemplo, el prefijo `toh` desde **T**our **o**f **H**eroes).

</div>

<div class="s-rule do">

**Realizar:** deletrear selectores no de elementos en mayúsculas y minúsculas menos que el selector está destinado a coincidir con un atributo HTML nativo.

</div>

<div class="s-why">

**¿Por qué?:** Evita colisiones de nombres.

</div>

<div class="s-why-last">

**¿Por qué?:** Las directivas se identifican fácilmente.

</div>

<code-example path="styleguide/src/02-08/app/shared/validate.directive.avoid.ts" region="example" header="app/shared/validate.directive.ts">

</code-example>

<code-example path="styleguide/src/02-08/app/shared/validate.directive.ts" region="example" header="app/shared/validate.directive.ts">

</code-example>

<a href="#toc">Volver arriba</a>

{@a 02-09}

### Nombre de pipes

#### Estilo 02-09

<div class="s-rule do">

**Realizar:** Utilizar nombres consistentes para todos los pipes, llamados así por su función.
El nombre de la clase de pipe debe usar [UpperCamelCase](guide/glossary#case-types)
(la convención general para los nombres de clases),
y el correspondiente `name` string debería usar _lowerCamelCase_.
EL `name` string no puede usar guiones ("dash-case" or "kebab-case").

</div>

<div class="s-why-last">

**¿Por qué?:** Proporciona una forma coherente de identificar y hacer referencia a los pipes rápidamente.

</div>

<table width="100%">

  <col width="50%">

  </col>

  <col width="50%">

  </col>

  <tr>

    <th>
      Nombre del símbolo
    </th>

    <th>
      Nombre del archivo
    </th>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Pipe({ name: 'ellipsis' })
        export class EllipsisPipe implements PipeTransform { }
      </code-example>

    </td>

    <td>
      ellipsis.pipe.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @Pipe({ name: 'initCaps' })
        export class InitCapsPipe implements PipeTransform { }
      </code-example>

    </td>

    <td>
      init-caps.pipe.ts
    </td>

  </tr>

</table>

<a href="#toc">Volver arriba</a>

{@a 02-10}

### Nombres de archivos de prueba unitaria

#### Estilo 02-10

<div class="s-rule do">

**Realizar:** nombrar los archivos de especificación de prueba de la misma manera que el componente que prueban.

</div>

<div class="s-rule do">

**Realizar:** nombrar archivos de especificación de prueba con un sufijo de `.spec`.

</div>

<div class="s-why">

**¿Por qué?:** Proporciona una forma coherente de identificar pruebas rápidamente.

</div>

<div class="s-why-last">

**¿Por qué?:** Proporciona coincidencia de patrones para [karma](http://karma-runner.github.io/) o otros corredores de prueba.

</div>

<table width="100%">

  <col width="50%">

  </col>

  <col width="50%">

  </col>

  <tr>

    <th>
      Tipo de prueba
    </th>

    <th>
      Nombres de archivos
    </th>

  </tr>

  <tr style=top>

    <td>
      Componentes
    </td>

    <td>
      heroes.component.spec.ts

      hero-list.component.spec.ts

      hero-detail.component.spec.ts
    </td>

  </tr>

  <tr style=top>

    <td>
      Servicios
    </td>

    <td>
      logger.service.spec.ts

      hero.service.spec.ts

      filter-text.service.spec.ts
    </td>

  </tr>

  <tr style=top>

    <td>
      Pipes
    </td>

    <td>
      ellipsis.pipe.spec.ts
      init-caps.pipe.spec.ts
    </td>

  </tr>

</table>

<a href="#toc">Volver arriba</a>

{@a 02-11}

### Nombres de archivos de puebas _End-to-End_ (E2E)

#### Estilo 02-11

<div class="s-rule do">

**Realizar:** denominar los archivos de especificación de prueba de extremo a extremo después de la función que ponen a prueba con el sufijo `.e2e-spec`.

</div>

<div class="s-why">

**¿Por qué?:** Proporciona una forma coherente de identificar rápidamente las pruebas de un extremo a otro.

</div>

<div class="s-why-last">

**¿Por qué?:** Proporciona coincidencia de patrones para los corredores de pruebas y la automatización de compilaciones.

</div>

<table width="100%">

  <col width="50%">

  </col>

  <col width="50%">

  </col>

  <tr>

    <th>
      Tipo de prueba
    </th>

    <th>
      Nombres de archivos
    </th>

  </tr>

  <tr style=top>

    <td>
      End-to-End Tests
    </td>

    <td>
      app.e2e-spec.ts
      heroes.e2e-spec.ts
    </td>

  </tr>

</table>

<a href="#toc">Volver arriba</a>

{@a 02-12}

### Angular _NgModule_ nombres

#### Estilo 02-12

<div class="s-rule do">

**Realizar:** añadir el nombre del símbolo con el sufijo `Module`.

</div>

<div class="s-rule do">

**Realizar:** dar el nombre del archivo `.module.ts` extensión.

</div>

<div class="s-rule do">

**Realizar:** el módulo según la función y la carpeta en la que reside.

</div>

<div class="s-why">

**¿Por qué?:** Proporciona una forma coherente de identificar y hacer referencia a los módulos rápidamente.

</div>

<div class="s-why">

**¿Por qué?:** El caso de mayúsculas/minúsculas superior es convencional para identificar objetos que se pueden instanciar usando un constructor.

</div>

<div class="s-why-last">

**¿Por qué?:** Identifica fácilmente el módulo como la raíz de la misma función nombrada.

</div>

<div class="s-rule do">

**Realizar:** sufijo a _RoutingModule_ nombre de clase con `RoutingModule`.

</div>

<div class="s-rule do">

**Realizar:** termina el nombre del archivo de una _RoutingModule_ con `-routing.module.ts`.

</div>

<div class="s-why-last">

**¿Por qué?:** Un `RoutingModule` es un módulo dedicado exclusivamente a configurar el enrutador angular. Una convención consistente de clases y nombres de archivos hace que estos módulos sean fáciles de detectar y verificar.

</div>

<table width="100%">

  <col width="50%">

  </col>

  <col width="50%">

  </col>

  <tr>

    <th>
      Nombre del símbolo
    </th>

    <th>
      Nombre del archivo
    </th>

  </tr>

  <tr style=top>

    <td>
      <code-example hideCopy class="no-box">
        @NgModule({ ... })
        export class AppModule { }
      </code-example>
    </td>

    <td>
      app.module.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @NgModule({ ... })
        export class HeroesModule { }
      </code-example>

    </td>

    <td>
      heroes.module.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @NgModule({ ... })
        export class VillainsModule { }
      </code-example>

    </td>

    <td>
      villains.module.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @NgModule({ ... })
        export class AppRoutingModule { }
      </code-example>

    </td>

    <td>
      app-routing.module.ts
    </td>

  </tr>

  <tr style=top>

    <td>

      <code-example hideCopy class="no-box">
        @NgModule({ ... })
        export class HeroesRoutingModule { }
      </code-example>

    </td>

    <td>
      heroes-routing.module.ts
    </td>

  </tr>

</table>

<a href="#toc">Volver arriba</a>

## Estructura de la aplicación y NgModules

Tener una visión a corto plazo de la implementación y una visión a largo plazo. Comience con algo pequeño, pero tenga en cuenta hacia dónde se dirige la aplicación en el futuro.

Todo el código de la aplicación va en una carpeta llamada "src".
Todas las áreas de características están en su propia carpeta, con su propio NgModule.

Todo el contenido es un activo por archivo. Cada componente, servicio y canalización está en su propio archivo.
Todos los scripts de terceros se almacenan en otra carpeta y no en la carpeta `src`.
No los escribiste y no quieres que abarroten `src`.
Utilice las convenciones de nomenclatura para los archivos de esta guía.

<a href="#toc">Volver arriba</a>

{@a 04-01}

### _LIFT_

#### Estilo 04-01

<div class="s-rule do">

**Realizar:** estructura la aplicación de modo que puedas **L**ocalizar el código rápidamente.
**I**dentificar el código de un vistazo,
mantener la estructura más **P**lana que puedas, y
**T**rate de estar DRY(Don't Repeat Yourself).

</div>

<div class="s-rule do">

**Realizar:** definir la estructura para seguir estas cuatro pautas básicas, enumeradas en orden de importancia.

</div>

<div class="s-why-last">

**¿Por qué?:** LIFT (Locate Identify Flattest Try) proporciona una estructura consistente que escala bien, es modular y facilita el aumento de la eficiencia del desarrollador al encontrar código rápidamente.
Para confirmar su intuición sobre una estructura en particular, pregunte:
¿_Puedo abrir rápidamente y comenzar a trabajar en todos los archivos relacionados para esta función_?

</div>

<a href="#toc">Volver arriba</a>

{@a 04-02}

### Localizar

#### Estilo 04-02

<div class="s-rule do">

**Realizar:** hacer que la localización de código sea intuitiva, simple y rápida.

</div>

<div class="s-why-last">

**¿Por qué?:** Para trabajar de manera eficiente, debe poder encontrar archivos rápidamente,
especialmente cuando no conoce (o no recuerda) el archivo _names_.
Mantener los archivos relacionados cerca unos de otros en una ubicación intuitiva ahorra tiempo.
Una estructura de carpetas descriptiva hace una gran diferencia para usted y las personas que vienen después de usted.

</div>

<a href="#toc">Volver arriba</a>

{@a 04-03}

### Identificar

#### Estilo 04-03

<div class="s-rule do">

**Realizar:** designar el archivo de modo que sepa instantáneamente lo que contiene y representa.

</div>

<div class="s-rule do">

**Realizar:** Sea descriptivo con los nombres de archivo y mantenga el contenido del archivo exactamente en un componente.

</div>

<div class="s-rule avoid">

**Evitar:** archivos con varios componentes, varios servicios o una combinación.

</div>

<div class="s-why-last">

**¿Por qué?:** Dedique menos tiempo a buscar y escarbar código, y sea más eficiente.
Los nombres de archivo más largos son mucho mejores que _short-but-obscure_ nombres abreviados.

</div>

<div class="alert is-helpful">

Puede ser ventajoso desviarse de la _one-thing-per-file_ regla cuando
tiene un conjunto de características pequeñas estrechamente relacionadas que se descubren y comprenden mejor
en un solo archivo que como varios archivos. Tenga cuidado con esta laguna.

</div>

<a href="#toc">Volver arriba</a>

{@a 04-04}

### Plano

#### Estilo 04-04

<div class="s-rule do">

**Realizar:** mantenga una estructura de carpetas plana el mayor tiempo posible.

</div>

<div class="s-rule consider">

**Considerar:** creando subcarpetas cuando una carpeta alcanza siete o más archivos.

</div>

<div class="s-rule consider">

**Considerar:** configurar el IDE para ocultar archivos irrelevantes que distraigan, como archivos generados `.js` y` .js.map`.

</div>

<div class="s-why-last">

**¿Por qué?:** Nadie quiere buscar un archivo en siete niveles de carpetas.
Una estructura plana es fácil de escanear.

Por otra parte,
<a href="https://en.wikipedia.org/wiki/The_Magical_Number_Seven,_Plus_or_Minus_Two">Los psicólogos creen</a>
que los humanos comienzan a luchar cuando el número de cosas interesantes adyacentes supera las nueve.
Entonces, cuando una carpeta tiene diez o más archivos, puede ser el momento de crear subcarpetas.

Base su decisión en su nivel de comodidad.
Utilice una estructura más plana hasta que haya un valor obvio para crear una nueva carpeta.

</div>

<a href="#toc">Volver arriba</a>

{@a 04-05}

### _T-DRY_ (Tratar de estar _DRY_)

#### Estilo 04-05

<div class="s-rule do">

**Realizar:** ser DRY (Don't Repeat Yourself).

</div>

<div class="s-rule avoid">

**Evitar:** la información no debe repetirse puesto que sacrifica la legibilidad.

</div>

<div class="s-why-last">

**¿Por qué?:** Estar DRY es importante, pero no crucial si sacrifica los otros elementos de LIFT.
Por eso se llama _T-DRY_.
Por ejemplo, es redundante nombrar una plantilla `hero-view.component.html` porque con la extensión `.html`, obviamente es una vista.
Pero si algo no es obvio o se aparta de una convención, escríbalo.

</div>

<a href="#toc">Volver arriba</a>

{@a 04-06}

### Directrices estructurales generales

#### Estilo 04-06

<div class="s-rule do">

**Realizar:** comience poco a poco, pero tenga en cuenta hacia dónde se dirige la aplicación en el futuro.

</div>

<div class="s-rule do">

**Realizar:** tener una visión a corto plazo de la implementación y una visión a largo plazo.

</div>

<div class="s-rule do">

**Realizar:** poner todo el código de la aplicación en una carpeta llamada `src`.

</div>

<div class="s-rule consider">

**Considerar:** crear una carpeta para un componente cuando tiene varios archivos adjuntos (`.ts`,` .html`, `.css` y` .spec`).

</div>

<div class="s-why">

**¿Por qué?:** Ayuda a mantener la estructura de la aplicación pequeña y fácil de mantener en las primeras etapas, mientras que es fácil de evolucionar a medida que la aplicación crece.

</div>

<div class="s-why-last">

**¿Por qué?:** Los componentes suelen tener cuatro archivos (p. Ej., `* .Html`,` * .css`, `* .ts` y` * .spec.ts`) y pueden saturar una carpeta rápidamente.

</div>

{@a file-tree}

Aquí hay una carpeta y una estructura de archivos compatibles:

<div class='filetree'>

  <div class='file'>
    &lt;raíz del proyecto&gt;
  </div>

  <div class='children'>

    <div class='file'>
      src
    </div>

    <div class='children'>

      <div class='file'>
        app
      </div>

      <div class='children'>

        <div class='file'>
          core
        </div>

        <div class='children'>

          <div class='file'>
            exception.service.ts|spec.ts
          </div>

          <div class='file'>
            user-profile.service.ts|spec.ts
          </div>

        </div>

        <div class='file'>
          heroes
        </div>

        <div class='children'>

          <div class='file'>
            hero
          </div>

          <div class='children'>

            <div class='file'>
              hero.component.ts|html|css|spec.ts
            </div>

          </div>

          <div class='file'>
            hero-list
          </div>

          <div class='children'>

            <div class='file'>
              hero-list.component.ts|html|css|spec.ts
            </div>

          </div>

          <div class='file'>
            shared
          </div>

          <div class='children'>

            <div class='file'>
              hero-button.component.ts|html|css|spec.ts
            </div>

            <div class='file'>
              hero.model.ts
            </div>

            <div class='file'>
              hero.service.ts|spec.ts
            </div>

          </div>

          <div class='file'>
            heroes.component.ts|html|css|spec.ts
          </div>

          <div class='file'>
            heroes.module.ts
          </div>

          <div class='file'>
            heroes-routing.module.ts
          </div>

        </div>

        <div class='file'>
          shared
        </div>

        <div class='children'>

          <div class='file'>
            shared.module.ts
          </div>

          <div class='file'>
            init-caps.pipe.ts|spec.ts
          </div>

          <div class='file'>
            filter-text.component.ts|spec.ts
          </div>

          <div class='file'>
            filter-text.service.ts|spec.ts
          </div>

        </div>

        <div class='file'>
          villains
        </div>

        <div class='children'>

          <div class='file'>
            villain
          </div>

          <div class='children'>

            <div class='file'>
              ...
            </div>

          </div>

          <div class='file'>
            villain-list
          </div>

          <div class='children'>

            <div class='file'>
              ...
            </div>

          </div>

          <div class='file'>
            shared
          </div>

          <div class='children'>

            <div class='file'>
              ...
            </div>

          </div>

          <div class='file'>
            villains.component.ts|html|css|spec.ts
          </div>

          <div class='file'>
            villains.module.ts
          </div>

          <div class='file'>
            villains-routing.module.ts
          </div>

        </div>

        <div class='file'>
          app.component.ts|html|css|spec.ts
        </div>

        <div class='file'>
          app.module.ts
        </div>

        <div class='file'>
          app-routing.module.ts
        </div>

      </div>

      <div class='file'>
        main.ts
      </div>

      <div class='file'>
        index.html
      </div>

      <div class='file'>
        ...
      </div>

    </div>

    <div class='file'>
      node_modules/...
    </div>

    <div class='file'>
      ...
    </div>

  </div>

</div>

<div class="alert is-helpful">

Si bien los componentes en carpetas dedicadas son los más preferidos,
otra opción para aplicaciones pequeñas es mantener los componentes planos (no en una carpeta dedicada).
Esto agrega hasta cuatro archivos a la carpeta existente, pero también reduce el anidamiento de la carpeta.
Sea lo que sea que elija, sea coherente.

</div>

<a href="#toc">Volver arriba</a>

{@a 04-07}

### _Folders-by-feature_ estructura

#### Estilo 04-07

<div class="s-rule do">

**Realizar:** crear carpetas con el nombre del área de funciones que representan.

</div>

<div class="s-why">

**¿Por qué?:** Un desarrollador puede localizar el código e identificar lo que representa cada archivo
de un vistazo. La estructura es tan plana como puede ser y no hay nombres repetitivos o redundantes.

</div>

<div class="s-why">

**¿Por qué?:** Todas las pautas LIFT están cubiertas.

</div>

<div class="s-why">

**¿Por qué?:** Ayuda a evitar que la aplicación se abarrote al organizar la
contenido y manteniéndolos alineados con las pautas de LIFT.

</div>

<div class="s-why">

**¿Por qué?:** Cuando hay muchos archivos, por ejemplo 10+,
localizarlos es más fácil con una estructura de carpetas consistente
y más difícil en una estructura plana.

</div>

<div class="s-rule do">

**Realizar:** cree un NgModule para cada área de características.

</div>

<div class="s-why">

**¿Por qué?:** NgModules facilita la carga diferida de funciones enrutables.

</div>

<div class="s-why-last">

**¿Por qué?:** NgModules facilita el aislamiento, prueba y reutilización de funciones.

</div>

<div>

Para obtener más información, consulte <a href="#file-tree"> este ejemplo de estructura de carpetas y archivos.</a>

</div>

<a href="#toc">Volver arriba

</a>

{@a 04-08}

### App _root module_

#### Estilo 04-08

<div class="s-rule do">

**Realizar:** crea un NgModule en la carpeta raíz de la aplicación,
por ejemplo, en`/src/app`.

</div>

<div class="s-why">

**¿Por qué?:** Cada aplicación requiere al menos un NgModule raíz.

</div>

<div class="s-rule consider">

**Considerar:** nombrar el módulo raíz `app.module.ts`.

</div>

<div class="s-why-last">

**¿Por qué?:** Facilita la localización e identificación del módulo raíz.

</div>

<code-example path="styleguide/src/04-08/app/app.module.ts" region="example" header="app/app.module.ts">

</code-example>

<a href="#toc">Volver arriba</a>

{@a 04-09}

### Módulos de funciones

#### Estilo 04-09

<div class="s-rule do">

**Realizar:** cree un NgModule para todas las funciones distintas en una aplicación;
por ejemplo, una función de `Heroes`.

</div>

<div class="s-rule do">

**Realizar:** coloque el módulo de funciones en la misma carpeta con el nombre que el área de funciones;
por ejemplo, en `app/heroes`.

</div>

<div class="s-rule do">

**Realizar:** nombrar el archivo del módulo de características que refleje el nombre del área de características
y carpeta; por ejemplo, `app/heroes/heroes.module.ts`.

</div>

<div class="s-rule do">

**Realizar:** nombrar el símbolo del módulo de características que refleje el nombre de la característica
área, carpeta y archivo; por ejemplo, `app/heroes/heroes.module.ts` define `HeroesModule`.

</div>

<div class="s-why">

**¿Por qué?:** Un módulo de funciones puede exponer u ocultar su implementación de otros módulos.

</div>

<div class="s-why">

**¿Por qué?:** Un módulo de características identifica distintos conjuntos de componentes relacionados que comprenden el área de características.

</div>

<div class="s-why">

**¿Por qué?:** Un módulo de funciones puede enrutarse fácilmente tanto con entusiasmo como con pereza.

</div>

<div class="s-why">

**¿Por qué?:** Un módulo de funciones define límites claros entre funciones específicas y otras funciones de la aplicación.

</div>

<div class="s-why">

**¿Por qué?:** Un módulo de funciones ayuda a aclarar y facilitar la asignación de responsabilidades de desarrollo a diferentes equipos.

</div>

<div class="s-why-last">

**¿Por qué?:** Un módulo de funciones se puede aislar fácilmente para realizar pruebas.

</div>

<a href="#toc">Volver arriba</a>

{@a 04-10}

### Módulo de funciones compartidas

#### Estilo 04-10

<div class="s-rule do">

**Realizar:** crear un módulo de funciones llamado `SharedModule` en una carpeta llamada `shared` ;
por ejemplo, `app/shared/shared.module.ts` define `SharedModule`.

</div>

<div class="s-rule do">

**Realizar:** declarar componentes, directivas y canalizaciones en un módulo compartido cuando esos
Los elementos serán reutilizados y referenciados por los componentes declarados en otros módulos de funciones.

</div>

<div class="s-rule consider">

**Considerar:** utilizando el nombre SharedModule cuando el contenido de un compartido
se hace referencia a los módulos en toda la aplicación.

</div>

<div class="s-rule avoid">

**Considerar:** _no_ prestar servicios en módulos compartidos. Los servicios son generalmente
singletons que se proporcionan una vez para toda la aplicación o
en un módulo de funciones en particular. Sin embargo, existen excepciones. Por ejemplo, en el código de muestra que sigue, observe que el `SharedModule` proporciona` FilterTextService`. Esto es aceptable aquí porque el servicio no tiene estado; es decir, los consumidores del servicio no se ven afectados por nuevas instancias.

</div>

<div class="s-rule do">

**Realizar:** importar todos los módulos requeridos por los activos en el `SharedModule`;
por ejemplo, `CommonModule` y` FormsModule`.

</div>

<div class="s-why">

**¿Por qué?:** `SharedModule` contendrá componentes, directivas y canalizaciones que pueden necesitar características de otro módulo común; por ejemplo,
`ngFor` en` CommonModule`.

</div>

<div class="s-rule do">

**Realizar:** declare todos los componentes, directivas y canalizaciones en el `SharedModule`.

</div>

<div class="s-rule do">

**Realizar:** exportar todos los símbolos del `SharedModule` que otros módulos de funciones necesitan usar.

</div>

<div class="s-why">

**¿Por qué?:** `SharedModule` existe para hacer que los componentes, directivas y conductos de uso común estén disponibles para su uso en las plantillas de componentes en muchos otros módulos.

</div>

<div class="s-rule avoid">

**Evitar:** especificando proveedores singleton para toda la aplicación en un `SharedModule`. Los singleton intencionales están bien. Cuídate.

</div>

<div class="s-why">

**¿Por qué?:** Un módulo de funciones de carga diferida que importa ese módulo compartido hará su propia copia del servicio y probablemente tenga resultados no deseados.

</div>

<div class="s-why-last">

**¿Por qué?:** No desea que cada módulo tenga su propia instancia separada de servicios singleton.
Sin embargo, existe un peligro real de que eso suceda si el `SharedModule` proporciona un servicio.

</div>

<div class='filetree'>

  <div class='file'>
    src
  </div>

  <div class='children'>

    <div class='file'>
      app
    </div>

    <div class='children'>
      <div class='file'>
        shared
      </div>

      <div class='children'>
        <div class='file'>
          shared.module.ts
        </div>

        <div class='file'>
          init-caps.pipe.ts|spec.ts
        </div>

        <div class='file'>
          filter-text.component.ts|spec.ts
        </div>

        <div class='file'>
          filter-text.service.ts|spec.ts
        </div>

      </div>

      <div class='file'>
        app.component.ts|html|css|spec.ts
      </div>

      <div class='file'>
        app.module.ts
      </div>

      <div class='file'>
        app-routing.module.ts
      </div>

    </div>

    <div class='file'>
      main.ts
    </div>

    <div class='file'>
      index.html
    </div>

  </div>

  <div class='file'>
    ...
  </div>

</div>

<code-tabs>

  <code-pane header="app/shared/shared.module.ts" path="styleguide/src/04-10/app/shared/shared.module.ts">

  </code-pane>

  <code-pane header="app/shared/init-caps.pipe.ts" path="styleguide/src/04-10/app/shared/init-caps.pipe.ts">

  </code-pane>

  <code-pane header="app/shared/filter-text/filter-text.component.ts" path="styleguide/src/04-10/app/shared/filter-text/filter-text.component.ts">

  </code-pane>

  <code-pane header="app/shared/filter-text/filter-text.service.ts" path="styleguide/src/04-10/app/shared/filter-text/filter-text.service.ts">

  </code-pane>

  <code-pane header="app/heroes/heroes.component.ts" path="styleguide/src/04-10/app/heroes/heroes.component.ts">

  </code-pane>

  <code-pane header="app/heroes/heroes.component.html" path="styleguide/src/04-10/app/heroes/heroes.component.html">

  </code-pane>

</code-tabs>

<a href="#toc">Volver arriba</a>

{@a 04-11}

### Carpetas cargadas de forma diferida

#### Estilo 04-11

Una función de aplicación o un flujo de trabajo distintos pueden cargarse de _forma diferida_ o _según demanda_ en lugar de cuando se inicia la aplicación.

<div class="s-rule do">

**Realizar:** coloque el contenido de las funciones de carga _diferida en una carpeta_.
Una _carpeta con carga diferida_ contiene un _routing component_, sus componentes secundarios y sus activos y módulos relacionados.

</div>

<div class="s-why-last">

**¿Por qué?:** La carpeta facilita la identificación y el aislamiento del contenido de la función.

</div>

<a href="#toc">Volver arriba</a>

{@a 04-12}

### Nunca importe directamente carpetas cargadas de forma diferida

#### Estilo 04-12

<div class="s-rule avoid">

**Evitar:** permitiendo que los módulos en las carpetas del hermano y de los padres importen directamente un módulo en una _función de carga diferida_.

</div>

<div class="s-why-last">

**¿Por qué?:** La importación directa y el uso de un módulo lo cargarán inmediatamente cuando la intención sea cargarlo a pedido.

</div>

<a href="#toc">Volver arriba</a>

## Componentes

{@a 05-03}

### Componentes como elementos

#### Estilo 05-03

<div class="s-rule do">

**Considerar:** dando a los componentes un selector de _elemento_, a diferencia de los selectores de _attribute_ o _clase_.

</div>

<div class="s-why">

**¿Por qué?:** Los componentes tienen plantillas que contienen HTML y sintaxis de plantilla angular opcional.
Muestran contenido.
Los desarrolladores colocan componentes en la página como lo harían con elementos HTML nativos y componentes web.

</div>

<div class="s-why-last">

**¿Por qué?:** Es más fácil reconocer que un símbolo es un componente mirando las plantillas html.

</div>

<div class="alert is-helpful">

Hay algunos casos en los que le asigna un atributo a un componente, como cuando desea aumentar un elemento integrado. Por ejemplo, [Material Design](https://material.angular.io/components/button/overview) utiliza esta técnica con `<button mat-button>`. Sin embargo, no utilizaría esta técnica en un elemento personalizado.

</div>

<code-example path="styleguide/src/05-03/app/heroes/shared/hero-button/hero-button.component.avoid.ts" region="example" header="app/heroes/hero-button/hero-button.component.ts">

</code-example>

<code-example path="styleguide/src/05-03/app/app.component.avoid.html" header="app/app.component.html">

</code-example>

<code-tabs>

  <code-pane header="app/heroes/shared/hero-button/hero-button.component.ts" path="styleguide/src/05-03/app/heroes/shared/hero-button/hero-button.component.ts" region="example">

  </code-pane>

  <code-pane header="app/app.component.html" path="styleguide/src/05-03/app/app.component.html">

  </code-pane>

</code-tabs>

<a href="#toc">Volver arriba</a>

{@a 05-04}

### Extraiga plantillas y estilos a sus propios archivos

#### Estilo 05-04

<div class="s-rule do">

**Realizar:** extrae plantillas y estilos en un archivo separado, cuando hay más de 3 líneas.

</div>

<div class="s-rule do">

**Realizar:** nombrar el archivo de plantilla `[component-name].component.html`, donde [component-name] es el nombre del componente.

</div>

<div class="s-rule do">

**Realizar:** nombrar el archivo de estilo `[component-name].component.css`, donde [component-name] es el nombre del componente.

</div>

<div class="s-rule do">

**Realizar:** especificar _component-relative_ URLs, prefijado con `./`.

</div>

<div class="s-why">

**¿Por qué?:** Las plantillas y estilos grandes en línea oscurecen el propósito y la implementación del componente, lo que reduce la legibilidad y el mantenimiento.

</div>

<div class="s-why">

**¿Por qué?:** En la mayoría de los editores, las sugerencias de sintaxis y los fragmentos de código no están disponibles al desarrollar estilos y plantillas en línea.
El servicio de lenguaje Angular TypeScript (de próxima aparición) promete superar esta deficiencia para las plantillas HTML
en aquellos editores que lo apoyan; no ayudará con los estilos CSS.

</div>

<div class="s-why">

**¿Por qué?:** Una _component relative_ La URL no requiere cambios cuando mueve los archivos componentes, siempre que los archivos permanezcan juntos.

</div>

<div class="s-why-last">

**¿Por qué?:** El prefijo `./` es una sintexia estándar para URL relativas; no depende de la capacidad actual de Angular para prescindir de ese prefijo.

</div>

<code-example path="styleguide/src/05-04/app/heroes/heroes.component.avoid.ts" region="example" header="app/heroes/heroes.component.ts">

</code-example>

<code-tabs>

  <code-pane header="app/heroes/heroes.component.ts" path="styleguide/src/05-04/app/heroes/heroes.component.ts" region="example">

  </code-pane>

  <code-pane header="app/heroes/heroes.component.html" path="styleguide/src/05-04/app/heroes/heroes.component.html">

  </code-pane>

  <code-pane header="app/heroes/heroes.component.css" path="styleguide/src/05-04/app/heroes/heroes.component.css">

  </code-pane>

</code-tabs>

<a href="#toc">Volver arriba</a>

{@a 05-12}

### decorador de las propiedades _input_ and _output_

#### Estilo 05-12

<div class="s-rule do">

**Realizar:** utilizar los decoradores de clase `@Input ()` y `@Output ()` en lugar de las propiedades `inputs` y `outputs` de los
Metadatos `@Directive` y` @Component`:

</div>

<div class="s-rule consider">

**Considerar:** colocando `@Input ()` o `@Output ()` en la misma línea que la propiedad que decora.

</div>

<div class="s-why">

**¿Por qué?:** Es más fácil y legible identificar qué propiedades de una clase son entradas o salidas.

</div>

<div class="s-why">

**¿Por qué?:** Si alguna vez necesita cambiar el nombre de la propiedad o del evento asociado con
`@Input()` o `@Output()`, puedes modificarlo en un solo lugar.

</div>

<div class="s-why">

**¿Por qué?:** La declaración de metadatos adjunta a la directiva es más corta y, por tanto, más legible.

</div>

<div class="s-why-last">

**¿Por qué?:** Colocar al decorador en la misma línea _ generalmente_ hace que el código sea más corto y aún identifica fácilmente la propiedad como una entrada o salida.
Colóquelo en la línea de arriba cuando al hacerlo sea claramente más legible.

</div>

<code-example path="styleguide/src/05-12/app/heroes/shared/hero-button/hero-button.component.avoid.ts" region="example" header="app/heroes/shared/hero-button/hero-button.component.ts">

</code-example>

<code-example path="styleguide/src/05-12/app/heroes/shared/hero-button/hero-button.component.ts" region="example" header="app/heroes/shared/hero-button/hero-button.component.ts">

</code-example>

<a href="#toc">Volver arriba</a>

{@a 05-13}

### Evite el alias _inputs_ and _outputs_

#### Estilo 05-13

<div class="s-rule avoid">

**Evitar:** _input_ y _output_ alias excepto cuando tiene un propósito importante.

</div>

<div class="s-why">

**¿Por qué?:** Dos nombres para la misma propiedad (uno privado y otro público) es intrínsecamente confuso.

</div>

<div class="s-why-last">

**¿Por qué?:** Debería utilizar un alias cuando el nombre de la directiva también sea una propiedad _input_,
y el nombre de la directiva no describe la propiedad.

</div>

<code-example path="styleguide/src/05-13/app/heroes/shared/hero-button/hero-button.component.avoid.ts" region="example" header="app/heroes/shared/hero-button/hero-button.component.ts">

</code-example>

<code-example path="styleguide/src/05-13/app/app.component.avoid.html" header="app/app.component.html">

</code-example>

<code-tabs>

  <code-pane header="app/heroes/shared/hero-button/hero-button.component.ts" path="styleguide/src/05-13/app/heroes/shared/hero-button/hero-button.component.ts" region="example">

  </code-pane>

  <code-pane header="app/heroes/shared/hero-button/hero-highlight.directive.ts" path="styleguide/src/05-13/app/heroes/shared/hero-highlight.directive.ts">

  </code-pane>

  <code-pane header="app/app.component.html" path="styleguide/src/05-13/app/app.component.html">

  </code-pane>

</code-tabs>

<a href="#toc">Volver arriba</a>

{@a 05-14}

### Secuencia de miembros

#### Estilo 05-14

<div class="s-rule do">

**Realizar:** coloque las propiedades en la parte superior seguidas de los métodos.

</div>

<div class="s-rule do">

**Realizar:** coloque los miembros privados después de los miembros públicos, en orden alfabético.

</div>

<div class="s-why-last">

**¿Por qué?:** Colocar los miembros en una secuencia consistente hace que sea fácil de leer y
ayuda a identificar instantáneamente qué miembros del componente sirven para qué propósito.

</div>

<code-example path="styleguide/src/05-14/app/shared/toast/toast.component.avoid.ts" region="example" header="app/shared/toast/toast.component.ts">

</code-example>

<code-example path="styleguide/src/05-14/app/shared/toast/toast.component.ts" region="example" header="app/shared/toast/toast.component.ts">

</code-example>

<a href="#toc">Volver arriba</a>

{@a 05-15}

### Delegar lógica de componentes complejos a los servicios

#### Estilo 05-15

<div class="s-rule do">

**Realizar:** limitar la lógica en un componente a solo la requerida para la vista. Toda otra lógica debe delegarse a los servicios.

</div>

<div class="s-rule do">

**Realizar:** Mueva la lógica reutilizable a los servicios y mantenga los componentes simples y enfocados en su propósito previsto.

</div>

<div class="s-why">

**¿Por qué?:** La lógica puede ser reutilizada por múltiples componentes cuando se coloca dentro de un servicio y se expone a través de una función.

</div>

<div class="s-why">

**¿Por qué?:** La lógica de un servicio se puede aislar más fácilmente en una prueba unitaria, mientras que la lógica de llamada en el componente se puede burlar fácilmente.

</div>

<div class="s-why">

**¿Por qué?:** Elimina las dependencias y oculta los detalles de implementación del componente.

</div>

<div class="s-why-last">

**¿Por qué?:** Mantiene el componente delgado, recortado y enfocado.

</div>

<code-example path="styleguide/src/05-15/app/heroes/hero-list/hero-list.component.avoid.ts" header="app/heroes/hero-list/hero-list.component.ts">

</code-example>

<code-example path="styleguide/src/05-15/app/heroes/hero-list/hero-list.component.ts" region="example" header="app/heroes/hero-list/hero-list.component.ts">

</code-example>

<a href="#toc">Volver arriba</a>

{@a 05-16}

### No prefijas las propiedades _output_

#### Estilo 05-16

<div class="s-rule do">

**Realizar:** nombrar eventos sin el prefijo `on`.

</div>

<div class="s-rule do">

**Realizar:** Nombre los métodos de manejo de eventos con el prefijo `on` seguido del nombre del evento.

</div>

<div class="s-why">

**¿Por qué?:** Esto es coherente con los eventos integrados, como los clics en los botones.

</div>

<div class="s-why-last">

**¿Por qué?:** Angular permite una [alternative syntax](guide/binding-syntax) `on-*`. Si el evento en sí tuviera el prefijo `on` esto resultaría en una expresión de enlace `on-onEvent`.

</div>

<code-example path="styleguide/src/05-16/app/heroes/hero.component.avoid.ts" region="example" header="app/heroes/hero.component.ts">

</code-example>

<code-example path="styleguide/src/05-16/app/app.component.avoid.html" header="app/app.component.html">

</code-example>

<code-tabs>

  <code-pane header="app/heroes/hero.component.ts" path="styleguide/src/05-16/app/heroes/hero.component.ts" region="example">

  </code-pane>

  <code-pane header="app/app.component.html" path="styleguide/src/05-16/app/app.component.html">

  </code-pane>

</code-tabs>

<a href="#toc">Volver arriba</a>

{@a 05-17}

### Ponga lógica de presentación en la clase de componente

#### Estilo 05-17

<div class="s-rule do">

**Realizar:** coloque la lógica de presentación en la clase de componente y no en la plantilla.

</div>

<div class="s-why">

**¿Por qué?:** La lógica estará contenida en un lugar (la clase de componente) en lugar de estar esparcida en dos lugares.

</div>

<div class="s-why-last">

**¿Por qué?:** Mantener la lógica de presentación del componente en la clase en lugar de la plantilla mejora la capacidad de prueba, el mantenimiento y la reutilización.

</div>

<code-example path="styleguide/src/05-17/app/heroes/hero-list/hero-list.component.avoid.ts" region="example" header="app/heroes/hero-list/hero-list.component.ts">

</code-example>

<code-example path="styleguide/src/05-17/app/heroes/hero-list/hero-list.component.ts" region="example" header="app/heroes/hero-list/hero-list.component.ts">

</code-example>

<a href="#toc">Volver arriba</a>

## Directivas

{@a 06-01}

### Usar directivas para mejorar un elemento

#### Estilo 06-01

<div class="s-rule do">

**Realizar:** use directivas de atributo cuando tenga lógica de presentación sin una plantilla.

</div>

<div class="s-why">

**¿Por qué?:** Las directivas de atributo no tienen una plantilla asociada.

</div>

<div class="s-why-last">

**¿Por qué?:** Un elemento puede tener aplicada más de una directiva de atributo.

</div>

<code-example path="styleguide/src/06-01/app/shared/highlight.directive.ts" region="example" header="app/shared/highlight.directive.ts">

</code-example>

<code-example path="styleguide/src/06-01/app/app.component.html" header="app/app.component.html">

</code-example>

<a href="#toc">Volver arriba</a>

{@a 06-03}

### El decorador _HostListener_/_HostBinding_ versus el decorador _host_ metadata

#### Estilo 06-03

<div class="s-rule consider">

**Considerar:** prefiriendo el `@HostListener` y` @HostBinding` al
Propiedad `host` de los decoradores` @Directive` y `@Component`.

</div>

<div class="s-rule do">

**Realizar:** Sea consistente en su elección.

</div>

<div class="s-why-last">

**¿Por qué?:** La propiedad asociada con `@ HostBinding` o el método asociado con` @ HostListener`
se puede modificar solo en un solo lugar&mdash; en la clase de la directiva.
Si usa la propiedad de metadatos `host`, debe modificar tanto la declaración de propiedad / método en el
la clase de la directiva y los metadatos en el decorador asociado con la directiva.

</div>

<code-example path="styleguide/src/06-03/app/shared/validator.directive.ts" header="app/shared/validator.directive.ts">

</code-example>

Compare con la alternativa de metadatos de `host` menos preferida.

<div class="s-why-last">

**¿Por qué?:** Los metadatos del `host` son solo un término para recordar y no requieren importaciones de ES adicionales.

</div>

<code-example path="styleguide/src/06-03/app/shared/validator2.directive.ts" header="app/shared/validator2.directive.ts">

</code-example>

<a href="#toc">Volver arriba</a>

## Servicios

{@a 07-01}

### Los servicios son singletons

#### Estilo 07-01

<div class="s-rule do">

**Realizar:** utilice los servicios como singletons dentro del mismo inyector. Úselos para compartir datos y funcionalidad.

</div>

<div class="s-why">

**¿Por qué?:** Los servicios son ideales para compartir métodos en un área de funciones o una aplicación.

</div>

<div class="s-why-last">

**¿Por qué?:** Services are ideal for sharing stateful in-memory data.

</div>

<code-example path="styleguide/src/07-01/app/heroes/shared/hero.service.ts" region="example" header="app/heroes/shared/hero.service.ts">

</code-example>

<a href="#toc">Volver arriba</a>

{@a 07-02}

### Responsabilidad única

#### Estilo 07-02

<div class="s-rule do">

**Realizar:** crear servicios con una sola responsabilidad que está encapsulada por su contexto.

</div>

<div class="s-rule do">

**Realizar:** crear un nuevo servicio una vez que el servicio comience a exceder ese propósito singular.

</div>

<div class="s-why">

**¿Por qué?:** Cuando un servicio tiene múltiples responsabilidades, resulta difícil probarlo.

</div>

<div class="s-why-last">

**¿Por qué?:** Cuando un servicio tiene múltiples responsabilidades, cada componente o servicio que lo inyecta ahora tiene el peso de todas.

</div>

<a href="#toc">Volver arriba</a>

{@a 07-03}

### Brindar un servicio

#### Estilo 07-03

<div class="s-rule do">

**Realizar:** proporcionar un servicio con la aplicación root injector en el decorador `@Injectable` del servicio.

</div>

<div class="s-why">

**¿Por qué?:** El inyector angular es jerárquico.

</div>

<div class="s-why">

**¿Por qué?:** Cuando proporciona el servicio a un inyector raíz, esa instancia del servicio se comparte y está disponible en todas las clases que necesitan el servicio. Esto es ideal cuando un servicio comparte métodos o estado.

</div>

<div class="s-why">

**¿Por qué?:** Cuando registra un servicio en el decorador `@Injectable` del servicio, las herramientas de optimización como las utilizadas por las compilaciones de producción de [Angular CLI's](cli) pueden realizar sacudidas de árbol y eliminar servicios que no utiliza su aplicación.

</div>

<div class="s-why-last">

**¿Por qué?:** Esto no es ideal cuando dos componentes diferentes necesitan diferentes instancias de un servicio. En este escenario, sería mejor proporcionar el servicio en el nivel de componente que necesita la instancia nueva y separada.

</div>

<code-example path="dependency-injection/src/app/tree-shaking/service.ts" header="src/app/treeshaking/service.ts"></code-example>

<a href="#toc">Volver arriba</a>

{@a 07-04}

### Usa el decorador de clases @Injectable()

#### Estilo 07-04

<div class="s-rule do">

**Realizar:** use el decorador de clase `@Injectable()` en lugar del decorador de parámetros `@Inject` cuando use tipos como tokens para las dependencias de un servicio.

</div>

<div class="s-why">

**¿Por qué?:** El mecanismo de inyección de dependencia angular (DI) resuelve el propio servicio
dependencias basadas en los tipos declarados de los parámetros del constructor de ese servicio.

</div>

<div class="s-why-last">

**¿Por qué?:** Cuando un servicio acepta solo dependencias asociadas con tokens de tipo, la sintaxis `@Injectable()` es mucho menos detallada en comparación con el uso de `@Inject()` en cada parámetro de constructor individual.

</div>

<code-example path="styleguide/src/07-04/app/heroes/shared/hero-arena.service.avoid.ts" region="example" header="app/heroes/shared/hero-arena.service.ts">

</code-example>

<code-example path="styleguide/src/07-04/app/heroes/shared/hero-arena.service.ts" region="example" header="app/heroes/shared/hero-arena.service.ts">

</code-example>

<a href="#toc">Volver arriba</a>

## Servicios de datos

{@a 08-01}

### Hablar con el servidor a través de un servicio

#### Estilo 08-01

<div class="s-rule do">

**Realizar:** refactorizar la lógica para realizar operaciones de datos e interactuar con los datos en un servicio.

</div>

<div class="s-rule do">

**Realizar:** hacer que los servicios de datos sean responsables de las llamadas XHR, el almacenamiento local, el almacenamiento en la memoria o cualquier otra operación de datos.

</div>

<div class="s-why">

**¿Por qué?:** The component's responsibility is for the presentation and gathering of information for the view. It should not care how it gets the data, just that it knows who to ask for it. Separating the data services moves the logic on how to get it to the data service, and lets the component be simpler and more focused on the view.

</div>

<div class="s-why">

**¿Por qué?:** Esto facilita la prueba (simulada o real) de las llamadas de datos cuando se prueba un componente que utiliza un servicio de datos.

</div>

<div class="s-why-last">

**¿Por qué?:** Los detalles de la gestión de datos, como encabezados, métodos HTTP, el almacenamiento en caché, el manejo de errores y la lógica de reintento son irrelevantes para los componentes
y otros consumidores de datos.

Un servicio de datos encapsula estos detalles. Es más fácil evolucionar estos
detalles dentro del servicio sin afectar a sus consumidores. Y es
Es más fácil probar a los consumidores con implementaciones de servicios simuladas.

</div>

<a href="#toc">Volver arriba</a>

## Ciclo de vida de los hooks

Use los hooks del ciclo de vida para aprovechar los eventos importantes expuestos por Angular.

<a href="#toc">Volver arriba</a>

{@a 09-01}

### Implementar interfaces de enlace de ciclo de vida

#### Estilo 09-01

<div class="s-rule do">

**Realizar:** implementar las interfaces de enlace del ciclo de vida.

</div>

<div class="s-why-last">

**¿Por qué?:** Las interfaces de ciclo de vida prescriben un método escrito
firmas. Utilice esas firmas para marcar errores de ortografía y sintaxis.

</div>

<code-example path="styleguide/src/09-01/app/heroes/shared/hero-button/hero-button.component.avoid.ts" region="example" header="app/heroes/shared/hero-button/hero-button.component.ts">

</code-example>

<code-example path="styleguide/src/09-01/app/heroes/shared/hero-button/hero-button.component.ts" region="example" header="app/heroes/shared/hero-button/hero-button.component.ts">

</code-example>

<a href="#toc">Volver arriba</a>

## Apéndice

Herramientas y consejos útiles para Angular.

<a href="#toc">Volver arriba</a>

{@a A-01}

### Codelyzer.

#### Estilo A-01

<div class="s-rule do">

**Realizar:** Utilice [codelyzer](https://www.npmjs.com/package/codelyzer) para seguir esta guía.

</div>

<div class="s-rule consider">

**Considerar:** ajustando las reglas en codelyzer para satisfacer sus necesidades.

</div>

<a href="#toc">Volver arriba</a>

{@a A-02}

### Plantillas de archivos y fragmentos

#### Estilo A-02

<div class="s-rule do">

**Use** plantillas de archivo o fragmentos para ayudar a seguir estilos y patrones consistentes. Aquí hay plantillas y / o fragmentos de algunos de los IDE y editores de desarrollo web.

</div>

<div class="s-rule consider">

**Considerar:** Utilizando [snippets](https://marketplace.visualstudio.com/items?itemName=johnpapa.Angular2) for [Visual Studio Code](https://code.visualstudio.com/) que siguen estos estilos y pautas.

<a href="https://marketplace.visualstudio.com/items?itemName=johnpapa.Angular2">
  <img src="generated/images/guide/styleguide/use-extension.gif" alt="Use Extension">
</a>

**Considerar:** Utilizando [snippets](https://atom.io/packages/angular-2-typescript-snippets) para [Atom](https://atom.io/) Siga estos estilos y pautas.

**Considerar:** Utilizando [snippets](https://github.com/orizens/sublime-angular2-snippets) for [Sublime Text](http://www.sublimetext.com/) Siga estos estilos y pautas.

**Considerar:** Utilizando [snippets](https://github.com/mhartington/vim-angular2-snippets) for [Vim](http://www.vim.org/) Siga estos estilos y pautas.

</div>

<a href="#toc">Volver arriba</a>
