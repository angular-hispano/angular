# Visión general del CLI y referencia de comandos

El CLI de Angular es una herramienta de línea de comandos que se utiliza para inicializar, desarrollar, estructurar y mantener aplicaciones de Angular directamente desde la terminal.

## Instalando el CLI de Angular

Las últimas versiones del CLI de Angular pueden ser instaladas siguiendo los mismos lineamientos de la última versión de Angular respaldada, pero versiones menores pueden ser instaladas por separado.

Instalar el CLI usando el gestor de paquetes `npm`:
<code-example language="bash">
npm install -g @angular/cli
</code-example>

Para detalles acerca de los cambios entre versiones, e información acerca de los lanzamientos de las versiones anteriores, ir a la pestaña de lanzamientos en GitHub: https://github.com/angular/angular-cli/releases.

## Flujo de trabajo básico

Ejecute la herramienta en la línea de comandos a través del ejecutable `ng`. La ayuda se encuentra disponible en la línea de comandos. Ingrese lo siguiente para listar comandos u opciones para un comando dado (Ejemplo: [generate](cli/generate)) con una breve descripción.

<code-example language="bash">
ng help
ng generate --help
</code-example>

Para crear, construir y servir un nuevo proyecto de Angular básico en el servidor de desarrollo, ve al directorio padre de tu nueva área de trabajo, usa los siguientes comandos:

<code-example language="bash">
ng new my-first-project
cd my-first-project
ng serve
</code-example>

En tu navegador, abre http://localhost:4200/ para ver como esta ejecutando la nueva aplicación. Cuando usas el [ng serve](cli/serve) comando para construir una aplicación y servirla localmente, el servidor automáticamente reconstruye la aplicación y recarga la página cuando tu cambias cualquiera de los archivos base.

<div class="alert is-helpful">

Cuando tu ejecutas `ng new my-first-project` se creará una nueva carpeta, llamada `my-first-project`, en tu actual directorio de trabajo. Como tu quieres poder crear archivos dentro de un folder, asegurate que tienes los permisos necesarios en tu actual directorio de trabajo antes de ejecutar el comando.

Si tu actual directorio de trabajo no es el lugar adecuado para tu proyecto, lo puedes cambiar a un directorio más apropiado ejecutando primero `cd <path-to-other-directory>`.

</div>

## Área de trabajo y archivos del proyecto

El comando [ng new](cli/new) crea un _área de trabajo de Angular_ en el folder y genera una nueva estructura de la aplicación. Un espacio de trabajo puede tener múltiples aplicaciones y librerías. La aplicación inicial creada por el comando [ng new](cli/new) está en el nivel superior del área de trabajo. Cuando generas una aplicación adicional o librería en tu área de trabajo, esta irá dentro una subcarpeta llamada `projects/`.

Una nueva aplicación generada consiste de archivos base para un módulo raíz, con un componente raíz y una plantilla. Cada aplicación tiene una carpeta `src` que contiene la lógica, datos y recursos.

Puedes editar los archivos generados directamente, o agregarlos y modificarlos usando los comandos del CLI. Usar el comando [ng generate](cli/generate) para agregar nuevos archivos para componentes adicionales y servicios, y código para nuevos pipes, directivas, etc. Comandos como son [add](cli/add) y [generate](cli/generate), los que crean y operan en aplicaciones y librerías, deben ser ejecutadas dentro del área de trabajo o carpeta del proyecto.

- Para ver más acerca de la [estructura del archivo del área de trabajo ](guide/file-structure).

### Configuración del área de trabajo y proyecto.

Se crea un único archivo de configuración de área de trabajo, `angular.json`, en el nivel superior del área de trabajo. Aquí es donde puedes establecer los valores predeterminados por proyecto las opciones de los comandos del CLI y especificar configuraciones que se usan cuando el CLI construye un proyecto para diferentes targets.

El comando [ng config](cli/config) te permite establecer y recuperar valores de configuración desde la línea de comandos, o puedes editar directamente el archivo `angular.json`. Tenga en cuenta que los nombres opcionales en el archivo de configuración debe usar [camelCase](guide/glossary#case-types), mientras que los nombres opcionales que se suministra a los comandos pueden ser tanto camelCase o dash-case.

- Para más información [configuración de área de trabajo](guide/workspace-config).
- Vea el [schema completo](https://github.com/angular/angular-cli/wiki/angular-workspace) para `angular.json`.

## La sintaxis del lenguaje de comando del CLI

La sintaxis del comando se muestra de la siguiente manera:

`ng` _commandNameOrAlias_ _requiredArg_ [*optionalArg*] `[options]`

- La mayoría de los comandos, y algunas opciones, tienen alias. Alias se muestran en la declaración de sintaxis para cada comando.

- Nombres opcionales llevan como prefijo el doble guión (--). La opción alias llevan como prefijo llevan un guión (-). Los argumentos no llevan prefijo. Por ejemplo:

  <code-example language="bash">
  ng build my-app -c production
  </code-example>

- Típicamente, el nombre de un artefacto generado puede ser dado como un argumento para el comando o ser especificado con --nombre opcional.

- Argumento y nombres opcionales pueden ser dados en cualquiera [camelCase o dash-case](guide/glossary#case-types). `--myOptionName` es equivalente a `--my-option-name`.

### Booleanos y opciones enumeradas

Opciones booleanos tienen dos formas: `--thisOption` establece la bandera, `--noThisOption` la borra.
Si no se proporciona ninguna de las opciones, la bandera permanece en su estado predeterminado, como se indica en la documentación de referencia.

Los valores permitidos son dados con cada descripción de opción enumerada, con el valor predeterminado en **negrita**.

### Rutas relativas

Las opciones que especifican archivos pueden ser dadas por rutas absolutas, o rutas relativas al directorio actual de trabajo, que generalmente es el espacio de trabajo o proyecto raíz.

### Esquemas

Los comandos [ng generate](cli/generate) y [ng add](cli/add) toman como argumento el artefacto o la libreria para ser generado o agregado al proyecto actual. Además de las opciones generales, cada artefacto o libreria define sus propias opciones en un _esquema_. Las opciones de esquema se proporcionan al comando en el mismo formato que las opciones de comando inmediatas.
