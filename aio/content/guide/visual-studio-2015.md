# Usando Angular con Visual Studio 2015

{@a top}

Algunos desarrolladores prefieren Visual Studio como su Entorno de Desarrollo Integrado (EDI).

Este tutorial describe los pasos necesarios para el uso y configuración de los archivos de la app de Angular en Visual Studio 2015 para un proyecto con ASP.NET 4.x.


<div class="alert is-helpful">

No hay un *ejemplo en tiempo real* para este tutorial porque se describe Visual Studio, y no en sí la propia aplicación Angular. Visual Studio utiliza la aplicación inicial de Angular creada por el comando CLI [`ng new`](cli/new) como ejemplo.

</div>


{@a asp-net-4}

## Proyectos con ASP.NET 4.x

Para configurar los archivos de inicio para un **Proyecto con ASP.NET 4.x** en
Visual Studio 2015, siga estos pasos:

<div class="alert is-helpful">

Si prefiere la experiencia de usar `Archivo | Nuevo Proyecto` y el **Núcleo ASP.NET**, 
entonces considere usar la plantilla _experimental_
<a href="http://blog.stevensanderson.com/2016/10/04/angular2-template-for-visual-studio/">Núcleo ASP.NET +  Angular para Visual Studio 2015</a>.
Tenga en cuenta que el código resultante no se asigna a los documentos. Ajústelos en consecuencia.

</div>




<h2 id='prereq1'>
  Pre-requisito: Node.js
</h2>



Instale **[Node.js® y npm](https://nodejs.org/en/download/)**, si no están ya en su máquina.
Vea [Instalación Local del Entorno](guide/setup-local "Setting up for Local Development") para versiones soportadas e instrucciones.






<h2 id='prereq2'>
  Pre-requisito: Visual Studio 2015 Update 3
</h2>

El requisito mínimo para desarrollar aplicaciones Angular con Visual Studio es el Update 3.
Las versiones anteriores no siguen las mejores prácticas para desarrollar aplicaciones con TypeScript.
Para ver su versión de Visual Studio 2015, vaya a `Ayuda | Acerca de Visual Studio`.

Si no lo tiene, instale **[Visual Studio 2015 Update 3](https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs)**.
O de click en `Herramientas | Extensiones y Actualizaciones` para actualizar a Update 3 directamente desde Visual Studio 2015.


<h2 id='prereq3'>
  Pre-requisito: Configure Herramientas Web Externas
</h2>

Configure Visual Studio para usar las herramientas web externas globales en lugar de las herramientas que se incluyen con Visual Studio:

  * Abra la caja de diálogo **Opciones** desde `Herramientas` | `Opciones`.
  * En el árbol de la izquierda, seleccione `Proyectos y soluciones` | `Herramientas web externas`.
  * A la derecha, mueva la entrada `$(PATH)` a por encima de las entradas `$(DevEnvDir`). Esto le dirá a Visual Studio que
    use las herramientas externas (como npm) que se encuentran en la ruta global antes de usar su propia versión de las herramientas externas.
  * Haga clic en Aceptar para cerrar el cuadro de diálogo.
  * Reinicie Visual Studio para que este cambio surta efecto.

Visual Studio ahora buscará primero las herramientas externas en el espacio de trabajo actual, 
si no los encuentra, buscará en la ruta global. Si Visual Studio no puede
encontrarlos en ninguna de ambas ubicaciones, utilizará sus propias versiones de las herramientas.

<h2 id='prereq4'>
  Pre-requisito: Instale Typescript para Visual Studio 2015
</h2>

Si bien Visual Studio Update 3 viene con soporte para TypeScript listo para usar, no cuenta con versiones recientes de TypeScript necesarias para desarrollar aplicaciones Angular.

Para instalar la última versión de TypeScript:

 * Descargue e instale la última versión de [TypeScript para Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48593),

  * O instalelo con npm: `npm install -g typescript@latest`.

Puede obtener mayor información sobre TypeScript con Visual Studio **[Aquí](https://blogs.msdn.microsoft.com/typescript/announcing-typescript-3-1/)**.

At this point, Visual Studio is ready. It’s a good idea to close Visual Studio and 
restart it to make sure everything is clean.

En este punto, Visual Studio estará listo. Es una buena idea cerrar Visual Studio y
reiníciarlo para asegurarse de que todo este correcto.

<h2 id='download'>
  Paso 1: Crear una aplicación desde cero con Angular
</h2>


Siga las instrucciones [Instalación de Entorno Local](guide/setup-local "Configuración de desarrollo local") para crear desde inicio una aplicación con Angular usando el comando CLI [`ng new`](cli/new).




<h2 id='create-project'>
  Paso 2: Crear un proyecto ASP.NET en Visual Studio
</h2>

Crear un proyecto con ASP.NET 4.x de la forma usual realizando las siguientes acciones:

* En Visual Studio, seleccionar desde el menu `Archivo` | `Nuevo` | `Proyecto`.
* En el árbol de la plantilla, seleccione `Plantilla` | `Visual C#` (ó `Visual Basic`) | `Web`.
* Seleccione la plantilla `Aplicación Web ASP.NET`, de un nombre al proyecto, y dé click en Aceptar.
* Seleccione la plantilla ASP.NET 4.5.2 deseada y dé click en Aceptar.


<div class="alert is-helpful">

Este tutorial utiliza la plantilla `Vacío` sin carpetas agregadas,
sin autenticación y sin alojamiento. Elija la plantilla y las opciones adecuadas para su proyecto.

</div>




<h2 id='copy'>
  Paso 3: Copie los archivos del proyecto de Angular dentro de la carpeta ASP.NET
</h2>

Copie los archivos de inicio de la aplicación Angular en la carpeta que contiene el archivo `.csproj`.
Incluya los archivos en el proyecto de Visual Studio de la siguiente manera:

* De click en el botón `Mostrar todos los archivos` en el Explorador de Soluciones para revelar todos los archivos ocultos del proyecto.
* De click derecho en cada carpeta/archivo a incluir en el proyecto y seleccione `Incluir en el Proyecto`.

  Minimamente incluya los siguientes carpetas/archivos:

  * Carpeta src/app (Responda *No*,  Si se le pide buscar tipificaciones de TypeScript)
  * src/styles.css
  * src/index.html
  * package.json
  * src/tsconfig.json



<h2 id='restore'>
  Paso 4: Restaure los paquetes necesarios
</h2>

Restaure los paquetes necesarios para una aplicación Angular de la siguiente manera:

* Haga clic derecho en el archivo `package.json` en el Explorador de Soluciones y seleccione` Restaurar Paquetes`.
  <br> Este proceso usa `npm` para instalar todos los paquetes definidos en el archivo` package.json`.
  Puede tardar algún tiempo.
* Si lo desea, abra la consola (`Ver` |` Salida`) para ver cómo se ejecutan los comandos npm.
* Ignore las advertencias.
* Cuando finalize la restauración, aparecerá un mensaje en la barra de mensajes inferior de Visual Studio
  Podría decir: `Instalación de paquetes completa`. Sea paciente. Podría tomar tiempo.
* Haga clic en el ícono `Actualizar` en el Explorador de soluciones.
* **No** incluir la carpeta `node_modules` en el proyecto. Dejarla como una carpeta oculta del proyecto .


<h2 id='build-and-run'>
  Paso 5: Cree y ejecute la aplicación
</h2>

Primero, asegúrese de que `src / index.html` esté configurado como la página de inicio.
Haga clic derecho en `index.html` en el Explorador de Soluciones y seleccione la opción` Establecer como página de inicio`.

### Para ejecutar en VS con F5

A la mayoría de los desarrolladores que usan Visual Studio les gusta presionar la tecla F5 y ver aparecer el servidor IIS.
Para utilizar el servidor IIS con la aplicación de inicio debe realizar los siguientes tres cambios.

1. En `index.html`, cambie el href base de `<base href = "/">` a `<base href = "/src/">`.
2. Igualmente en `index.html`, cambie el script para usar `/node_modules` con la barra diagonal
en lugar de `node_modules` sin la barra.
3. En `src/systemjs.config.js`, cerca del inicio del archivo,
cambie la `ruta` de npm a `/node_modules/` con una barra diagonal.

<div class="alert is-important">

Después de estos cambios, `npm start` no funcionará.
Deberá elegir configurar _ya sea_ con F5 con IIS _o_ `npm start` con el lite-server.


</div>

### Para aplicaciones que usan enrutamiento

Si su aplicación utiliza enrutamiento, debe enseñarle al servidor a que siempre regrese
`index.html` cuando el usuario solicita una página HTML
por los motivos que se explican en la guía de [Despliegue](guide/deployment#fallback).

Todo parece estar bien mientras te mueves _dentro_ de la aplicación.
Pero verá el problema de inmediato si actualiza el navegador
o pegue un enlace a la página de una aplicación (llamado "enlace profundo") en la barra de direcciones del navegador.

Lo más probable es que obtenga una respuesta *404 - Página no encontrada* del servidor
para cualquier dirección que no sea `/` ó `/index.html`.

Tendrá que configurar el servidor para que devuelva `index.html` para las solicitudes a estas páginas "desconocidas".
El servidor de desarrollo `lite-server` lo resulve desde su inicio.
Si ha cambiado a F5 e IIS, deberá configurar IIS para hacerlo.
Esta sección describe los pasos para adaptar la aplicación de inicio.

#### Configurar reglas de reescritura de IIS

Visual Studio viene con IIS Express, el cuál tiene un módulo de reescritura integrado.
Sin embargo, si está utilizando IIS normal, tendrá que instalar el módulo de reescritura.

Indique a Visual Studio cómo manejar las rutas para las solicitudes de páginas de la aplicación  agregando estas reglas de reescritura, cerca del final del archivo `web.config`:

<code-example format='.'>
  &lt;system.webServer&gt;
    &lt;rewrite&gt;
      &lt;rules&gt;
        &lt;rule name="Angular Routes" stopProcessing="true"&gt;
          &lt;match url=".*" /&gt;
          &lt;conditions logicalGrouping="MatchAll"&gt;
            &lt;add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" /&gt;
            &lt;add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" /&gt;
          &lt;/conditions&gt;
          &lt;action type="Rewrite" url="/src/" /&gt;
        &lt;/rule&gt;
      &lt;/rules&gt;
    &lt;/rewrite&gt;
  &lt;/system.webServer&gt;

</code-example>



<div class="alert is-helpful">

La coincidencia Url, `<match url=".*" />`, Reescribirá toda solicitud. Tendrá que ajustar esto si
desea que pasen algunas solicitudes, como las solicitudes de API web.

La URL en `<action type = "Rewrite" url = "/src/" />` deberá
coincidir con el href base en `index.html`.


</div>



Cree e inicie la aplicación con el depurador haciendo clic en el botón **Ejecutar** o presionando `F5`.


<div class="alert is-helpful">



Se ejecutará más rápido sin el depurador presionando `Ctrl-F5`.

</div>



El navegador predeterminado se abrirá y mostrará la aplicación de inicio de ejemplo.

Intente editar cualquiera de los archivos del proyecto. Guarde y actualice el navegador para
ver los cambios.