# Generando código usando esquemas

Un esquema es un generador de código basado en plantillas que soporta lógica compleja.
Es un set de instrucciones para transformar un proyecto de software, generando o modificando código.
Los esquemas están en el package [collections](guide/glossary#collection)  e instalados con npm.

La colección de esquemas puede ser una herramienta poderosa para la creación, modificación, y mantenimiento de cualquier proyecto de software, pero es particularmente útil para personalizar proyectos de Angular de acuerdo a las necesidades de su propia organización.

Podría utilizar esquemas, por ejemplo, para generar patrones UI o componentes específicos, usando templates o layouts.
Puede usarlos también para hacer cumplir las reglas y convenciones arquitectónicas, haciendo que sus proyectos sean coherentes e inter operativos.

## Esquemas para Angular CLI
 
Los esquemas son parte del ecosistema de Angular, [Angular CLI](guide/glossary#cli) usa esquemas para aplicar transformaciones a proyectos web.
 
Usted puede modificar estos esquemas, y definir nuevos para hacer cosas como actualizar su código para corregir cambios importantes en una dependencia, o para agregar una nueva opción de configuración o bien un framework a un proyecto existente.
 
Los esquemas que se incluyen en la colección `@schematics/angular` se ejecutan de forma predeterminada por los comandos `ng generate` y `ng add`.
 
El package contiene esquemas con nombre que configuran las opciones que están disponibles en el CLI para los subcomandos `ng generate`, por ejemplo `ng generate component` y `ng generate service`.
 
Los subcomandos para `ng generate` son una abreviatura para el schema correspondiente. Usted puede especificar un esquema particular (o colección de esquemas) para generar, utilizando la forma larga:
 
<code-example language="bash">
ng generate my-schematic-collection:my-schematic-name
</code-example>
 
o
 
<code-example language="bash">
ng generate my-schematic-name --collection collection-name
</code-example>
 
### Configuración de esquemas de CLI
Un schema JSON asociado con un schema le dice a Angular CLI qué opciones están disponibles para comandos y subcomandos, y determina los valores predeterminados.
 
Estos valores predeterminados pueden ser sobrescritos para proporcionar un valor diferente para una opción en la línea de comandos.
Puede ver [Workspace Configuration](guide/workspace-config) para obtener información de cómo usted puede cambiar la opción de generación predeterminada para su workspace.
 
Los schemas JSON para los escquemas predeterminados que utiliza el CLI para generar proyectos y partes de proyectos estan ubicados en el package [`@schematics/angular`](https://raw.githubusercontent.com/angular/angular-cli/v7.0.0/packages/schematics/angular/application/schema.json).
 
El schema describe las opciones disponibles para el CLI para cada subcomando de `ng generate`, como se muestra en la salida de `--help`.
 
## Desarrollo schemas para librerías.
 
Como desarrollador de librerías, usted puede crear sus propias colecciones con schemas personalizados para integrar sus librerías con Angular CLI.
 
* Un *add schematic* permite a los desarrolladores instalar su librería en un workspace de angular usando `ng add`.
 
* *Generation schematics* puede decirle a los subcomandos de  `ng generate` como modificar proyectos, configuraciones, scripts y la estructura de artefactos que están definidos en su librería.
 
* Un *update schematic* puede decirle a los subcomandos de `ng update` como modificar las dependencias de las librerías y ajustarlos a los cambios importantes cuando lanza una nueva versión.
 
Para más detalles de cómo se ven y cómo crearlos, visitar.
* [Esquemas de autoria](guide/schematics-authoring)
* [Esquemas para librerias](guide/schematics-for-libraries)
 
### Esquemas de adición
 
Un esquema de adición es típicamente suministrado por una librería, por lo que la librería puede ser agregado a un proyecto existente con `ng add`.
 
El comando `add` usa su administrador de paquetes para descargar una nueva dependencia, e invocar una script de instalación que es implementado como un schama.
 
Por ejemplo, el schema [`@angular/material`](https://material.angular.io/guide/schematics) le dice al comando `add` que instale y configure Angular Material junto con un theme, y registrar nuevos componentes que pueden ser creados con `ng generate`.
 
Puede verlo como un ejemplo y modelo para su propio schema de adición.
 
Las librerías de terceros también soportan Angular CLI con esquemas de adición.
 
Por ejemplo, `@ng-bootstrap/schematics` agrega [ng-bootstrap](https://ng-bootstrap.github.io/) para una aplicación, y `@clr/angular` instala y configura [Clarity from VMWare](https://vmware.github.io/clarity/documentation/v1.0/get-started).
 
Un esquema de adición también puede actualizar un proyecto con cambios de configuración, agregar dependencias adicionales (así como polyfills), o código de inicialización específico del paquete de scaffold.
 
Por ejemplo, el schema `@angular/pwa` convierte su aplicación en una PWA agregando un archivo manifest y un service worker, y el schema `@angular/elements`  agrega el `document-register-element.js` polyfill y dependencias para Angular Elements.
 
### Esquemas de Generación
Los esquemas de generación, son instrucciones para el comando `ng generate`.
 
Los sub comandos documentados usan esquemas de generación Angular predeterminados, pero usted puede especificar un esquema diferente (en lugar de un sub comando) para generar un artefacto definido en su librería.
 
Angular Material, por ejemplo, proporciona esquemas de generación para el componentes UI que los definen.
El siguiente comando usa uno de esos esquemas para renderizar Angular Material `<mat-table>`  que es preconfigurado con un datasource para ordenar y paginar.
 
<code-example language="bash">
ng generate @angular/material:table <component-name>
</code-example>
 
### Actualizar esquemas.
 
Los comandos `ng update` pueden ser usados para actualizar las dependencias de la librería de su workspace. Si usted no proporciona opciones o usa la opción help, el comando examina su workspace y sugiere librerías para actualizar.
 
<code-example language="bash">
ng update
 
   Analizamos su package.json, Hay algunos packages para actualizar:
     Name                               Version                  Command to update
    --------------------------------------------------------------------------------
     @angular/cdk                       7.2.2 -> 7.3.1           ng update @angular/cdk
     @angular/cli                       7.2.3 -> 7.3.0           ng update @angular/cli
     @angular/core                      7.2.2 -> 7.2.3           ng update @angular/core
     @angular/material                  7.2.2 -> 7.3.1           ng update @angular/material
     rxjs                               6.3.3 -> 6.4.0           ng update rxjs
 
   Es posible que haya paquetes adicionales que están actualizados.
   Ejecutar "ng update --all" trata de actualizar todos los packages al mismo tiempo.
</code-example>
 
Si le pasan el comando un set de librerías para actualizar (o la bandera `--all`), Este actualiza esas librerías, sus dependencias de pares, y las dependencias de pares que dependen de ellos.
 
<div class="alert is-helpful">
 
Si hay inconsistencias (por ejemplo, si las dependencias de pares no coinciden con un simple range [semver](https://semver.io/)), El comando genera un error y no cambia nada en el workspace.
 
Nosotros recomendamos que nos se force la actualización de todas la dependencias predeterminadas. Trata actualizando primero dependencias específicas.
 
Para más información acerca del como trabaja el comando `ng update`, puedes vistar [Update Command](https://github.com/angular/angular-cli/blob/master/docs/specifications/update.md).
 
</div>
 
Si usted crea una nueva versión de su librería que introduce cambios importantes, usted puedes proveer un *update schematic* para habilitar el comando `ng update` para automáticamente resolver cualquier cambio en un proyecto que se actualiza.
 
Por ejemplo, suponga que quiere actualizar la librería Angular Material.
 
<code-example language="bash">
ng update @angular/material
</code-example>
 
Este comando actualiza ambos `@angular/material` y sus dependencias `@angular/cdk`  en su workspace `package.json`.
Si alguno de los paquetes contiene un schema de actualización que cubre la migración de una versión existente hacia una nueva versión, el comando ejecuta ese esquema en su workspace.