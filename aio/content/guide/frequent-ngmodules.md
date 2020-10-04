# Módulos utilizados frecuentemente

Una app de Angular requiere al menos un módulo que funciona como el módulo raíz.
Puedes agregar nuevas funcionalidades a tu app enforma de módulos.
A continuación encontrarás módulos frecuentemente utilizados en Angular 
con ejemplos de lo que contienen:

<table>

 <tr>
   <th style="vertical-align: top">
     NgModule
   </th>

   <th style="vertical-align: top">
     Impórtalo desde
   </th>

   <th style="vertical-align: top">
     Para que sirve
   </th>
 </tr>

 <tr>
   <td><code>BrowserModule</code></td>
   <td><code>@angular/platform-browser</code></td>
   <td>Permite correr tu app en un navegador</td>
 </tr>

 <tr>
   <td><code>CommonModule</code></td>
   <td><code>@angular/common</code></td>
   <td>Permite utilizar <code>NgIf</code>, <code>NgFor</code></td>
 </tr>

 <tr>
   <td><code>FormsModule</code></td>
   <td><code>@angular/forms</code></td>
   <td>Permite utlizar formularios basados en plantillas  (incluye <code>NgModel</code>)</td>
 </tr>

 <tr>
   <td><code>ReactiveFormsModule</code></td>
   <td><code>@angular/forms</code></td>
   <td>Permite crear formularios reactivos</td>
 </tr>

 <tr>
   <td><code>RouterModule</code></td>
   <td><code>@angular/router</code></td>
   <td>Permite utlizar <code>RouterLink</code>, <code>.forRoot()</code>, y <code>.forChild()</code></td>
 </tr>

 <tr>
   <td><code>HttpClientModule</code></td>
   <td><code>@angular/common/http</code></td>
   <td>Permite comunicarte con un servidor</td>
 </tr>

</table>

## Importación de módulos

Al utilizar estos módulos de Angular, importalos dentro de `AppModule`,
o en tu modulo de funcionalidades en caso de ser necesario, y enlístalos 
en el array de `imports` de `@NgModule`. Por ejemploe, en la app básica 
generada por la [Angular CLI](cli), `BrowserModule` es el primero en ser importado
al inicio de `AppModule`, `app.module.ts`.


```typescript
/* importa los módulos de manera que AppModule pueda accederlos */
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [ /* agrega los módulos aquí para que Angular pueda utilizarlos */
    BrowserModule,
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Los imports al inicio del array son declaraciones de importación de Javascript
mientras que el array de `imports` dentro de `@NgModule` es específico de Angular.
Para más información sobre sus diferencias, consulta  [JavaScript Modules vs. NgModules](guide/ngmodule-vs-jsmodule).


## `BrowserModule` y `CommonModule`

`BrowserModule` importa a `CommonModule`, el cual contibuyte con muchas
directivas comunes como `ngIf` y `ngFor`. Adicionalmente, `BrowserModule`
exporta nuevamente a `CommonModule` haciendo que todas sus directivas
estén disponibles a cualquier módulo que importe a `BrowserModule`.

Para apps que corran en el navegador, importa `BrowserModule` en la 
raíz `AppModule` ya que así proveerá los servicios escenciales para 
iniciar y correr la app en el navegador. Los proveedores de `BrowserModule`
son para toda la aplicación por lo que debería de estar sólamente dentro
del módulo raíz, no en módulos de funcionalidades. Los módulos de funcionalidades
solamente necesitan de las directivas comunes encontradas en `CommonModule`;
no necesitan volver a instalar proveedores que sirven para toda la aplicación.

Si llegas a importar `BrowserModule` dentro de un módulo de funcionalidad 
con carga diferida, Angular regresará un error diciéndote que utilizes `CommonModule`.

<div class="lightbox">
  <img src="generated/images/guide/frequent-ngmodules/browser-module-error.gif" width=750 alt="BrowserModule error">
</div>

<hr />


## Más sobre NgModules

Puede que estés interesado en lo siguiente:
* [Bootstrapping](guide/bootstrapping).
* [NgModules](guide/ngmodules).
* [JavaScript Modules vs. NgModules](guide/ngmodule-vs-jsmodule).
