# Pruébalo: Despliegue


Para desplegar tu aplicación, tienes que compilarla, y después alojar el JavaScript, CSS, y HTML en un servidor web. Las aplicaciones construidas en Angular son muy portables y pueden vivir en cualquier entorno o ser servidas por cualquier tecnología, como Node, Java, .NET, PHP, y muchos otros.

<div class="alert is-helpful">

Si llegaste aquí directamente desde la [Parte 1](start "Una aplicación básica"), o completaste toda la aplicación de la tienda en línea a través de las secciones [Navegación en la aplicación](start/start-routing "Pruébalo: Navegación en la aplicación"), [Gestión de datos](start/start-data "Pruébalo: Gestión de datos"), y [Formularios para la entrada del usuario](start/start-forms "Pruébalo: Formularios para la entrada del usuario"), tienes una aplicación que puedes desplegar siguiendo las instrucciones en esta sección.

</div>

## Comparte tu aplicación

Los proyectos en StackBlitz son públicos por defecto, permitiéndote compartir tu aplicación Angular vía URL del proyecto. Ten en cuenta que esto es una gran forma de compartir ideas y prototipos, pero no esta destinado para alojamiento en producción.

1. En tu proyecto StackBlitz, asegúrate de haber hecho fork o guardado tu proyecto.
1. En la página de vista previa, deberías ver una URL parecida a `https://<Project ID>.stackblitz.io`.
1. Comparte esta URL con amigos o colegas.
1. Los usuarios que visiten tu URL verán que inicia un servidor de desarrollo y después se cargara tu aplicación.

## Creando localmente

Para crear tu aplicación localmente o para producción, descarga el código fuente desde tu proyecto StackBlitz haciendo click en el icono `Download Project` en el menú de la izquierda frente a `Project` para descargar tus archivos.

Una vez que descargues y descomprimas el código fuente, instala `Node.js` y sirve tu aplicación con el CLI de Angular.

Desde la terminal, instala globalmente el CLI de Angular con:

```sh
npm install -g @angular/cli
```

Esto instalará el comando `ng` en tu sistema, es el comando que usa para crear nuevos espacios de trabajo, nuevos proyectos, servir su aplicación durante el desarrollo o producir compilaciones para compartir o distribuir.

Crea un nuevo espacio de trabajo en el CLI de Angular usando el comando: [`ng new`](cli/new "CLI referencia de comando ng new ")

```sh
ng new my-project-name
```
En tu nueva aplicación generada por CLI, reemplaza la carpeta `/src` con la descargada de tu `StackBlitz` luego realiza una compilación.

```sh
ng build --prod
```

Esto producirá los archivos que necesitas para el despliegue.

<div class="alert is-helpful">

Si el comando `ng build` anterior arroja un error sobre paquetes faltantes, agrega las dependencias faltantes en el archivo` package.json` de tu proyecto local para que coincida con el del proyecto StackBlitz descargado.

</div>

#### Alojando el proyecto creado

Los archivos en la carpeta `dist/my-project-name` son estáticos, Esto quiere decir que tu puedes alojarlos en otro servidor web capaz de servir archivos (como `Node.js`, Java, .NET) o cualquier otro backend (como Firebase, Google Cloud, o App Engine).)

### Alojando una aplicación Angular en firebase

Una de las formas más sencillas de hacer que su sitio esté activo es alojarlo con Firebase.

1. Registra una cuenta Firebase en [Firebase](https://firebase.google.com/ "sitio web Firebase").
1. Crea un nuevo proyecto, proporcionando el nombre que quieras.
1. Agrega los esquemas `@angular/fire` que manejarán tu despliegue usando `ng add @angular/fire`.
1. Conecta tu CLI a tu cuenta Firebase e inicializa la conexión de tu proyecto usando `firebase login` y `firebase init`.
1. Sigue las instrucciones para seleccionar el proyecto `Firebase` que estas creando para alojar.
    - Selecciona la opción `Hosting` en el primer prompt.
    - Selecciona el proyecto que previamente creaste en Firebase.
    - Selecciona `dist/my-project-name` como directorio público.
1. Despliega tu aplicación con `ng deploy`.
1. Una vez desplegado, visita https://your-firebase-project-name.web.app para verlo activo!

### Alojando una aplicación angular en otro lado

Para alojar una aplicación Angular en otro alojamiento web, carga o envía los archivos al servidor.
Debido a que estás creando una aplicación de una sola página, también deberás asegurarte de redirigir cualquier URL no válida a tu archivo `index.html`.
Obtén más información sobre el desarrollo y la distribución de tu aplicación en las guías [Creando & Sirviendo](guide/build "Creando y Sirviendo aplicaciones Angular") y [Despliegue](guide/deployment "Guia de despliegue").

## Únete a la comunidad Angular

¡Ahora eres un desarrollador Angular! [Comparte este momento](https://twitter.com/intent/tweet?url=https://angular.io/start&text=I%20just%20finished%20the%20Angular%20Getting%20Started%20Tutorial "Angular en Twitter"), cuéntanos que te pareció este ejercicio de introducción, o envía [sugerencias para ediciones futuras](https://github.com/angular/angular/issues/new/choose "GitHub de Angular formulario de nuevo issue").

Angular ofrece muchas más capacidades, y ahora tienes las bases que te permiten crear una aplicación y explorar esas otras capacidades:

* Angular proporciona capacidades avanzadas para aplicaciones móviles, animación, internacionalización, renderizado del lado del servidor y más.
* [Angular Material](https://material.angular.io/ "Sitio web de Angular Material") ofrece una extensa biblioteca de componentes de Material Design.
* [Angular Protractor](https://protractor.angular.io/ "Sitio web de Angular Protractor") ofrece un marco de prueba de extremo a extremo para aplicaciones de Angular.
* Angular también tiene una extensa [red de herramientas y librerías de terceros](resources "Lista de recursos Angular").

Mantente actualizado siguiendo el [blog de Angular](https://blog.angular.io/ "Blog de Angular").
