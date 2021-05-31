{@a top}

# Testing

Testear tu aplicación Angular te ayuda a comprobar que tu app funciona como esperabas. 

## Prerequisitos

Antes de escribir tests para tu aplicación Angular, deberías tener un conocimiento básico de los siguientes conceptos:

* Fundamentos de Angular
* JavaScript
* HTML
* CSS
* [Angular CLI](/cli)

<hr>

La documentación de testing ofrece consejos y técnicas para test unitarios y de integración para aplicaciones Angular a través de una aplicación de ejemplo
creada con [Angular CLI](cli).
Esta aplicación de ejemplo es muy parecida a la aplicación [_Tour de Heroes_ tutorial](tutorial).

<div class="alert is-helpful">

  Para la aplicación que la guía de testing describe, mira la <live-example noDownload>aplicación de ejemplo</live-example>.

  Para las características de tests en las guías de testing, mira see <live-example stackblitz="specs" noDownload>tests</live-example>.

</div>

{@a setup}

## Preparando los tests

La CLI de Angular descarga e instala todo lo que necesitas para testear una aplicación Angular con el [Jasmine test framework](https://jasmine.github.io/).

El proyecto que creas con el CLI esta inmediatamente preparado para testing.
Solo ejecuta el comando CLI [`ng test`](cli/test):

<code-example language="sh" class="code-shell">
  ng test
</code-example>

El comando `ng test` compila la aplicación en _watch mode_,
y lanza el [Karma test runner](https://karma-runner.github.io).

La salida de la consola se parece un poco a esto:

<code-example language="sh" class="code-shell">
10% building modules 1/1 modules 0 active
...INFO [karma]: Karma v1.7.1 server started at http://0.0.0.0:9876/
...INFO [launcher]: Launching browser Chrome ...
...INFO [launcher]: Starting browser Chrome
...INFO [Chrome ...]: Connected on socket ...
Chrome ...: Executed 3 of 3 SUCCESS (0.135 secs / 0.205 secs)
</code-example>

La última linea del log es la más importante:
Muestra que Karma ejecutó tres test y todos pasaron correctamente.

Un navegador Chrome también se abre y muestra la salida de los tests en un "Jasmine HTML Reporter" como este.

<div class="lightbox">
  <img src='generated/images/guide/testing/initial-jasmine-html-reporter.png' alt="Jasmine HTML Reporter in the browser">
</div>

La mayoría de la gente encuentra esta salida del navegador más facil de leer que la salida del log de la consola.
Puedes hacer click en una linea de test para re-ejecutar ese test o hacer click en una descripcion para re-ejecutar los tests in el grupo de test seleccionado
("test suite").

Mientras tando, el comando `ng test` está observando si hay cambios.

Para ver esto en acción, haz un pequeño cambio al `app.component.ts` y guarda.
Los test se ejecutan otra vez, el navegador se refresca, y los nuevos resultados de los tests aparecen.

## Configuración

La CLI se encarga de la configuración de Jasmine y Karma por ti.

Puedes ajustar muchas opciones editando los archivos `karma.conf.js` y 
`test.ts` en la carpeta `src/`.

El archivo `karma.conf.js` es un archivo de configuración parcial de Karma.
La CLI construye la configuración de ejecución completa en memoria, basada en la estructura de la aplicación especificada en el archivo `angular.json`,
suplementada por el archivo `karma.conf.js`.

Busca en la web para más detalles sobre las configuraciones de Jasmine y Karma.

### Otros frameworks de testing

También puedes hacer test unitarios de Angular con otras librerías de testing y ejecutores de test. 
Cada librería y ejecutor tienen sus propios procedimientos de instalación distinctivos, configuración y sintaxis. 

Busca en la web para aprender más.

### Nombre del archivo de test y localización

Mira dentro de la carpeta `src/app`.

La CLI genera un archivo de test para `AppComponent` llamado `app.component.spec.ts`.

<div class="alert is-important">

La extensión del archivo **debe ser `.spec.ts`** para que la herramienta pueda identificarlo como un archivos con tests (AKA, un archivo _spec_).

</div>

Los archivos `app.component.ts` y `app.component.spec.ts` son hermanos en el mismo directorio.
La raiz del nombre de los archivos (`app.component`) es la misma para ambos archivos.

Adopta estas dos convenciones en tus proyectos para _todo tipo_ de archivos de test.

{@a q-spec-file-location}

#### Pon tus archivos spec junto al archivo al que testea 

Es una buena idea poner los archivos spec de tests unitarios en la misma carpeta
que el archivo del codigo fuente de la aplicación que testean.

- Estos tests son fáciles de encontrar.
- Se puede ver de un vistazo si una parte de tu aplicación carece de pruebas.
- Los tests cercanos pueden revelar como funciona una pieza en su contexto.
- Cuando es inevitable mover un archivo, recuerdas mover con él sus tests.
- Cuando es inevitable mover un archivo, recuerdas renombrar el archivo de tests.

{@a q-specs-in-test-folder}

#### Pon tus archivos spec en un directorio de test

Los archivos spec de integración de la aplicación pueden testear la interacción
de múltiples partes repartidas a lo largo de los directorios y modulos.
No pertenecen a ningun lugar en particular, así que no tienen un hogar
natural junto a ningun archivo.

A menudo es mejor crear una carpeta apropiada para ellos en el directorio `tests`. 

Por supesto, specs que testean los test helpers pertenecen al directorio `test`,
junto a su correspondiente archivo de ayuda.

{@a ci}

## Configurando la integración continua

Una de las mejores formas de mantener tu proyecto libre de bugs es a través de una test suite, pero es fácil olvidarse de ejecutar los tests todo el tiempo.
Los servidores de Integración Contínua (CI) te dejan configurar el repositorio de tu proyecto para ejecutar los tests en cada commit y pull request. 

Hay servicios CI de pago como Circle CI y Travis CI, y puedes incluso hostear tus propios gratuitos usando Jenkins y otros.
Aunque Circle CI y Travis CI son servicios de pago, se proporcionan de forma gratuita para proyectos open source.
Puedes crear un proyecto publico en GitHub y añadir estos servicios sin pagar.
Las contribuciones al repositorio de Angular son automaticamente ejecutadas a través de una suite completa de tests de Circle CI.

Este artículo explica como configurar tu proyecto para ejecutra Circle CI y Travis CI, y además actualizar tu configuración de test para ser capaz de ejecutar
tests en el navegador Chrome en cualquier entorno.


### Configuración para Circle CI

Paso 1: Crea una carpeta llamada `.circleci` en la raiz del proyecto.

Paso 2: En la nueva carpeta, crea un archivo llamado `config.yml` con el siguiente contenido:

```
version: 2
jobs:
  build:
    working_directory: ~/my-project
    docker:
      - image: circleci/node:10-browsers
    steps:
      - checkout
      - restore_cache:
          key: my-project-{{ .Branch }}-{{ checksum "package-lock.json" }}
      - run: npm install
      - save_cache:
          key: my-project-{{ .Branch }}-{{ checksum "package-lock.json" }}
          paths:
            - "node_modules"
      - run: npm run test -- --no-watch --no-progress --browsers=ChromeHeadlessCI
      - run: npm run e2e -- --protractor-config=e2e/protractor-ci.conf.js
```

Esta configuración cachea `node_modules/` y usa [`npm run`](https://docs.npmjs.com/cli/run-script) para ejecutar comandos CLI, debido a que `@angular/cli`
no está instalado globalmente.
El doble guión (`--`) es necesario para pasar argumentos al script `npm`.

Paso 3: Haz commit de tus cambios y subelos a tu repositorio.

Paso 4: [Registrate en Circle CI](https://circleci.com/docs/2.0/first-steps/) y [añadelo a tu proyecto](https://circleci.com/add-projects).
Tu proyecto debería empezar a compilarse.

* Aprende mas sobre Circle CI  en [Documentacion de Circle CI](https://circleci.com/docs/2.0/).

### Configuración para Travis CI

Paso 1: Crea un archivo llamado `.travis.yml` en la raiz del proyecto, con el siguiente contenido:

```
dist: trusty
sudo: false

language: node_js
node_js:
  - "10"

addons:
  apt:
    sources:
      - google-chrome
    packages:
      - google-chrome-stable

cache:
  directories:
     - ./node_modules

install:
  - npm install

script:
  - npm run test -- --no-watch --no-progress --browsers=ChromeHeadlessCI
  - npm run e2e -- --protractor-config=e2e/protractor-ci.conf.js
```

Esto hace lo mismo que la configuración de Circle CI, excepto que Travis no viene con Chrome, así que usamos Chromium en su lugar.

Paso 2: Haz commit de tus cambios y subelos a tu repositorio.

Paso 3: [Registrate en Travis CI](https://travis-ci.org/auth) y [añade tu proyecto](https://travis-ci.org/profile).
Necesitarás subir un nuevo commit para disparar la compilación del proyecto.

* Aprende mas sobre Travis CI en [Documentación de Travis CI](https://docs.travis-ci.com/).

### Configuración de la CLI para el testeo de CI en Chrome

Normalmente, cuando los comandos CLI `ng test` y `ng e2e` son ejecutados la CI se testea en tu entorno, quizas tendrías todavía que ajustar tu configuración para ejecutar los test en Chrome.

Existen archivos de configuracion para ambos [Karma JavaScript test runner](https://karma-runner.github.io/latest/config/configuration-file.html)
y [Protractor](https://www.protractortest.org/#/api-overview) end-to-end herramienta de testing,
que debes configurar para ejecutar Chrome sin sandboxing.

Usaremos [Headless Chrome](https://developers.google.com/web/updates/2017/04/headless-chrome#cli) en estos ejemplos.

* En el archivo de configuración de Karma, `karma.conf.js`, añade un disparador personalizado llamado ChromeHeadlessCI debajo de browsers:
```
browsers: ['Chrome'],
customLaunchers: {
  ChromeHeadlessCI: {
    base: 'ChromeHeadless',
    flags: ['--no-sandbox']
  }
},
```

* En el directorio raiz de tu proyecto de tests e2e, crea un nuevo archivo llamado `protractor-ci.conf.js`. Este nuevo archivo extiende el original `protractor.conf.js`.
```
const config = require('./protractor.conf').config;

config.capabilities = {
  browserName: 'chrome',
  chromeOptions: {
    args: ['--headless', '--no-sandbox']
  }
};

exports.config = config;
```

Ahora puedes ejecutar los siguientes comandos para usar el flag `--no-sandbox`:

<code-example language="sh" class="code-shell">
  ng test --no-watch --no-progress --browsers=ChromeHeadlessCI
  ng e2e --protractor-config=e2e/protractor-ci.conf.js
</code-example>

<div class="alert is-helpful">

   **Nota:** Justo ahora, querrás incluir el flag `--disable-gpu` si estás ejecutando en Windows. Visita [crbug.com/737678](https://crbug.com/737678).

</div>


<hr />

## Mas información para testing

Después de configurar tu aplicación para testing, quizas encuentres útiles las siguientes guias para test.

* [Cobertura de código](guide/testing-code-coverage)&mdash;averigua cuanto de tu aplicación tus tests están cubriendo y como especificar cantidades requeridas.
* [Testing de servicios](guide/testing-services)&mdash;aprende como testear los servicios que usa tu aplicación.
* [Fundamentos del testing de componentes](guide/testing-components-basics)&mdash;descubre los fundamentos de testear componentes Angular.
* [Escenarios del testing de componentes](guide/testing-components-scenarios)&mdash;lee sobre los varios tipos de escenarios de testeo de componentes y casos de uso.
* [Testing de directivas de atributo](guide/testing-attribute-directives)&mdash;aprende como testear tus directivas de atributo.
* [Testing de pipes](guide/testing-pipes)&mdash;aprende como testear tu pipes.
* [Debugeando tests](guide/testing-attribute-directives)&mdash;Descubre bugs de testing comunes.
* [APIs de utilidades de testing](guide/testing-utility-apis)&mdash;familiarízate con las funcionalidades de testing de Angular.


