# Componentes de Entrada

Un componente de entrada es cualquier componente que Angular carga imperativamente, (lo que significa que no está referenciando hacia la plantilla) por tipo.
Tú especificas un componente de entrada inicializándolo en NgModule, o incluyéndolo en la definición de ruta.

<div class="alert is-helpful">

Para contrastar los dos tipos de componentes, existen componentes que son incluidos en la plantilla, los cuales son declarativos.
Adicionalmente existen componentes los cuales puedes cargarlos de forma imperativa; eso es un componente de entrada.

</div>


Hay dos principales tipos de componentes de entrada:

* El componente inicializado de raíz.
* Un componente especificado en una ruta definida.


## Inicio de un Componente de entrada


Lo siguiente es un ejemplo de especificar un inicio de componente,
`AppComponent`, en un básico `app.module.ts`:

```typescript
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpClientModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent] // Inicio del componente de entrada
})
```

Un componente bootstrapped es un componente de entrada,
Eso Angular lo carga dentro del DOM durante el proceso de arranque (lanzamiento de la aplicación).
Otros componentes de entrada son cargados dinámicamente por otros medios, como con el enrutador.

Angular carga de inicio `AppComponent` dinámicamente porque está siendo escuchado en el `@NgModule.bootstrap`.

<div class="alert is-helpful">

Un componente puede también ser arrancado imperativamente en los módulos con el método `ngDoBootstrap()`.
La propiedad `@NgModule.bootstrap` le dice al compilador que esto es un componente de entrada y
debería generar el código de arranque de la aplicación con este componente.

</div>


Un componente inicializado es necesariamente un componente de entrada porque es inicializado en un proceso imperativo, por lo tanto, debe tener un componente de entrada.
## Un componente de entrada ruteado

El segundo tipo de componente de entrada ocurre en una definición de ruta como esta:

```typescript
const routes: Routes = [
  {
    path: '',
    component: CustomerListComponent
  }
];
```

Una ruta definida que se refiera hacia un componente por su tipo con `component: CustomerListComponent`.

Todos los componentes de ruta deben ser componentes de entrada. Porque esto requeriría que agregues el componente en dos lugares (en el enrutador y en `entryComponents`)El Compilador es lo suficientemente listo para reconocer eso es un enrutador definido y automáticamente agrega la el componente enrutado dentro de `entryComponents`.


## El array de `entryComponents`

Aunque el decorador `@NgModule` tiene un array de  `entryComponents`, la mayoría del tiempo
tú no tienes que establecer explícitamente ningún componente de entrada porque Angular agrega los componentes listados en `@NgModule.bootstrap` y los que están definidos en la ruta como componentes de entrada de forma automática.Aunque estos dos mecanismos representan la mayoría de los componentes de entrada,si tu aplicación pasa de arranque o cargar de forma dinámica un componente por tipo imperativo,
deberías agregarlo a `entryComponents` explícitamente.

### `entryComponents` y el compilador

Para aplicaciones de producción quieres cargar el menor código posible.
El código debería contener solo las clases que actualmente tú necesitas y
excluir componentes que nunca son usados. Por esta razón, el compilador de Angular solo genera código para componentes que son accesibles desde el `entryComponents`; Esto significa que agregando más referencias a `@NgModule.declarations` no implica que necesariamente serán incluidas el empaquetado final.

De hecho, muchas librerías declaran y exportan componentes que tú jamás usaras.
Por ejemplo, Una librería de material design exportara todos los componentes porque no conoce cuáles usaras. Sin embargo, no es como si tú usaras todos ellos.
Para los que no haces referencia, El "tree shaker" descarta esos componentes para el empaquetado del código final.

Si un componente no es un _entry component_ y  no es encontrado en una plantilla,
el "tree shaker" lo sacará del camino. Entonces, es mejor agregar únicamente los componentes que realmente son componentes de entrada para ayudar a mantener tu aplicación lo más limpia posible.


<hr />

## Más en módulos de Angular

Puede que estés interesado en continuar:
* [Tipos de NgModules](guide/module-types)
* [Modulos cargados Lazy con el enrutador de Angular](guide/lazy-loading-ngmodules).
* [Providers](guide/providers).
* [NgModules FAQ](guide/ngmodule-faq).
