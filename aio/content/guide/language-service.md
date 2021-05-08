# Servicio del Lenguaje Angular

El servicio de Angular proporciona a los editores de código una forma de obtener finalización, errores, pistas, y navegación dentro de las plantillas de Angular.
Funciona con plantillas externas en archivos HTML separados, y también con plantillas en línea.

## Características

Su editor autodetecta que está abriendo un archivo angular, luego usa el servicio de Angular para leer su archivo `tsconfig.json`, encontrar todas las plantillas que tengan en su aplicación, y luego proporcionar un servicio de lenguaje para cualquier plantilla que abra.

Los servicios del lenguaje incluyen:

* Listas de finalizaciones
* Mensajes de diagnóstico
* Información rápida
* Definiciones


### Autocompletado

El autocompletado puede acelerar el tiempo de desarrollo al proporcionarle posibilidades contextuales y sugerencias a medida que se escribe.
Este ejemplo muestra el autocompletado en una interpolación. A medida que lo 
escribe, puede presionar la pestaña para completar.

<div class="lightbox">
  <img src="generated/images/guide/language-service/language-completion.gif" alt="autocompletion">
</div>

También hay finalizaciones dentro de los elementos. Cualquier elemento que tenga como 
selector de componentes aparecerá en la lista de finalización.

### Comprobación de errores

El servicio del lenguaje de Angular puede prevenir errores en su código.
En este ejemplo, Angular no sabe qué `pedidos` son o de dónde vienen.

<div class="lightbox">
  <img src="generated/images/guide/language-service/language-error.gif" alt="error checking">
</div>

### Información rápida y navegación

La característica de la información rápida permite desplazarse para ver dónde están los componentes, directivas, módulos, y así sucesivamente. 
Puede hacer clic "ir a definición" o presionar F12 para ir directamente a la definición.

<div class="lightbox">
  <img src="generated/images/guide/language-service/language-navigation.gif" alt="navigation">
</div>


## Servicio de lenguaje angular en su editor

El servicio de lenguaje Angular está actualmente habilitado con una extensión para 
[Visual Studio Code](https://code.visualstudio.com/), [WebStorm](https://www.jetbrains.com/webstorm), y [Sublime Text](https://www.sublimetext.com/).

### Visual Studio Code

En [Visual Studio Code](https://code.visualstudio.com/), instalar la extensión desde [Extensiones: Marketplace](https://marketplace.visualstudio.com/items?itemName=Angular.ng-template). Puede abrir el Marketplace desde el editor usando el icono en el panel de menú de la izquierda, o use  (⌘+P para Mac, CTRL+P para Windows) y escriba "la extensión".

En el marketplace, buscar extensiones para servicio del lenguaje Angular, y dar clic en el botón **Install**

###  Entorno de desarrollo integrado ( WebStorm )

En [WebStorm](https://www.jetbrains.com/webstorm/), deberá instalar el paquete del servicio del lenguaje como una dependencia del proyecto.

1. Agregue lo siguiente a sus `dependencias de desarrollo` en el `package.json` de su proyecto.

<código-ejemplo language="json" header="package.json">
devDependencies {
  "@angular/lenguaje-servicio": "^6.0.0"
}
</código -ejemplo>

2. En la terminal de windows en el root de su proyecto, instalar las `dependencias de desarrollo` con `npm` o `yarn`:

```sh
npm install
```
*O*

```sh
yarn
```

*O*

```sh
yarn install
```

Cuando Angular ve esta dependencia de desarrollo, proporciona al servicio del lenguaje el ambiente WebStorm.
WebStorm le da coloración dentro de la plantilla y autocompleta la adición al servicio del lenguaje Angular.



### Sublime Text

En [Sublime Text](https://www.sublimetext.com/), el servicio del lenguaje solo admite plantillas en línea cuando se instalan como complemento.
Necesita un complemento personalizado Sublime (o modificaciones al complemento actual) para terminaciones en archivos HTML.

Para usar el servicio del lenguaje de plantillas en línea, primero debe agregar una extensión para permitir TypeScript, luego instala el complemento para el servicio del lenguaje Angular. Iniciar con TypeScript 2.3, TypeScript tiene un complemento que el servicio del lenguaje puede usar.

1. Instalar la última versión en el directorio local `node_modules` :

```sh
npm install --save-dev typescript
```

2. Instalar el paquete de servicio del lenguaje Angular en la misma ubicación.

```sh
npm install --save-dev @angular/language-service
```

3. Una vez el paquete se encuentra instalado, agregue lo siguiente `"compilerOptions"` en la sección `tsconfig.json` de su proyecto.

<código-ejemplo language="json" header="tsconfig.json">
  "plugins": [
      {"name": "@angular/language-service"}
  ]
</código-ejemplo>

4. En las preferencias de usuario de su editor (`Cmd+,` o `Ctrl+,`), registre lo siguiente:

<codigo-ejemplo language="json" header="Sublime Text user preferences">
"typescript-tsdk": "<path to your folder>/node_modules/typescript/lib"
</código-ejemplo>

Esto permite al servicio del lenguaje Angular obtener diagnósticos y terminaciones de archivos `.ts`.



## Cómo trabaja el servicio del lenguaje

Cuando usa un editor con un servicio del lenguaje, el editor inicia un proceso de servicios por separado y se comunica con él a través de un [RPC](https://en.wikipedia.org/wiki/Remote_procedure_call), usando el [Language Server Protocol](https://microsoft.github.io/language-server-protocol/).

Cuando escribe en el editor, el editor envía información al proceso de servicios para rastrear el estado de su proyecto.

Cuando activa una lista de finalización dentro de una plantilla, el editor analiza la plantilla en un
HTML [abstract syntax tree (AST)](https://en.wikipedia.org/wiki/Abstract_syntax_tree).
El compilador de Angular interpreta ese árbol para determinar el contexto: de qué módulo forma parte la plantilla, el alcance actual, el selector de componentes y dónde está el cursor en la plantilla AS. Puede luego determinar los símbolos que potencialmente podrían estar en esa posición.

Esto es un poco más complicado si usted está en una interpolación.
Si tiene una interpolación de `{{data.---}}` dentro de un `div` y necesita después la lista de finalización `data.---`, el compilador no puede utilizar el HTML AST para encontrar la respuesta.
El HTML solamente puede decirle al compilador que tiene algún texto con los caracteres "`{{data.---}}`".
Ahí es cuando el analizador de plantillas produce una expresión AST. El servicio del lenguaje angular mira luego a `data.---` dentro del contexto, pregunta al servicio del lenguaje TypeScript cuales miembros de `data` están, y retorna el listado de posibilidades.

<hr>

## Mas información

* Para obtener más información detallada sobre la implementación, puede consultar
[Angular Language Service API](https://github.com/angular/angular/blob/master/packages/language-service/src/types.ts).

* Para obtener más información sobre las intenciones y consideraciones de diseño, consulte [design documentation here](https://github.com/angular/vscode-ng-language-service/wiki/Design).

* También puede consultar en [Chuck Jazdzewski's presentation](https://www.youtube.com/watch?v=ez3R0Gi4z5A&t=368s) en el servicio del lenguaje de Angular de [ng-conf](https://www.ng-conf.org/) 2017.
