# Generando código usando esquemas

Un esquema es un generador de código basado en plantillas que soporta lógica compleja.
Es un conjunto de instrucciones para transformar un proyecto de software, generando o modificando código.
Los esquemas están en el paquete [collections](guide/glossary#collection)  e instalados con npm.

La colección de esquemas puede ser una herramienta poderosa para la creación, modificación, y mantenimiento de cualquier proyecto de software, pero es particularmente útil para personalizar proyectos de Angular de acuerdo a las necesidades de tu propia organización.

Podrías utilizar esquemas, por ejemplo, para generar patrones UI o componentes específicos, usando templates o layouts.
Puedes usarlos también para hacer cumplir las reglas y convenciones arquitectónicas, haciendo que tus proyectos sean coherentes e inter operativos.

## Esquemas para Angular CLI

Los esquemas son parte del ecosistema de Angular, [Angular CLI](guide/glossary#cli) usa esquemas para aplicar transformaciones a proyectos web.

Tu puedes modificar estos esquemas, y definir nuevos para hacer cosas como actualizar tu código para corregir cambios importantes en una dependencia, o para agregar una nueva opción de configuración o bien un framework a un proyecto existente.

Los esquemas que se incluyen en la colección `@schematics/angular` se ejecutan de forma predeterminada por los comandos `ng generate` y `ng add`.

El paquete contiene esquemas con nombre que configuran las opciones que están disponibles en el CLI para los subcomandos `ng generate`, por ejemplo `ng generate component` y `ng generate service`.

Los subcomandos para `ng generate` son una abreviatura para el schema correspondiente. Puedes especificar un esquema particular (o colección de esquemas) para generar, utilizando la forma larga:

<code-example language="bash">
ng generate my-schematic-collection:my-schematic-name
</code-example>

o

<code-example language="bash">
ng generate my-schematic-name --collection collection-name
</code-example>

### Configuración de esquemas de CLI
Un esquema JSON asociado con un esquema le dice a Angular CLI qué opciones están disponibles para comandos y subcomandos, y determina los valores predeterminados.

Estos valores predeterminados pueden ser sobrescritos para proporcionar un valor diferente para una opción en la línea de comandos.
Puede ver [Configuración del espacio de trabajo](guide/workspace-config) para obtener información de cómo puedes cambiar la opción de generación predeterminada para tu espacion de trabajo.

Los esquemas JSON para los esquemas predeterminados que utiliza el CLI para generar proyectos y partes de proyectos están ubicados en el paquete [`@schematics/angular`](https://raw.githubusercontent.com/angular/angular-cli/v7.0.0/packages/schematics/angular/application/schema.json).

El esquema describe las opciones disponibles para el CLI para cada subcomando de `ng generate`, como se muestra en la salida de `--help`.

## Desarrollo de esquemas para librerías.

Como desarrollador de librerías, puedes crear tus propias colecciones con esquemas personalizados para integrar tus librerías con Angular CLI.

* Un *add schematic* permite a los desarrolladores instalar sus librería en un espacion de trabajo de Angular usando `ng add`.

* *Generation schematics* puede decirle a los subcomandos de  `ng generate` como modificar proyectos, configuraciones, scripts y la estructura de artefactos que están definidos en su librería.

* Un *update schematic* puede decirle a los subcomandos de `ng update` como modificar las dependencias de las librerías y ajustarlos a los cambios importantes cuando lanza una nueva versión.

Para más detalles de cómo se ven y cómo crearlos, visitar.
* [Esquemas de autoria](guide/schematics-authoring)
* [Esquemas para librerias](guide/schematics-for-libraries)

### Esquemas de adición

Un esquema de adición es típicamente suministrado por una librería, por lo que la librería puede ser agregado a un proyecto existente con `ng add`.

El comando `add` usa su administrador de paquetes para descargar una nueva dependencia, e invocar una script de instalación que es implementado como un schama.

Por ejemplo, el esquema [`@angular/material`](https://material.angular.io/guide/schematics) le dice al comando `add` que instale y configure Angular Material junto con un tema, y que registre nuevos componentes que pueden ser creados con `ng generate`.

Puedes verlo como un ejemplo y modelo para tu propio esquema de adición.

Las librerías de terceros también soportan Angular CLI con esquemas de adición.

Por ejemplo, `@ng-bootstrap/schematics` agrega [ng-bootstrap](https://ng-bootstrap.github.io/) para una aplicación, y `@clr/angular` instala y configura [Clarity from VMWare](https://vmware.github.io/clarity/documentation/v1.0/get-started).

Un esquema de adición también puede actualizar un proyecto con cambios de configuración, agregar dependencias adicionales (así como polyfills), o estructurar código de inicialización específico del paquete.

Por ejemplo, el esquema `@angular/pwa` convierte tu aplicación en una PWA agregando un archivo manifest y un service worker, y el esquema `@angular/elements`  agrega el `document-register-element.js` polyfill y dependencias para Angular Elements.

### Esquemas de Generación
Los esquemas de generación, son instrucciones para el comando `ng generate`.
Los sub comandos documentados usan esquemas de generación predeterminados de Angular, pero puedes especificar un esquema diferente (en lugar de un sub comando) para generar un artefacto definido en su librería.

Angular Material, por ejemplo, proporciona esquemas de generación para el componentes UI que los definen.
El siguiente comando usa uno de esos esquemas para renderizar Angular Material `<mat-table>` que es preconfigurado con un datasource para ordenar y paginar.

<code-example language="bash">
ng generate @angular/material:table <component-name>
</code-example>

### Actualizar esquemas.

Los comandos `ng update` pueden ser usados para actualizar las dependencias de la librería de tu espacio de trabajo. Si no proporcionas opciones o usas la opción help, el comando examina tu espácio de trabajo y sugiere librerías para actualizar.

<code-example language="bash">
ng update

   Analizamos tu package.json, hay algunos paquetes por actualizar:
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

Si le pasas el comando un conjunto de librerías para actualizar (o la bandera `--all`), este actualiza esas librerías, sus dependencias de pares, y las dependencias de pares que dependen de ellos.

<div class="alert is-helpful">

Si hay inconsistencias (por ejemplo, si las dependencias de pares no coinciden con un simple rango [semver](https://semver.io/)), el comando genera un error y no cambia nada en el espacio de trabajo.

Nosotros recomendamos que nos se force la actualización de todas la dependencias predeterminadas. Trata actualizando primero dependencias específicas.

Para más información acerca del como trabaja el comando `ng update`, puedes vistar [Update Command](https://github.com/angular/angular-cli/blob/master/docs/specifications/update.md).

</div>

Si creas una nueva versión de tu librería que introduce cambios importantes, puedes proveer un *update schematic* para habilitar el comando `ng update` para automáticamente resolver cualquier cambio en un proyecto que se actualiza.

Por ejemplo, supón que quieres actualizar la librería Angular Material.

<code-example language="bash">
ng update @angular/material
</code-example>

Este comando actualiza ambos `@angular/material` y sus dependencias `@angular/cdk`  en tu espacion de trabajo `package.json`.
Si alguno de los paquetes contiene un esquema de actualización que cubre la migración de una versión existente hacia una nueva versión, el comando ejecuta ese esquema en su espacio de trabajo.
