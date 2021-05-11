# Modulos en JavaScript vs. NgModules

Los modulos en JavaScript y los NgModules ayudan a modularizar tu codigo, pero tienen muchas diferencias.
Las aplicaciones Angular se basan en ambos tipos de modulos.

## Modulos en JavaScript: Archivos que contienen codigo

Un [Modulo en JavaScript](https://javascript.info/modules "JavaScript.Info - Modules") es un archivo con codigo JavaScript que, usualmente contiene una clase o una libreria de funciones para un proposito en especifico dentro de tu aplicacion.
Los modulos en JavaScript te permiten propagar tu trabajo a travez de multiples archivos.

<div class="alert is-helpful">

Para aprender mas acerca de los modulos en JavaScript, puedes visitar [ES6 A Fondo: Modulos](https://hacks.mozilla.org/2015/08/es6-in-depth-modules/).
Para la especificacion del modulo, puedes ver la [6ta Edicion del estandar de ECMAScript](http://www.ecma-international.org/ecma-262/6.0/#sec-modules).

</div>

Para hacer que el codigo de un modulo de JavaScript este disponible para otros modulos, use una declaracion de `export` al final del codigo relevante en el modulo, como la siguiente:

```typescript
export class AppComponent { ... }
```

Cuando necesites el codigo de ese modulo en otro modulo, use una declaracion de `import` de la siguiente manera:

```typescript
import { AppComponent } from './app.component';
```

Cada modulo tiene su propio alcance de nivel superior.
En otras palabras, las variables y funciones de nivel superior en un modulo no son vistas en otros modulos o scrips.
Cada modulo proporciona un namespace para los identificadores esto para prevenir conflictos con otros identificadores en otro modulo.
Con varios modulos, se puede evitar variables globales accidentales creando un unico namespace y aniadiendole sub-modulos.

El framework de Angular en si se carga como un conjunto de modulos de JavaScript

## NgModules: Clases con metadatos para compilar

Un [NgModule](guide/glossary#ngmodule "Definicion de NgModule") es una clase marcada por el decorador `@NgModule` con  un objeto de metadatos que describe como esa particular parte de la aplicacion encaja junto con las otras partes.
NgModules son especificamente para Angular.
Mientras las clases con un decorador `@NgModule` mantienen por convencion sus propios archivos, se diferencian de los modulos de JavaScript porque incluyen estos metadatos.

Los metadatos en `@NgModule` juegan un importante papel en guiar el proceso de compilación de Angular en el que convierte el código de la aplicación que escribes en código JavaScript de alto desempeño.
Los metadatos describen como compilar componentes plantilla y como crear un [injector](guide/glossary#injector "Definicion de injector") en tiempo de ejecución.
IEsto identifica los componentes  [componentes](guide/glossary#component "Definicion de componente"), [directivas](guide/glossary#directive "Definicion de directiva"), y [pipes](guide/glossary#pipe "Definicion de pipe)"),
y hace algunos de ellos públicos a través de la propiedad ` exports` así esos componentes externos pueden usarlos.

Puedes también usar un NgModule para agregar [providers](guide/glossary#provider "Definicion de provider") para [services](guide/glossary#service "Definicion of a service"), de modo que esos servicios estén disponibles en cualquier lugar de tu aplicación.

En lugar de definir todas las clases en un archivo gigante como un modulo de JavaScript, declare cuales componentes, directivas y pipes pertenecen al NgModule en la lista de declaraciones `@NgModule.declarations`.
Estas clases se llaman [declarables](guide/glossary#declarable "Definicion of a declarable").
Un NgModule puede exportar solo las clases declarables que posee o las importadas desde otros NgModules.
No puede exportar o declarar ningún otro tipo de clases.
Estas Declarables son solo clases que importan en el proceso de compilación de Angular.

Para una complete descripción de las propiedades de los metadatos de NgModule, ver [El Uso de los metadatos de  NgModule metadata](guide/ngmodule-api "Usando la metadata de NgModule").

## Un ejemplo usando los dos

La raiz de NgModule `AppModule` generado por el [Angular CLI](cli) para un nuevo proyecto demuestra como se usan las dos clases de modulos:

<code-example path="ngmodules/src/app/app.module.1.ts" header="src/app/app.module.ts (default AppModule)"></code-example>

La raiz de NgModule empieza con la declaracion `import` para importar los modulos de JavaScript.
Esto luego configura el `@NgModule` con los siguientes arreglos:

* `declarations`: Los componentes, directivas y pipes que pertenecen al NgModule.
  La raiz de un nuevo proyecto o aplicacion tiene un solo componente, llamado `AppComponent`.

* `imports`: Otro NgModules que usas, para que tu puedas usar sus declarables.
  La recien generada raiz de NgModule importa [`BrowserModule`](api/platform-browser/BrowserModule "BrowserModule NgModule") para usar servicios especificos como [DOM](https://www.w3.org/TR/DOM-Level-2-Core/introduction.html "Definicion de Document Object Model") representación, desinversización y ubicación.

* `providers`: Providers de servicios que los componentes en otros NgModules puedan usar.
  No hay providers en un NgModule recien generado.

* `bootstrap`: El [componente de entrada](guide/entry-components "Especificando un componente de entrada") que Angular creo e inserto dentro del host de la pagina web `index.html`, de este modo se hace bootstrapping de la aplicacion.
  Este componente de entrada, `AppComponent`, aparece en las dos `declarations` y en los arreglos `bootstrap`.

## Siguientes Pasos

* Para mas acerca de NgModules, ver [Organizando su aplicacion con NgModules](guide/ngmodules "Organizando su aplicacion con NgModules").
* Para aprender mas acerca de la raiz de NgModule, ver [Lanzando aplicaciones con la raiz de NgModule](guide/bootstrapping "Lanzando aplicaciones con la raiz de NgModule").
* Para aprender sobre el uso frecuente de Angular NgModules y como importarlos dentro de tu aplicacion, ver [Uso frecuente de modulos](guide/frequent-ngmodules "Uso frecuente de modulos").
