# El editor de Héroe

La aplicación ahora tiene un título básico.
A continuación, creará un nuevo componente para mostrar la información del héroe y colocará ese componente en el armazón de la aplicación.
<div class="alert is-helpful">

  Para ver la aplicación de ejemplo que describe esta página, consulte el <live-example></live-example>.

</div>

## Crear un componente de héroes

Use el CLI angular para generar un nuevo componente llamado `heroes`.

<code-example language="sh" class="code-shell">
  ng generate component heroes
</code-example>

El CLI crea una nueva carpeta llamada `src/app/heroes/`,
Genera tres archivos sobre `HeroesComponent` junto con un archivo de prueba.

El archivo de la clase `HeroesComponent` es el siguiente:

<code-example path="toh-pt1/src/app/heroes/heroes.component.ts" region="v1" header="app/heroes/heroes.component.ts (initial version)"></code-example>

Importe siempre el símbolo `Component` de la biblioteca central de Angular,
Anote la clase de componente con `@Component`.

`@Component`  es un decorador que especifica metadatos de Angular para un componente.

El CLI generó 3 propiedades de metadatos:

1. `selector`&mdash; El selector de elementos CSS para el componente
1. `templateUrl`&mdash; La ubicación del archivo de plantilla para el componente
1. `styleUrls`&mdash; La ubicación de los estilos CSS privados del componente.

{@a selector}

El [Selector de elementos CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/Type_selectors)
`'app-heroes'` coincide con el nombre del elemento HTML que identifica este componente en el componente padre Plantillas.

El `ngOnInit()` es un [gancho de ciclo de vida](guide/lifecycle-hooks#oninit)("lifecycle hook"). Angular llama a `ngOnInit ()` inmediatamente después de crear el componente.
Adecuado para poner la lógica de inicialización.

Siempre `exporta` la clase de componente, por lo que siempre puede` importarla 'en otro lugar, como un `AppModule`.

### Agregue la propiedad `hero`

Agregue una propiedad `hero` al `HeroesComponent` para un héroe llamado "Windstorm".

<code-example path="toh-pt1/src/app/heroes/heroes.component.ts" region="add-hero" header="heroes.component.ts (hero property)"></code-example>

### Mostrar heroe 

Abra el archivo `heroes.component.html` Plantillas.
Elimine el texto predeterminado generado por el CLI angular,
Reemplácelo con un enlace de datos a la nueva propiedad `hero`.

<code-example path="toh-pt1/src/app/heroes/heroes.component.1.html" header="heroes.component.html" region="show-hero-1"></code-example>

## Mostrar la vista `HeroesComponent` 

Para ver el `HeroesComponent`, debe agregarlo a las Plantillas en el `AppComponent` del shell de su aplicación.

Recuerde que `app-heroes` es el [selector de elemento](#selector) del `HeroesComponent`.
Entonces, en el archivo Plantillas de `AppComponent`, agregue el elemento `<app-heroes> ` directamente debajo del título.

<code-example path="toh-pt1/src/app/app.component.html" header="src/app/app.component.html"></code-example>

Si el comando CLI `ng serve` todavía se está ejecutando,
El navegador se actualiza para mostrar el título de la aplicación y el nombre del héroe.

## Crear interfaz de héroe

El un héroe es más que un nombre.

Cree una interfaz `Hero` en su propio archivo en la carpeta `src/app`.
Déle una propiedad `id` y una propiedad `name`.

<code-example path="toh-pt1/src/app/hero.ts"  header="src/app/hero.ts"></code-example>


Regrese a la clase `HeroesComponent` e importe la interfaz `Hero`.

Refactorice la propiedad de héroe del componente para que sea del tipo 'Héroe'.
Inicialícelo con un `id` de `1` y un nombre de `Windstorm`.

El archivo de clase revisado `HeroesComponent` se ve así:

<code-example path="toh-pt1/src/app/heroes/heroes.component.ts" header="src/app/heroes/heroes.component.ts"></code-example>

Cambió el héroe de texto a un objeto, lo que provocó que la página se mostrara incorrectamente.

## Mostrar objeto de héroe 

Actualice los enlaces de Plantillas para anunciar el nombre del héroe,
Muestre tanto el `id` como el `name` con un diseño detallado como este:

<code-example path="toh-pt1/src/app/heroes/heroes.component.1.html" region="show-hero-2" header="heroes.component.html (HeroesComponent's template)"></code-example>

El navegador se actualiza para mostrar la información del héroe.

## Formatee con _UppercasePipe_ 

Modifique el enlace para `hero.name` de esta manera:
<code-example path="toh-pt1/src/app/heroes/heroes.component.html" header="src/app/heroes/heroes.component.html" region="pipe">
</code-example>

El navegador se actualizará para mostrar el nombre del héroe en mayúsculas.

En el enlace de interpolación, la palabra `mayúscula` inmediatamente después del operador de tubería (|) es
Inicie el 'Tubo(Pipe) en mayúscula' incorporado.

[tuberia](guide/pipes) ("pipe") Es adecuado para formatear cadenas, importes monetarios, fechas y otros datos de visualización.
Angular viene con múltiples tuberías incorporadas, y puede crear las suyas propias.

## Editar héroe

Los usuarios deberían poder editar el nombre del héroe en un cuadro de texto `<input>`.

El cuadro de texto debe _mostrar_ la propiedad `name` del héroe
y _actualizar_ esa propiedad a medida que el usuario escribe.
Eso significa que los datos fluyen desde la clase de componente _hacia la pantalla_ y
de la pantalla _devuelta a la clase_.

Para automatizar ese flujo de datos, configure un enlace de datos bidireccional entre el elemento de formulario `<input>` y la propiedad `hero.name`.

### Enlace de datos bidireccional

Refactorizando el área de detalle de las Plantas `HeroesComponent` se ve así:

<code-example path="toh-pt1/src/app/heroes/heroes.component.1.html" region="name-input" header="src/app/heroes/heroes.component.html (HeroesComponent's template)"></code-example>

**[(ngModel)]** Es la sintaxis de enlace de datos bidireccional de Angular.

Aquí enlaza la propiedad `hero.name` al cuadro de texto HTML para que los datos puedan fluir _en ambas direcciones:_ desde la propiedad `hero.name` al cuadro de texto, y desde el cuadro de texto de regreso al `hero.name`.

### No encontrado _FormsModule_ 

Observe que la aplicación dejó de funcionar cuando agregué el `[(ngModel)]`.

Para ver el error, abra las herramientas de desarrollo de su navegador,
Busque mensajes como el siguiente en la consola,

<code-example language="sh" class="code-shell">
Errores de análisis de plantilla:
No se puede vincular a 'ngModel' ya que no es una propiedad conocida de 'entrada'.
</code-example>

`ngModel` Es una directiva angular válida pero no está disponible por defecto.

Pertenece al `FormsModule` opcional y debe optar por ese módulo para usarlo.

## _AppModule_

En Angular, cómo encajan las partes de la aplicación,
Necesita saber qué otros archivos y bibliotecas necesita su aplicación.
Esta información se llama _metadata_.

Algunos de los metadatos se encuentran en el decorador `@ Component` que agregó a su clase de componentes.
Otros metadatos importantes son [`@NgModule`](guide/ngmodules)Está en el decorador.

El decorador más importante `@NgModule` anota la clase ** AppModule ** de nivel superior.

Angular CLI creó la clase `AppModule` en `src/app/app.module.ts` al crear el proyecto.
Ahora opta por el `FormsModule`.

### Importar _FormsModule_

Abra `AppModule`(` app.module.ts`) e importe el símbolo `FormsModule` desde la biblioteca `@angular/forms`.

<code-example path="toh-pt1/src/app/app.module.ts" header="app.module.ts (@NgModule imports)"
 region="formsmodule-js-import">
</code-example>

A continuación, agregue el `FormsModule` a el arreglo `imports` de los metadatos `@ NgModule`.
Esta matriz contiene una lista de módulos externos que requiere su aplicación.

<code-example path="toh-pt1/src/app/app.module.ts" header="app.module.ts ( @NgModule imports)"
region="ng-imports">
</code-example>

La aplicación debería funcionar nuevamente cuando se actualice el navegador. Puedes editar el nombre del héroe y ver los cambios reflejados inmediatamente en el `<h2>` arriba del cuadro de texto.

### Declarar `HeroesComponent` 

Todos los componentes deben declararse con _exactamente uno_ [NgModule](guide/ngmodules).

_No ha declarado_ `HeroesComponent`.
Entonces, ¿por qué funcionó la aplicación?

La aplicación funcionó porque Angular CLI declaró el componente en el `AppModule` cuando generó el `HeroesComponent`.

Abra `src/app/app.module.ts` y encuentre el `HeroesComponent` importado cerca de la parte superior.

<code-example path="toh-pt1/src/app/app.module.ts" header="src/app/app.module.ts" region="heroes-import" >
</code-example>

`HeroesComponent` se declara en la matriz `@NgModule.declarations`.
<code-example path="toh-pt1/src/app/app.module.ts" header="src/app/app.module.ts" region="declarations">
</code-example>

`AppModule` declara los componentes de aplicación `AppComponent` y `HeroesComponent`.


## Revisión del código final

Los archivos de código descritos en esta página son:

<code-tabs>

  <code-pane header="src/app/heroes/heroes.component.ts" path="toh-pt1/src/app/heroes/heroes.component.ts">
  </code-pane>

  <code-pane header="src/app/heroes/heroes.component.html" path="toh-pt1/src/app/heroes/heroes.component.html">
  </code-pane>

  <code-pane header="src/app/app.module.ts" 
  path="toh-pt1/src/app/app.module.ts">
  </code-pane>

  <code-pane header="src/app/app.component.ts" path="toh-pt1/src/app/app.component.ts">
  </code-pane>

  <code-pane header="src/app/app.component.html" path="toh-pt1/src/app/app.component.html">
  </code-pane>

  <code-pane header="src/app/hero.ts" 
  path="toh-pt1/src/app/hero.ts">
  </code-pane>

</code-tabs>
## Resumen 

* Usó el CLI para crear un segundo "Componente de Héroes".
* Agregó `HeroesComponent` al caparazón de `AppComponent` y lo mostró.
* Aplico `UppercasePipe` para formatear el nombre.
* Utilizo el enlace de datos bidireccional(two-way data binding) en la directiva `ngModel`.
* Aprendío sobre `AppModule`.
* Importó `FormsModule` en `AppModule` para reconocer y aplicar la directiva Angular `ngModel`.
* Aprendío la importancia de declarar un componente en un `AppModule` y se dio cuenta de que el CLI está haciendo esa declaración por usted.