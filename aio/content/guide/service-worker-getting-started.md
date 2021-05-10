# Comenzando con los Trabajadores de Servicio


Este documento explica como habilitar el soporte de Trabajadores de Servicio Angular en proyectos que creaste con [Angular CLI](cli). Luego usa un ejemplo simple para mostrarle un trabajador de servicio en acción demostrando la carga y almacenamiento en caché básico.

#### Prerequisitos

Una comprensión básica de la información en [Introduction to Angular service workers](guide/service-worker-intro).


## Agregando un trabajador de servicio a tu proyecto

Para configurar un trabajador de servicio Angular en su proyecto, use el comando CLI `ng add @angular/pwa`. Se encarga de configurar su aplicación para usar trabajador de servicio agregando el paquete `service-worker` junto con la preparación de los archivos de soporte necesarios.

```sh
ng add @angular/pwa --project *project-name*
```

El anterior comando completa las siguientes accciones:

1. Agrega el paquete `@angular/service-worker` en su proyecto.
2. Habilita el soporte de trabajador de servicio en la interfaz de línea de comandos.
3. importa y registra el trabajador de servicio en el módulo de la aplicación.
4. Actualiza el archivo `index.html`:
    * Incluye un enlace para agregar el archivo `manifest.webmanifest`.
    * Agrega metaetiquetas para `theme-color`.
5. Instala archivos de iconos para soportar la aplición web progresiva (PWA) instalada.
6. Crea el archivo de configuración de trabajador de servicio llamado [`ngsw-config.json`](/guide/service-worker-config), el cual especifica los comportamientos de almacenamiento en caché y otras configuraciones.


Ahora, crear el proyecto:

```sh
ng build --prod
```

El proyecto CLI está ahora preparado para usar trabajador de servicio de Angular.

## Trabajador de servicio en acción: un recorrido

Esta sección demuestra un trabajador de servicio en acción, usando una aplicación ejemplo.

### Sirviendo con `http-server`

Debido a que `ng serve` no funciona con trabajador de servicios, debe usar un servidor HTTP separado para probar su proyecto localmente. Puede usar cualquier servidor HTTP. El siguiente ejemplo usa el paquete[http-server](https://www.npmjs.com/package/http-server) de npm. Para reducir la posibilidad de conflictos y evitar servir contenido obsoleto, pruebe en un puerto dedicado y deshabilite el almacenamiento en caché.

Para servir el directorio que contiene sus archivos web con `http-server`, ejecute el siguiente comando:

```sh
http-server -p 8080 -c-1 dist/<project-name>
```

### Carga inicial

Con el servidor en ejecución, puede apuntar su navegador a http://localhost:8080/. Su aplicación debería cargarse normalmente.

**Consejo** Cuando pruebe trabajador de servicios de Angular, es una buena idea usar una ventana privada o de incognito en su navegador para asegurarse de que el trabajador de servicio no termine leyendo de un estado sobrante anterior, el cual puede causar un comportamiento inesperado.

<div class="alert is-helpful">

**Nota:*
Si no está utilizando HTTPS, el trabajador de servicio solo se registrará cuando acceda a la aplicación en `localhost`.

</div>

### Simulando un problema de red

Para simular un problema de red, deshabilite la interacción de red para su aplicación: En Chrome:

1. Seleccione **Herramientas** > **Herramientas de desarrollador** (desde el menu de Chrome ubicado en la esquina superior derecha).
2. Vaya a **pestaña red**.
3. Compruebe que **caja esté desactivada**

<div class="lightbox">
    <img src="generated/images/guide/service-worker/offline-checkbox.png" alt="The offline checkbox in the Network tab is checked">
</div>

Ahora la aplicación no tiene acceso a la interacción de red.

Para aplicaciones que no usen trabajador de servicio de Angular, la actualización ahora mostraría la página de desconectado de Chrome que dice "No hay conexión a internet".

Con la incorporación de un trabajador de servicio Angular, el comportamiento de la aplicación cambia. En una actualización, la página carga normarmente.

Si observa la pestaña de red, puede verificar que trabajador de servicio esté activo.

<div class="lightbox">
  <img src="generated/images/guide/service-worker/sw-active.png" alt="Requests are marked as from ServiceWorker">
</div>

Note que debajo de la columna "Tamaño", el estado de las solicitudes es `(from ServiceWorker)`. Esto significa que los recursos no estan siendo cargados desde la red. En su lugar estan siendo cargados desde la memoria caché del service worker.


### Qué esta siendo almacenado en caché?

Note que todos los archivos que el navegador necesita para procesar esta aplicación son almacenados en caché. La configuración estándar `ngsw-config.json` está preparada para almacenar los recursos específicos usados por el CLI:

* `index.html`.
* `favicon.ico`.
* Construir artefactos (paquetes JS y CSS).
* Cualquier cosa bajo `assets`.
* Imagenes y fuentes directamente bajo el `outputPath` configurado (por defecto `./dist/<project-name>/`) o `resourcesOutputPath`. Mire [`ng build`](cli/build) para más información acerca de estas opciones.


<div class="alert is-helpful">
Preste atención a dos puntos clave:

1. El `ngsw-config.json` generado incluye una lista limitada de fuentes almacenables en caché y extensiones de imágenes. En algunos casos podrías querer modificar el patrón global para adaptarlo a sus necesidades.

2. Si las rutas `resourcesOutputPath` o `assets` son modificadas después de la generación del archivo de configuración, necesita cambiar las rutas manualmente en `ngsw-config.json`.
</div>

### Haciendo cambios en su aplicación

Ahora que ha visto como trabajadores de servicio alamcenan su aplicación, el próximo paso es entender como funcionan las actualizaciones.

1. Si esta probando en una ventana de incónito, abra una segunda pestaña en blanco. Esto mantendrá vivo el incógnito y el estado de cache durante la prueba.

2. Cierre la pestaña de la aplicación, pero no la ventana. Este también debería  cerrar las herramientas de desarrollador.

3. Apague `http-server`.

4. A continuación, realice un cambio en la aplicación, y observe como el trabajador de servicio instala la actualización.

5. Abra `src/app/app.component.html` para editarlo.

6. Cambie el texto `Welcome to {{title}}!` a `Bienvenue à {{title}}!`.

7. Cree y ejecute el servidor nuevamente.

```sh
ng build --prod
http-server -p 8080 -c-1 dist/<project-name>
```
### Actualizando su aplicación en el navegador

Ahora observe como el navegador y el trabajador de servicio manejan la aplicación actulizada.

1. Abra http://localhost:8080 nuevamente en la misma ventana. Qué sucede?

<div class="lightbox">
  <img src="generated/images/guide/service-worker/welcome-msg-en.png" alt="It still says Welcome to Service Workers!">
</div>

¿Qué salio mal? Nada en realidad. El trabajador de servicio de Angular está haciendo este trabajo y entregando la versión de la aplicación que tiene **instalada** aunque hay una actualización disponible. En aras de la velocidad, el trabajador de servicio no espera para verificar actualizaciones antes de servir la aplicación que tiene almacenado en caché.

Si observa los registros de `http-server`, puede ver el trabajador de servicio solicitando `/ngsw.json`. Así es como el trabajador de servicio busca actualizaciones.

2. Recargue la página.

<div class="lightbox">
  <img src="generated/images/guide/service-worker/welcome-msg-fr.png" alt="The text has changed to say Bienvenue à app!">
</div>

El trabajador de servicio instaló la versión actualizada de su aplicación *en segundo plano*, y la próxima vez que la página es carguada o recargada, el trabajador de servicio  cambia a la última versión.

<hr />

## Más sobre trabajadores de servicio de Angular

También le puede interesar lo siguiente:
* [Comunicarse con los trabajadores de servicio](guide/service-worker-communications).