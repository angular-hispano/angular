# Configuración TypeScript

TypeScript es el principal lenguaje para el desarrollo de aplicaciones con Angular.
Es un superset de JavaScript con soporte tiempo-diseño para herramientas y seguridad de tipos.

Los navegadores no pueden ejecutar TypeScript directamente. 
Este debe “transpilarse” a JavaScript utilizando el compilador tsc, que requiere de cierta configuración.

Esta pagina cubre algunos de los aspectos de la configuración de TypeScript y su entorno.
Los cuales son importantes para los desarrolladores de Angular, incluyendo algunos detalles sobre los siguientes archivos:

* [tsconfig.json](guide/typescript-configuration#tsconfig)&mdash; Configuración del  compilador de TypeScript.
* [typings](guide/typescript-configuration#typings)&mdash; Archivos de  declaración de TypeScript.


{@a tsconfig}

## Archivos de configuración

Un área de trabajo de Angular contiene varios archivos de configuración de TypeScript.
En el nivel raíz, están los dos archivos de configuración principales: el archivo `tsconfig.json` y el archivo `tsconfig.base.json`.

El archivo `tsconfig.json` es un archivo de ["Solution Style"](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-9.html#support-for-solution-style-tsconfigjson-files) de la configuración de TypeScript. 
Los editores de código y el servidor de TypeScript utilizan este archivo para  mejorar la experiencia de desarrollo. 
Los compiladores no usan  este archivo. 

El archivo `tsconfig.json` contiene una lista de rutas a otros archivos de configuración, utilizados en el área de trabajo.

<code-example lang="json" header="tsconfig.json" linenums="false">
{
 "files": [],
 "references": [
   {
     "path": "./tsconfig.app.json"
   },
   {
     "path": "./tsconfig.spec.json"
   },
   {
     "path": "./projects/my-lib/tsconfig.lib.json"
   }
 ]
}
</code-example>

El archivo `tsconfig.base.json` especifica las opciones básicas del compilador de TypeScript y Angular que heredan todos los proyectos en el área de trabajo. 

TypeScript y Angular tienen una amplia gama de opciones que se pueden usar para configurar funciones de verificación de tipos y salidas generadas.
Para mas información, consulte la sección [Herencia  de configuración extendida](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#configuration-inheritance-with-extends) de la documentación de TypeScript. 

<div class="alert is-helpful">

Para obtener más información sobre los archivos de configuración de TypeScript, consulte la [Wiki oficial de TypeScript](http://www.typescriptlang.org/docs/handbook/tsconfig-json.html). 
Para obtener detalles sobre la herencia en la configuración consulte la sección [Herencia  de configuración extendida](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#configuration-inheritance-with-extends) 

</div>

El archivo inicial `tsconfig.base.json` para un área de trabajo en Angular suele parecerse al siguiente ejemplo.

<code-example lang="json" header="tsconfig.base.json" linenums="false">
{
  "compileOnSave": false,
  "compilerOptions": {
    "baseUrl": "./",
    "outDir": "./dist/out-tsc",
    "sourceMap": true,
    "declarativo": false,
    "downlevelIteration": true,
    "experimentalDecorators": true,
    "moduleResolution": "node",
    "importHelpers": true,
    "target": "es2015",
    "module": "es2020",
    "lib": [
      "es2018",
      "dom"
    ]
  }
}
</code-example>

### Modo estricto

Cuando creas nuevas áreas de trabajo y proyectos, tienes la opción de usar el Modo Estricto de Angular, que puede ayudarte a escribir un código mejor y mas fácil de mantener. 
Para obtener mas información consulte [Modo estricto](/guide/strict-mode).

{@a noImplicitAny}

### *noImplicitAny* y *suppressImplicitAnyIndexErrors*

Los desarrolladores de TypeScript no están de acuerdo sobre si la bandera `noImplicitAny` debería ser `true` o  `false`. 
No hay una respuesta correcta y puede cambiar la bandera más tarde. 
Pero su elección ahora puede marcar la diferencia en proyectos más grandes, por lo que merece una discusión.

Cuando la bandera `noImplicitAny` es `false`(el valor por defecto), y si 
el compilador no puede inferir el tipo de variable en función de cómo se usa, 
el compilador coloca como predeterminado el tipo `any`. Eso es lo que se entiende por *implícito `any`*.

Cuando la bandera `noImplicitAny` es `true`, y el compilador no puede inferir 
el tipo de variable, aun  se generan  los archivos de JavaScript, pero también **muestra un error**. 
Muchos desarrolladores experimentados prefieren esta configuración mas estricta porque la verificación de tipos detecta mas errores 
en el momento de la compilación.

Puedes declarar el tipo de variable como `any` aunque la bandera `noImplicitAny` sea `true`.

Cuando la bandera `noImplicitAny` es `true`, puedes obtener  *implicit index errors*. 
La mayoría  de los desarrolladores sienten que *este error en particular* es mas molesto que útil. 
Puedes suprimirlos añadiendo la siguiente bandera: 

<code-example>

  "suppressImplicitAnyIndexErrors": true

</code-example>

<div class="alert is-helpful">

Para obtener más información sobre cómo afecta la configuración de TypeScript a la compilación, consulte [Opciones del compilador de angular](guide/angular-compiler-options) y [Comprobación del tipo de plantilla](guide/template-typecheck).

</div>


{@a typings}

## Tipeado en TypeScript

Muchas bibliotecas de JavaScript, como jQuery, la biblioteca de pruebas de Jasmine y Angular, 
amplían el entorno de JavaScript con características y sintaxis 
que el compilador de TypeScript no reconoce de forma nativa.
Cuando el compilador no reconoce algo, arroja un error.

Usa [Archivos para la definición de tipos de TypeScript](https://www.typescriptlang.org/docs/handbook/writing-declaration-files.html)&mdash;`d.ts files`&mdash; para decirle al compilador sobre las librerías que cargas. 

Los editores compatibles con TypeScript aprovechan estos mismos archivos de definición para mostar información de tipo sobre las funciones de la biblioteca.

Muchas bibliotecas incluyen archivos de definición en sus paquetes npm donde tanto el compilador de TypeScript como los editores 
puedan encontrarlos. Angular es una de esas bibliotecas.
La carpeta `node_modules/@angular/core/` en cualquier aplicación de Angular contiene varios archivos `d.ts` que describen partes de Angular.

<div class="alert is-helpful">

No necesitas hacer nada para obtener los archivos de *tipado* de la biblioteca de archivos que incluyen los archivos `d.ts`.
Los paquetes de Angular ya los incluyen

</div>

### lib.d.ts

TypeScript incluye un archivo de declaración especial llamado `lib.d.ts`. Este archivo contiene las declaraciones de ambiente para varias construcciones comunes de JavaScript en el tiempo de ejecución de JavaScript y el DOM.

Según el `--target`, TypeScript agrega declaraciones de ambiente _adicionales_ como `Promise` 
si la selección es `es6`.

Por defecto, la selección es `es2015`. Si se selecciona `es5`, tienes declaraciones de tipo, mas nuevas  debido a la lista de archivos de declaración incluidos:

<code-example path="getting-started/tsconfig.0.json" header="tsconfig.json (lib excerpt)" region="lib"></code-example> 

### Archivos de tipos instalables. 

Muchas bibliotecas &mdash;jQuery, Jasmine y Lodash entre ellas&mdash; *no* incluyen archivos `d.ts` en sus paquetes npm. 
Afortunadamente, sus autores o colaboradores de la comunidad han creado archivos `d.ts` separados para estas bibliotecas y 
los han publicado en ubicaciones conocidas.

Puedes instalar estos tipos mediante `npm` usando 
[`@types/*` scoped package](http://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html) 
y TypeScript, a partir de 2.0, los reconoce automáticamente.

Por ejemplo, para instalar los tipos de `jasmine` se ejecuta `npm install @types/jasmine –save-dev`.


{@a target}

### *Selección*

Por defecto, la selección es `es2015`, el cual es soportado unicamente en los navegadores modernos. Puedes configurar la  selección para que `es5` admita  las especificaciones de los navegadores de legado. La CLI de Angular proporciona una [Carga diferencial](guide/deployment#differential-loading) para admitir navegadores modernos y de legado con paquetes separados. 