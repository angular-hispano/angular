# Proyecto de documentación Angular (https://angular.io)

Todo en esta carpeta es parte del proyecto de documentación. Esto incluye:

* El sitio web para mostrar la documentación
* La configuración de dgeni para convertir los archivos de origen a archivos renderizados que se pueden vizualizar en el sitio web.
* Las herramientas para establecer ejemplos para el desarrollo; y generar archivos en tiempo real y archivos zip desde los ejemplos.

## Tareas de desarrollador

Nosotros usamos [Yarn](https://yarnpkg.com) para gestionar las dependencias y crear tareas de compilación.
Debes ejecutar todas estas tareas desde la carpeta `angular/aio`.
Aquí están las tareas más importantes que podrías necesitar a usar:

* `yarn` - instalar todas las dependencias.
* `yarn setup` - instalar todas las dependencias, boilerplate, stackblitz, zips y ejecuta dgeni en los documentos.
* `yarn setup-local` - igual que `setup`, pero crea los paquetes de Angular a partir del código y usa estas versiones construidas localmente (en lugar de las recuperadas desde npm) para aio y ejemplos de documentos boilerplate.

* `yarn build` - crear una compilación de producción de la aplicación (después de instalar dependencias, boilerplate, etc).
* `yarn build-local` - igual que `build`, pero usa `setup-local` en lugar de `setup`.
* `yarn build-local-with-viewengine` - igual que `build-local`, pero además también enciende el modo `ViewEngine` (pre-Ivy) en aio.
                                       (Nota: Encender el modo `ViewEngine` en ejemplos de documentos, ver `yarn boilerplate:add:viewengine` abajo.)

* `yarn start` - ejecutar un servidor web de desarrollo que observa los archivos; luego crea el doc-viewer y vuelve a cargar la página, según sea necesario.
* `yarn serve-and-sync` - ejecutar ambos, el `docs-watch` y `start` en la misma consola.
* `yarn lint` - comprobar que el código del doc-viewer sigue nuestras reglas de estilo.
* `yarn test` - observar todos los archivos de origen, para el doc-viewer, y ejecutar todas las pruebas unitarias cuando haya algún cambio.
* `yarn test --watch=false` -ejecutar todas las pruebas unitarias una sola vez.
* `yarn e2e` - ejecutar todas las pruebas de e2e para el doc-viewer.

* `yarn docs` - generar todos los documentos desde los archivos de origen.
* `yarn docs-watch` - observar el código de Angular y los archivos desde documentos y ejecutar un doc-gen en short-circuited para los documentos que fueron cambiados.
* `yarn docs-lint` - comprobar que el código del documento generado sigue nuestras reglas de estilo.
* `yarn docs-test` - ejecutar las pruebas unitarias para el código de generación de doc.

* `yarn boilerplate:add` - generar todo el código boilerplate para los ejemplos, para que puedan ejecutarse localmente.
* `yarn boilerplate:add:viewengine` - igual que `boilerplate:add` pero también enciende el modo `ViewEngine` (pre-Ivy).

* `yarn boilerplate:remove` - eliminar todo el código boilerplate que fue añadido a través`yarn boilerplate:add`.
* `yarn generate-stackblitz` - generar los archivos stackblitz que están usados por la etiqueta `live-example` en documentos.
* `yarn generate-zips` - generar los archivos zip desde los ejemplos. Zip esta disponible a través de la etiqueta `live-example` en los documentos.

* `yarn example-e2e` - ejecutar todas las pruebas e2e para ejemplos. Opciones disponibles:
  - `--setup`: generar boilerplate, forzar la actualización del controlador web y otra configuración, luego ejecutar prueba.
  - `--local`: ejecutar pruebas e2e con la versión local de Angular contenida en la carpeta "dist".
               _Requiere `--setup` para que surta efecto._
  - `--viewengine`: ejecutar pruebas e2e en modo `ViewEngine` (pre-Ivy).
  - `--filter=foo`: limitar pruebas e2e a las que contienen la palabra "foo".

> **Nota para usuarios Windows**
>
> Configurar los ejemplos implica crear algunos [enlaces simbólicos](https://en.wikipedia.org/wiki/Symbolic_link) (ver [Aquí](./tools/examples/README.md#symlinked-node_modules) para más detalles). En Windows, esto requiere tener [Habilitado el Modo de desarrollador ](https://blogs.windows.com/windowsdeveloper/2016/12/02/symlinks-windows-10) (compatible con  Windows 10 o más reciente) o ejecutar los comandos de configuración cómo administrador.
>
> Los comandos afectados son:
> - `yarn setup` / `yarn setup-*`
> - `yarn build` / `yarn build-*`
> - `yarn boilerplate:add`
> - `yarn example-e2e --setup`

## Usando ServiceWorker localmente

Ejecutando `yarn start` (incluso cuando se apunta explícitamente al modo de producción) no configura el
ServiceWorker. Si tú quieres probar el ServiceWorker localmente, tú puedes usar `yarn build` y luego
ejecutar los archivos en `dist/` con `yarn http-server dist -p 4200`.


## Guía de autoría
Existen dos tipos de contenido en la documentación:

* **API docs**: descripciones de los módulos, clases, interfaces, decoradores, etc que son parte de la plataforma Angular.
API docs estan generados directamente desde el código fuente.
El código fuente está contenido en archivos TypeScript , almacenados en la carpeta `angular/packages`.
Cada elemento de la API puede tener un comentario anterior, el cual contiene etiquetas y contenido de estilos JSDoc.
El contenido está escrito en markdown.

* **Otro contenido**: guias, tutoriales, y otro material de marketing.
Todos los demás contenidos se escriben utilizando markdown en archivos de texto, ubicados en la carpeta `angular/aio/content`.
Más específicamente, hay subcarpetas que contienen tipos particulares de contenido: guías, tutoriales y marketing.

* **Ejempos de código**: los ejemplos de código deben ser comprobables para garantizar su precisión.
Además, nuestros ejemplos tienen un aspecto específico y permiten al usuario copiar el código fuente. Para mayor
ejemplos se representan en una interfaz con pestañas (e.g. template, HTML, y TypeScript en pestañas separadas). Adicionalmente, algunos son ejemplos en acción ,que proporcionan enlaces donde se puede editar el código, ejecuta, y/o descarga. Para obtener más detalles sobre cómo trabajar con ejemplos de código, lea los [fragmentos de código](https://angular.io/guide/docs-style-guide#code-snippets), [código fuente de marcado ](https://angular.io/guide/docs-style-guide#source-code-markup), y [ ejemplos en acción ](https://angular.io/guide/docs-style-guide#live-examples) paginas de los [ autores de guías de estilo ](https://angular.io/guide/docs-style-guide).

Usamos la herramienta [dgeni](https://github.com/angular/dgeni) para convertir estos archivos en docs que se pueden ver en el doc-viewer.
Los [Autores de guías de estilo](https://angular.io/guide/docs-style-guide) prescribe pautas para
escribir páginas de guía, explica cómo usar la documentación de clases y componentes, y cómo marcar el código fuente para producir fragmentos de código.

### Generando doumentos completos

La principal tarea para generar los documentos es `yarn docs`. Esto procesará todos los archivos fuente (API y otros), extrayendo la documentación y generando archivos JSON que pueden ser consumidos por el doc-viewer.

### Generación parcial de doc para editores

La generación completa de documentos puede llevar hasta un minuto. Eso es demasiado lento para la creación y edición eficiente de documentos.

Puedes ealizar pequeños cambios en un editor inteligente que muestre un markdown con formato:
>En VS Code, _Cmd-K, V_ abre la vista previa de markdown en el panel lateral; _Cmd-B_ alterna la barra izquierda

Puedes también mirar los cambios que se muestran correctamente en el doc-viewer
con un tiempo de ciclo rápido de edición / visualización.

Para este propósito, usa la tarea `yarn docs-watch`, que observa los cambios en los archivos de origen y solo vuelve a procesar los archivos necesarios para generar los documentos relacionados con el archivo que ha cambiado. 
Dado que esta tarea tiene accesos directos, es mucho más rápido (a menudo menos de 1 segundo) pero no producirá contenido de fidelidad completa. Por ejemplo, los enlaces a otros documentoss y ejemplos de código pueden no mostrarse correctamente. Esto se nota especialmente en los enlaces a otros documentos y en los ejemplos incrustados, que no siempre se representan correctamente.

La configuración general es la siguiente:

* Abrir una terminal, estar seguro que las dependencias están instaladas; ejecutar una generación inicial del doc; luego iniciar el doc-viewer:

```bash
yarn setup
yarn start
```

* Abrir una segunda terminal y iniciar el observador de documentos

```bash
yarn docs-watch
```

>Alternativamente, prueba el comando fusionado `serve-and-sync` que crea, observa y ejecuta en la misma ventana de la terminal
```bash
yarn serve-and-sync
```

* Abrir un navegador con la siguiente dirección https://localhost:4200/ y navega hasta el documento en el que quieras trabajar.
Puedes automáticamente abrir el navegador utilizando `yarn start -o` en el primer terminal.

* Realizá cambios en el doc asociado de la página o en los archivos de ejemplo. Cada vez que un archivo es guardado, el doc
 se regenerará, la aplicación se reconstruirá y la página se volverá a cargar.

*Si recibes un error de compilación acerca de los ejemplos o cualquier otro error, asegúrese de consultar la
 [Guía de estilo de autores.](https://angular.io/guide/docs-style-guide) para más información.
