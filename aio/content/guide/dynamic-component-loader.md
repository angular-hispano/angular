# Cargador de componentes dinámicos

Las plantillas de componentes no siempre están fijas. Una aplicación puede necesitar cargar nuevos componentes en tiempo de ejecución.

Esta guía te muestra cómo usar `ComponentFactoryResolver` para añadir componentes dinámicamente.

Mira el <live-example name="dynamic-component-loader"></live-example>
del código de esta guía.

{@a dynamic-loading}

## Carga dinámica de componentes

El siguiente ejemplo muestra cómo crear un banner publicitario dinámico.

La agencia "hero" está planeando una campaña publicitaria en el que varias publicidades roten en un banner. Nuevos componentes de publicidad fueron añadidos frecuentemente por diferentes equipos. Esto hace que utilizar una estructura de componentes estáticos sea poco práctico.

En cambio, necesitas una manera de cargar un nuevo componente sin una referencia fija
al componente en la plantilla del banner publicitario.

Angular viene con su propio API para cargar componentes dinámicamente. 

{@a directive}

## La directiva de anclaje

Antes de que pueda añadir componentes tu debes definir el punto de anclaje
para decirle a Angular donde insertar componentes. 

El banner publicitario utiliza una directiva de ayuda llamada `AdDirective` para
marcar puntos válidos de inserción en la plantilla.


<code-example path="dynamic-component-loader/src/app/ad.directive.ts" header="src/app/ad.directive.ts"></code-example>



`AdDirective` inyecta el `ViewContainerRef` para acceder al contenedor
de vistas del elemento que alojará los componentes añadidos dinámicamente.

En el decorador `@Directive`, observa el nombre del selector, `adHost`;
que es usado para aplicar la directiva al elemento. 
La siguiente sección te muestra como.

{@a loading-components}

## Carga de componentes

La mayor parte de la implementación del banner publicitario está en `ad-banner.component.ts`.
Para mantener las cosas simples en este ejemplo, el HTML está en la `plantilla` del decorador `@Component` como una propiedad de plantillas literales.

El elemento `<ng-template>` es donde se aplica la directiva que acabas de hacer.
Para aplicar la `AdDirective`, recuerda el selector de `ad.directive.ts`,
`[adHost]`. Aplica eso a `<ng-template>` sin los corchetes. Ahora Angular conoce
donde cargar los componentes dinámicamente.


<code-example path="dynamic-component-loader/src/app/ad-banner.component.ts" region="ad-host" header="src/app/ad-banner.component.ts (template)"></code-example>



El elemento `<ng-template>` es una una buena elección para componentes dinámicos
porque no renderiza ninguna salida adicional.


{@a resolving-components}


## Resolución de componentes

Mira más de cerca los métodos de `ad-banner.component.ts`.

`AdBannerComponent` toma un array de objetos `AdItem` como entrada,
que viene de `AdService`. Los objetos de `AdItem` específican
el tipo de componente a cargar y cualquier dato que se vincule al
componente.`AdService` retorna los anuncios actuales que componen la campaña publicitaria.

Pasando un array de componentes a `AdBannerComponent` permite una
lista dinámica de anuncios sin elementos estáticos en la plantilla.

Con su método `getAds()`, `AdBannerComponent` recorre el array de `AdItems`
y carga un nuevo componente cada 3 segundos llamando a `loadComponent()`.


<code-example path="dynamic-component-loader/src/app/ad-banner.component.ts" region="class" header="src/app/ad-banner.component.ts (excerpt)"></code-example>



El método `loadComponent()` está haciendo bastante trabajo pesado aquí.
Hazlo paso a paso. Primero, elige un anuncio.


<div class="alert is-helpful">



**¿Cómo _loadComponent()_ escoge un anuncio?**

El método `loadComponent()` escoge un anuncio usando algo de matemática.

Primero, coloca el `currentAdIndex` tomando lo que sea
actualmente mas uno, dividiendo eso por el tamaño del array de `AdItem`, y
usando el _sobrante_ como un nuevo valor de `currentAdIndex`. Luego, usa ese 
valor para seleccionar un `adItem` del array.


</div>


Después `loadComponent()` selecciona un anuncio, usa `ComponentFactoryResolver`
para resolver un `ComponentFactory` para cada componente especifico.
El `ComponentFactory` entonces crea una instancia de cada componente.

A continuación, tu estás enfocando el `viewContainerRef` que
existe en esta instancia específica del componente. ¿Cómo sabes que es
esta instancia específica? Porque se está refiriendo a `adHost` y `adHost` es la 
directiva que tú configuraste antes para decirle a Angular donde insertar los componentes dinámicos.

Como recordarás, `AdDirective` inyecta `ViewContainerRef` en su constructor.
Así es como se accede a la directiva del elemento que tu quieres usar como para alojar
el componente dinámico.

Para añadir el componente a la plantilla, tu llamas a `createComponent()` en `ViewContainerRef`.

El método `createComponent()` retorna una referencia del componente cargado.
Usa esa referencia para interactuar con el componente asignando sus propiedades o llamando a sus métodos.



{@a common-interface}


## La interfaz de _AdComponent_

El en banner del anuncio, todos los componentes implementan un interfaz común de `AdComponent` para
estandarizar el API para pasar datos a los componentes.

Aquí hay dos ejemplos de componentes y de la interfaz de `AdComponent` como referencia:


<code-tabs>

  <code-pane header="hero-job-ad.component.ts" path="dynamic-component-loader/src/app/hero-job-ad.component.ts">

  </code-pane>

  <code-pane header="hero-profile.component.ts" path="dynamic-component-loader/src/app/hero-profile.component.ts">

  </code-pane>

  <code-pane header="ad.component.ts" path="dynamic-component-loader/src/app/ad.component.ts">

  </code-pane>

</code-tabs>



{@a final-ad-baner}


## Banner publicitario final
El banner publicitario final se ve como:

<div class="lightbox">
  <img src="generated/images/guide/dynamic-component-loader/ads-example.gif" alt="Ads">
</div>

Mira el <live-example name="dynamic-component-loader"></live-example>.
