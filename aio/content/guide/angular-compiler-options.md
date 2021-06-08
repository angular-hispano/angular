# Opciones del compilador de Angular

Cuando se hace uso de la [compilación anticipada](guide/aot-compiler), es posible controlar cómo es compilada la aplicación especificando una *plantilla* de opciones de compilación en el [archivo de configuración de TypeScript](guide/typescript-configuration).

El objeto de las opciones de plantilla, `angularCompilerOptions`, es un similar al objeto `compilerOptions` que provee opciones estándar al compilador de Typescript.

```json
    {
      "compilerOptions": {
        "experimentalDecorators": true,
                  ...
      },
      "angularCompilerOptions": {
        "fullTemplateTypeCheck": true,
        "preserveWhitespaces": true,
                  ...
      }
  }
  ```

{@a tsconfig-extends}
## Herencia de la configuración usando "extends"

Al igual que el compilador de Typescript, el compilador AOT de Angular soporta `extends` en la sección del archivo de configuración de Typescript `angularCompilerOptions`. La propiedad `extends` se encuentra en el nivel superior, paralelo a las propiedades `compilerOptions` y `angularCompilerOptions`.

Una configuración de TypeScript puede heredar configuraciones de otro archivo haciendo uso de la propiedad `extends`. Las opciones de configuración del archivo base son cargadas primero, y después sobreescritas por las opciones del archivo de configuración que hereda al archivo base.

Por ejemplo:

```json
{
  "extends": "../tsconfig.base.json",
  "compilerOptions": {
    "experimentalDecorators": true,
    ...
  },
  "angularCompilerOptions": {
    "fullTemplateTypeCheck": true,
    "preserveWhitespaces": true,
    ...
  }
}
```

Para más información, puede consultar el [Manual de TypeScript](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html).

## Opciones de plantilla

Las siguientes opciones están disponibles para configurar la plantilla del compilador AOT.

### `allowEmptyCodegenFiles`

Cuando el valor es `true`, genera todos los archivos posibles sin importar si están vacíos. El valor predeterminado es `false`. Utilizado por las reglas de construcción de Bazel para simplificar la forma en que las reglas de Bazel hacen seguimiento a las dependencias de archivos. No utilizar esta opción fuera de las reglas de Bazel.

### `annotationsAs`

Modifica la forma en que las anotaciones específicas de Angular son emitidas para mejorar la etapa de _tree-shake_. Las anotaciones que no son Angular no son afectadas. Uno de `static fields` (el valor predeterminado) o `decorators`.

* Por defecto, el compilador reemplaza los decodadores con un campo estático en la clase, lo que permite _tree-shakers_ avanzados como el [Closure compiler](https://github.com/google/closure-compiler) para remover clases que no son utilizadas.

* El valor de `decorators` deja los decoradores en su lugar, lo que conlleva a una compilación más rápida. TypeScript genera llamadas al auxiliar ` __decorate`. Utiliza `--emitDecoratorMetadata` para _runtime reflection_ (ten en cuenta que el código resultante no hará _tree-shake_ de forma apropiada).

### `annotateForClosureCompiler`

Cuando el valor es `true`, utiliza [Tsickle](https://github.com/angular/tsickle) para anotar en el JavaScript generado, comentarios [JSDoc](http://usejsdoc.org/) necesitados por el [Closure Compiler](https://github.com/google/closure-compiler). El valor por defecto es `false`.

### `disableExpressionLowering`

Cuando el valor es `true` (valor predeterminado), transforma código que es o puede ser utilizado en una anotación, para permitirle ser importado desde módulos de fábrica de plantillas. Consultar [Re-escribiendo metadatos](guide/aot-compiler#metadata-rewriting) para más información.

Cuando el valor es `false`, deshabilita esta reescritura, requiriendo que la reescritura sea hecha de forma manual.

### `disableTypeScriptVersionCheck`

Cuando el valor es `true`, el compilador no verifica la versión de TypeScript y no informa de un error cuando una versión no soportada de TypeScript is utilizada. No se recomienda, puesto que las versiones no soportadas de TypeScript pueden tener un comportamiento indefinido. El valor por defecto es `false`.

### `enableIvy`

Habilita la compilación [Ivy](guide/ivy) y el pipeline de renderización. El valor por defecto es `true`, a partir de la versión 9. Para la versión 9, es posible la [no utilización de Ivy](guide/ivy#opting-out-of-angular-ivy) para continuar utilizando el compilador anterior, View Engine.

Para proyectos de librería generados con la CLI, la configuración de producción (`prod`) establece el valor de la opción a `false` en forma predeterminada para la versión 9.

### `enableResourceInlining`

Cuando el valor es `true`, reemplaza las propiedades `templateUrl` y `styleUrls` en todos los decoradores `@Component` con contenido en línea en las propiedades `template` y `styles`.

Cuando se habilita, la salida `.js` de `ngc` no incluye ninguna plantilla en carga diferida o URLs de estilo.

Para proyectos de librería generados con la CLI, la configuración de desarrollo predetermina el valor de la opción a `true`.


{@a enablelegacytemplate}

### `enableLegacyTemplate`

Cuando el valor es `true`, habilita el uso del elemento `<template>`, el cual dejo de ser soportado en Angular 4.0, en favor de `<ng-template>` (para evitar colisiones con los elementos del DOM con el mismo nombre). El valor por defecto es `false`. Puede ser requerido por alguna librerías de Angular de terceros.

### `flatModuleId`

El ID del módulo a ser usado para importar un módulo plano "_flat module_"  (cuando `flatModuleOutFile` es `true`). Las referencias generadas por el compilador de plantillas utiliza este nombre de módulo al momento de importar símbolos del módulo plano "_flat module_". Es ignorado si el valor de `flatModuleOutFile` es `false`.

### `flatModuleOutFile`

Cuando el valor es `true`, genera un índice de módulo plano (_flot module_) del nombre de archivo dado y los metados del módulo plano correspondientes. Se utiliza para crear módulos planos que son empaquetados de manera simular a `@angular/core` y `@angular/common`. Cuando se utiliza esta opción, el `package.json` para la librería debe referirse al índice del módulo plano generado en lugar de al archivo índice de la librería.

Genera sólo un archivo `.metadata.json`, el cual contiene todos los metadatos necesarios
para los símbolos exportados del índice de la librería. En los archivos `.ngfactory.js` generados, el índice del módulo plano
es utilizado para importar símbolos que incluyen tanto la API pública del índice de la librería
como símbolos internos envueltos (_shrowded internal symbols_).

Por defecto el archivo `.ts` provisto en el campo `files` se toma como el índice de la librería.
Si más de un arhivo `.ts` es especificado, `libraryIndex` es utilizado para seleccionar el archivo a utilizar.
Si más de un archivo `.ts` es provisto sin un `libraryIndex`, se generará un error.

Un índice de módulo plano `.d.ts` y `.js` es generado con el nombre dado en `flatModuleOutFile` en la misma dirección que el archivo índice de librería `.d.ts`.

Por ejemplo, si una librerías utiliza el archivo `public_api.ts` como índice de librería del módulo, el campo `tsconfig.json` `files` sería `["public_api.ts"]`.
La opción `flatModuleOutFile` puede ser establecida a (por ejemplo) `"index.js"`, lo que genera los archivos `index.d.ts` y `index.metadata.json`.
El campo `module` del `package.json` de la librería sería `"index.js"` y el campo `typings` sería `"index.d.ts"`.

### `fullTemplateTypeCheck`

Cuando el valor es `true` (valor recomendado), habilita la fase de validación de expresiones enlazantes ([binding expression validation](guide/aot-compiler#binding-expression-validation)) del compilador de plantillas, que utiliza TypeScript para validar las expresiones enlazantes (_binding expressions_). Para más información, consultar la [Verificación de tipos en plantillas](guide/template-typecheck).

El valor predeterminado es `false`, pero cuando se utiliza en la CLI el comando `ng new`, el valor se establece en `true` por defecto en la configuración del proyecto generado.

### `generateCodeForLibraries`

Cuando el valor es `true` (valor predeterminado), genera los archivos de fábrica (`.ngfactory.js` and `.ngstyle.js`)
para los archivos `.d.ts` que tienen un archivo `.metadata.json` correspondiente.

Cuando el valor es `false`, lo archivos de fábrica son generados únicamente para los archivos `.ts`. Usarlo de esta forma solamente cuando se utilizan _factory summaries_.


### `preserveWhitespaces`

Cuando el valor es `false` (valor predeterminado), elimina nodos de texto en blanco (espacios o _whitespaces_) de las plantillas compiladas, lo cual resulta en que los módulos de fábrica de plantillas generadas son de un menor tamaño. Establecer el valor en `true` para preservar nodos de texto en blanco.

### `skipMetadataEmit`

Cuando el valor es `true`, no genera los archivos `.metadata.json`. El valor predeterminado es `false`.

Los archivos `.metadata.json` contienen información necesaria por el compilador de plantillas para un archivo `.ts`
que no se encuentre incluido en el archivo `.d.ts` generado por el compilador de TypeScript.
La información incluye, por ejemplo, el contenido de las anotaciones (como la plantilla de un componente), el cual TypeScript agrega al archivo `.js` pero no al archivo `.d.ts`.

Puedes establecer el valor en `true` cuando haces uso de _factory summaries_ (fábricas), porque las _factory summaries_
incluyen una copia de la información que se encuentra en el archivo `.metadata.json`.

Establece el valor en `true` si estás utilizando la opción `--outFile` de TypeScript, debido a que los archivos de metadatos
no son válidos para este estilo de salida (_output_) de TypeScript. De cualquier manera, no se recomienda utilizar la opción `--outFile` con Angular. Utiliza una herramienta de compilación (empaquetador o _bundler_), como [webpack](https://webpack.js.org/), en su lugar.

### `skipTemplateCodegen`

Cuando el valor es `true`, no genera los archivos `.ngfactory.js` y `.ngstyle.js`. Esto deshabilita en su mayoría el compilador de plantillas y deshabilita el informe de diagnósticos de plantilla.

Puede ser utilizada para indicarle al compilador de plantillas que debe generar los archivos `.metadata.json` para ser distribuidos con un paquete de `npm` y al mismo tiempo evitar generar los archivos `.ngfactory.js` y `.ngstyle.js` que no pueden ser distribuidos hacia `npm`.

Para proyectos de librería generados con la CLI, la configuración de desarrollo predetermina el valor de la opción a `true`.

### `strictMetadataEmit`

Cuando el valor es `true`, informa un error al archivo `.metadata.json` siempre y cuando el valor de la opción `"skipMetadataEmit"` sea `false`.
El valor predeterminado es `false`. Se debe utilizar sólo cuando las opciones: `"skipMetadataEmit"` es `false` y `"skipTemplateCodeGen"` es`true`.

El objetivo de esta opción es de validar los archivos `.metadata.json` producidos por el empaquetamiento con un paquete de `npm`. La validación es estricta y puede producir errores por metadatos que nunca producirían un error cuando es utilizado el compilador de plantillas. Es posible eliminar el error producido por esta opción para un símbolo exportado al incluir `@dynamic` en el comentario que documenta el símbolo.

Es válido que los archivos `.metadata.json` contengan errores.
El compilador de plantillas informa estos errores si los metadatos son utilizados para determinar el contenido de una anotación.
El recolector de metadatos no es capaz de predecir los símbolos que fueron diseñados para utilizarse en una anotación, así que de forma preventiva incluye errores de nodo en los metadatos para los símbolos exportados.
El compilador de plantillas puede entonces utilizar los errores de nodo para reportar un error si estos símbolos son utilizados.

Sí el cliente de una librería pretende utilizar un símbolo en una anotación, el compilador de plantillas no lo informa normalmente, al menos hasta que sea utilizado el símbolo.
Esta opción permite la detección de estos errores durante la fase de construcción de la librería y se utiliza, por ejemplo, en la creación de librerías de Angular.

Para proyectos de librería generados con la CLI, la configuración de desarrollo predetermina el valor de la opción a `true`.

### `strictInjectionParameters`

Cuando el valor es `true` (recomendado), informa un error para un parámetro suministrado cuyo tipo de inyección no puede ser determinado. Cuando el valor es `false` (actualmente el valor predeterminado), los parámetros constructores de clases marcados con el decorador `@Injectable` cuyo tipo no pueda ser resuelto generarán una advertencia (warning).

Cuando utilizas el comando `ng new --strict`, el valor de `strictInjectionParameters` se establece a `true` en la configuración del proyecto generado.

### `strictTemplates`

Cuando el valor es `true`, habilita el [strict template type checking](guide/template-typecheck#strict-mode) en la versión 9 de Angular. El modo estricto (strict mode) sólo se encuentra disponible cuando se utiliza [Ivy](guide/ivy).

Existen opciones de severidad (strictness flags) adicionales que permiten habilitar o deshabilitar modos específicos para la verificación estricta de tipos en plantillas (strict template type checking). Consultar [Solucionando errores en plantillas (troubleshooting template errors)](guide/template-typecheck#troubleshooting-template-errors).

Cuando utilizas el comando `ng new --strict`, el valor de `strictTemplates` se establece a `true` en la configuración del proyecto generado.

### `trace`

Cuando el valor es `true`, imprime información extra durante la compilación de las plantillas. El valor predeterminado es `false`.
