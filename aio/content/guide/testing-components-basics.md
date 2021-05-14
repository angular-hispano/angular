# Conceptos básicos de prueba de componentes.

Un componente, a diferencia de otros partes de una aplicación de angular,
combina una plantilla HTML y una clase de TypeScript.
El componente realmente es la plantilla y la clase _trabajando_ juntas. Para probar adecuadamente un
componente debe probar que funcionan juntas como se espera.

Tal prueba requiere la creación del elemento host del componente en el DOM del navegador,
como lo hace Angular, e investigando la interacción de la clase del componente con
el DOM como está descrito por su plantilla.

El `TestBed` de Angular facilita este tipo de pruebas como podría ver en la sección de abajo.
Pero en muchos casos, _probar solo el componente de la clase_, sin intervención del DOM,
puede validar muchos de los comportamientos de los componentes de una manera más fácil, y obvia.

<div class="alert is-helpful">

  La aplicación de muestra describe las guías de pruebas, ver la <live-example name="testing" embedded-style noDownload>aplicación de muestra</live-example>.

  Para las funciones de prueba en las guías de prueba, ver <live-example name="testing" stackblitz="specs" noDownload>pruebas</live-example>.

</div>


{@a component-class-testing}

## Prueba de la clase del componente

Pruebe una clase del componente por su cuenta como lo haría una clase de servicio.

La prueba de clase de componente podría mantenerse demasiado simple y limpio.
Podría probar solo una unidad singular.
A primera vista, podría ser capaz de comprender
lo que está probando la prueba.

Considerando esto `LightswitchComponent` que alterna entre encendido y apagado
(representado por un mensaje en la pantalla) cuando el usuario cliquea el botón.

<code-example
  path="testing/src/app/demo/demo.ts"
  region="LightswitchComp"
  header="app/demo/demo.ts (LightswitchComp)"></code-example>

Podría decidir solo probar `clicked()`, el método
cambia el estado de _encendido/apagado_ y establece el mensaje apropiadamente.

Esta clase de componente no tiene dependencias. Al probar estos tipos de clases siguiendo algunos pasos como debería para un servicio que no tiene dependencias:

1. Crear un componente usando la nueva palabra clave.
2. Poke at its API.
3. Afirma expectativas sobre su estado público.

<code-example
  path="testing/src/app/demo/demo.spec.ts"
  region="Lightswitch"
  header="app/demo/demo.spec.ts (Lightswitch tests)"></code-example>

Aquí está el `DashboardHeroComponent` desde el tutorial de  _Gira de Héroes_.

<code-example
  path="testing/src/app/dashboard/dashboard-hero.component.ts"
  region="class"
  header="app/dashboard/dashboard-hero.component.ts (component)"></code-example>

Aparece dentro de la plantilla de un componente padre,
que une un _héroe_ a la propiedad `@Input` y
escucha un evento elevado a través de la propiedad `@Output` _seleccionada_.

Puedes probar que el código de la clase funciona sin crear el `DashboardHeroComponent`
o su componente padre.

<code-example
  path="testing/src/app/dashboard/dashboard-hero.component.spec.ts"
  region="class-only"
  header="app/dashboard/dashboard-hero.component.spec.ts (class tests)"></code-example>

Cuando un componente tiene dependencias, es posible que desees utilizar el `TestBed` para crear
el componente y sus dependencias.

El siguiente `WelcomeComponent` depende del `UserService` para conocer el nombre del usuario a saludar.

<code-example
  path="testing/src/app/welcome/welcome.component.ts"
  region="class"
  header="app/welcome/welcome.component.ts"></code-example>

Podrias comenzar por crear un Mock de el `UserService` que cumpla con la minima necesidad de este componente.

<code-example
  path="testing/src/app/welcome/welcome.component.spec.ts"
  region="mock-user-service"
  header="app/welcome/welcome.component.spec.ts (MockUserService)"></code-example>

Luego promueve e inyecta _tanto el_ **componente** _y el servicio_ en la configuración del `TestBed`.

<code-example
  path="testing/src/app/welcome/welcome.component.spec.ts"
  region="class-only-before-each"
  header="app/welcome/welcome.component.spec.ts (class-only setup)"></code-example>

Luego la clase del componente, recuerda la llamada de los [métodos de enlace del ciclo de vida](guide/lifecycle-hooks) como Angular lo hace al ejecutar la aplicación.

<code-example
  path="testing/src/app/welcome/welcome.component.spec.ts"
  region="class-only-tests"
  header="app/welcome/welcome.component.spec.ts (class-only tests)"></code-example>

## Pruebas de componentes DOM

Probando el componente _clase_ es demasiado facil como [probar un servicio](guide/testing-services).

Pero un componente es más que solo su clase.
Un componente interactúa con el DOM y con otros componentes.
Las pruebas _solo-clase_ pueden decirle sobre el comportamiento de la clase.
Ellos no pueden decirle si el componente se va a renderizar correctamente,
respondiendo a la entrada y los gestos del usuario, o integrarse con su componente padre e hijo.

Ninguna de las pruebas _solo-clase_ anteriores pueden responder preguntas claves acerca de como los
componentes realmente se comportan en la pantalla.

- ¿`Lightswitch.clicked()` está vinculado a algo de modo que el usuario pueda invocarlo?
- ¿Se muestra el mensaje `Lightswitch.message`?
- ¿Puede el usuario actualmente seleccionar el héroe mostrado por `DashboardHeroComponent`?
- ¿Se muestra el nombre del héroe como se esperaba (es decir, en mayúsculas)?
- ¿Se muestra el mensaje de bienvenida por la plantilla de `WelcomeComponent`?

Es posible que estas no sean preguntas preocupantes para los componentes simples ilustrados
anteriormente.
Pero muchos componentes tienen interacciones complejas con elementos DOM
descritos en sus plantillas, causando que HTML aparezca y desaparezca como
el estado del componente cambia.

Para responder a este tipo de preguntas, debe crear los elementos DOM asociados
con los componentes, debe examinar el DOM para confirmar que el estado del componente
se muestra correctamente en los momentos adecuados, y debe simular la interacción del usuario
con la pantalla para determinar si esas interacciones hacen que el componente
se comporta como se esperaba.

Para escribir este tipo de prueba, utilizará funcionalidades adicionales del `TestBed`
así como otros ayudantes de prueba.

### CLI - Pruebas generadas

El CLI crea un archivo de prueba inicial para usted de forma predeterminada cuando se le solicita
que genere un nuevo componente.

Por ejemplo, el siguiente comando CLI genera un `BannerComponent` en la carpeta `app/banner` 
(con plantilla y estilos en línea):

<code-example language="sh" class="code-shell">
ng generate component banner --inline-template --inline-style --module app
</code-example>

Tambien genera un archivo de prueba inicial para el componente, `banner-external.component.spec.ts`, que se ve así:

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="v1"
  header="app/banner/banner-external.component.spec.ts (initial)"></code-example>

<div class="alert is-helpful">

Porque `compileComponents` es asíncrono, usa
el [`waitForAsync`](api/core/testing/waitForAsync) función de
utilidad importada de `@angular/core/testing`.

Por favor, consulte la sección [waitForAsync](guide/testing-components-scenarios#waitForAsync) para más detalles.

</div>

### Reducir la preparación.

Solo las últimas tres líneas de este archivo prueban realmente el componente
y todo lo que hacen es afirmar que Angular puede crear el componente.

El resto del archivo es un código de preparación estándar que anticipa pruebas más avanzadas que _podrían_ ser necesarias si el componente se convierte en algo sustancial.

Aprenderá sobre estas funciones de prueba avanzadas a continuación.
Por ahora, puedes radicalmente reducir estos archivos de prueba a un tamaño más manejable.

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="v2"
  header="app/banner/banner-initial.component.spec.ts (minimal)"></code-example>

En este ejemplo, los objetos de metadatos pasaron a `TestBed.configureTestingModule`
simplemente declara el componente de prueba `BannerComponent`.

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="configureTestingModule">
</code-example>

<div class="alert is-helpful">

No es necesario declarar ni importar nada más.
El módulo de prueba predeterminado está pre configurado con
algo como `BrowserModule` de `@angular/platform-browser`.

Después podrás llamar `TestBed.configureTestingModule()` con
imports, providers, y más declaraciones para adaptarse a sus necesidades de prueba.
Los métodos opcionales de `override` pueden afinar aún más aspectos de la configuración.

</div>

{@a create-component}

### _createComponent()_

Después de configurar `TestBed`, llamarás el método `createComponent()`

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="createComponent">
</code-example>

`TestBed.createComponent()` crea una instancia del `BannerComponent`,
añade un elemento correspondiente al DOM del corredor de pruebas,
y regresa un [`ComponentFixture`](#component-fixture).

<div class="alert is-important">

No reconfigure `TestBed` después de llamar `createComponent`.

El método `createComponent` congela la definición actual de `TestBed`,
cerrándolo para más configuración.

No puede llamar a ningún otro método de configuración `TestBed`, no `configureTestingModule()`,
ni `get()`, ni ninguno de los métodos `override...`.
Si lo intenta, `TestBed` arroja un error.

</div>

{@a component-fixture}

### _ComponentFixture_

El [ComponenteFixture](api/core/testing/ComponentFixture) es un arnés de prueba para interactuar con el componente creado y su elemento correspondiente.

Accede a la instancia del componente a través del fixture y confirmar la existencia con una expectativa de Jasmine:

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="componentInstance">
</code-example>

### _beforeEach()_

Podría agregar más pruebas a medida que este componente evolucione.
En lugar de duplicar la configuración del `TestBed` para cada prueba,
refactorice`beforeEach()` para atraer la preparación a Jasmine y algunas variables de apoyo.

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="v3"
 ></code-example>

Ahora agregue una prueba que obtenga el elemento del componente desde `fixture.nativeElement` y
observe el texto esperado. 

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="v4-test-2">
</code-example>

{@a native-element}

### _nativeElement_

El valor de `ComponentFixture.nativeElement` tiene el tipo `any`.
Más tarde encontrará el `DebugElement.nativeElement` y también tendrá el tipo `any`.

Angular no puede conocer en tiempo de compilación que tipo de elemento HTML es `nativeElement` o
incluso si es un elemento HTML.
La aplicación podría ser ejecutada sobre una _plataforma sin-navegador_, como el servidor o un
[Trabajador Web](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API),
donde el elemento puede tener una API disminuida o no existir del todo.

Las pruebas en esta guía, están diseñadas para ejecutarse en un navegador por lo
que el valor de `nativeElement` siempre será un `HTMLElement` o
una de sus clases derivadas.

Sabiendo que es un "HTMLElement" de algún tipo, `querySelector` puede usar
el estándar HTML para sumergirse más profundamente en el árbol de elementos.

Aquí hay otra prueba que llama a `HTMLElement.querySelector` para obtener el elemento de párrafo y
buscar el texto del banner:

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="v4-test-3">
</code-example>

{@a debug-element}

### _DebugElement_

El _fixture_ de Angular proporciona el elemento del componente directamente a través del `fixture.nativeElement`.

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="nativeElement">
</code-example>

Este es actualmente un método conveniente, implementado como
`fixture.debugElement.nativeElement`.

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="debugElement-nativeElement">
</code-example>

Hay una buena razón para este camino tortuoso al elemento.

Las propiedades del `nativeElement` depende del entorno de ejecución.
Podría ser ejecutadas esas pruebas sobre una plataforma _no-navegador_ que no tiene DOM o
cuya emulación DOM no sea compatible con la API completa de `HTMLElement`.

Angular se basa en la abstracción del `DebugElement` para trabajar de forma segura en
_todas las plataformas compatibles_.
En lugar de crear un árbol elementos HTML, Angular crea un árbol `DebugElement` que envuelve los _elementos nativos_ para la plataforma de ejecución.
La propiedad `nativeElement` desenvuelve `DebugElement` y retorna el objecto de elemento especifico
de la plataforma.

Dado que las pruebas de muestra para esta guía son diseñadas para ejecutarse solo en un navegador,
un `nativeElement` en estas pruebas es siempre un `HTMLElement`
cuyos metodos y propiedades familiares puede explorarse dentro de una prueba.

Aquí está la prueba anterior, reimplementado con `fixture.debugElement.nativeElement`:

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="v4-test-4">
</code-example>

El `DebugElement` tiene otros métodos y propiedades que
son útiles en las pruebas, como ya verás en otra parte en esta guía.

Importas el símbolo del `DebugElement`desde la librería central de Angular.

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="import-debug-element">
</code-example>

{@a by-css}
### _By.css()_

Aunque las pruebas en esta guía todas se ejecutan en el navegador,
algunas aplicaciones pueden ejecutarse en una plataforma diferente al menos parte del tiempo.

Por ejemplo, el componente podría renderizarse primero en el servidor como parte de una estrategia para hacer a la aplicación lanzarse más rápido en dispositivos con mala conexión. La renderización del lado del servidor podría no soportar la API de elementos HTML completa.
Sí no es compatible con el `querySelector`, la prueba anterior podría fallar.

El `DebugElement` ofrece métodos de consulta que funcionan para todas las plataformas.
Estos métodos de consulta toman una función _predicado_ que devuelve `verdadero` cuando un nodo
en el árbol `DebugElement` coincide con los criterios de selección.

Creas un _predicado_ con la ayuda de una clase `By` importada desde una
librería para la plataforma de ejecución. Aquí está la importación `By` para la plataforma del navegador:

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="import-by">
</code-example>

El siguiente ejemplo re-implementamos la prueba anterior con
`DebugElement.query()` y el método `By.css` del navegador.

<code-example
  path="testing/src/app/banner/banner-initial.component.spec.ts"
  region="v4-test-5">
</code-example>

Algunas observaciones notables:

- El metodo estatico `By.css()` selecciona los nodos `DebugElement`
  con un [Selector estandar CSS](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_started/Selectors 'CSS selectors').
- La consulta devuelve un `DebugElement` para el párrafo.
- Debes desenvolver ese resultado para obtener el elemento párrafo.

Cuando estás filtrando por un selector CSS y solo pruebas propiedades de un _elemento nativo_ del navegador, el enfoque `By.css` puede ser excesivo.

A menudo es mas facil y claro filtrar con un metodo estandar de `HTMLElement`
como `querySelector()` o `querySelectorAll()`,
como veras en el siguiente conjunto de pruebas.

