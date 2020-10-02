{@a top}

# Establezca el título del documento

Su aplicación debería poder hacer que el título de la barra del navegador diga lo que quiera que diga.
Esta guía explica cómo hacerlo.

## El problema con el *&lt;título&gt;*

La manera mas obvia es enlazar una propiedad del componente al HTML `<title>` como este:

<code-example format=''>
  &lt;title&gt;{{Esto_No_Funciona}}&lt;/title&gt;
</code-example>

Lamentablemente eso no funcionará. El componente raíz de la aplicación es un elemento contenido en la etiqueta `<body>`. El `<title>` HTML está en el `<head>` del documento, fuera del `<body>`, lo que lo hace inaccesible para el enlace de datos de Angular.

Puede tomar el objeto `document` del navegador y establecer el título manualmente.
Eso no es ideal y socava sus posibilidades de ejecutar la aplicación fuera de un navegador algún día.

<div class="alert is-helpful">

  Ejecutar su aplicación fuera de un navegador significa que puede aprovechar las ventajas del rendizaje previo del servidor
  para tiempos de rendizajes casi instantáneos de la primera aplicación y para SEO. Significa que puede correr la aplicación
  dentro de un Web Worker para mejorar la capacidad de respuesta de su aplicación mediante el uso de varios subprocesos. Y también
  significa que puede ejecutar su aplicación dentro de Electron.js o Windows Universal para enviarla al escritorio.

</div>

## Utilice el servicio `Title`

Afortunadamente, Angular reduce las diferencias al proporcionar un servicio `Title` como parte de la *plataforma del navegador*.
El servicio [Title](api/platform-browser/Title) es una clase simple que proporciona un API
para obtener y configurar el título del documento HTML actual:

* `getTitle() : string`&mdash;Obtiene el título del documento HTML actual.
* `setTitle( newTitle : string )`&mdash;Establece el título del documento HTML actual.

Puede inyectar el servicio `Title` en la raíz de `AppComponent` y exponer un método `setTitle` enlazable que lo llame:

<code-example path="set-document-title/src/app/app.component.ts" region="class" header="src/app/app.component.ts (class)"></code-example>

¡Elace ese método a tres etiquetas de anclaje y listo!

<div class="lightbox">
  <img src="generated/images/guide/set-document-title/set-title-anim.gif" alt="Set title">
</div>

Aquí está la solución completa:

<code-tabs>
  <code-pane header="src/main.ts" path="set-document-title/src/main.ts"></code-pane>
  <code-pane header="src/app/app.module.ts" path="set-document-title/src/app/app.module.ts"></code-pane>
  <code-pane header="src/app/app.component.ts" path="set-document-title/src/app/app.component.ts"></code-pane>
</code-tabs>

## Por qué proporcionar el servicio `Título` en `bootstrap` (el arranque)

Por lo general, desea proporcionar servicios para toda la aplicación en el componente de la aplicación raíz, `AppComponent`.

Esta guía recomienda registrar el servicio de títulos durante el arranque (boostrapping),
una ubicación que se reserva para configurar el ambiente de ejecución de Angular.

Eso es exactamente lo que está haciendo.
El servicio `Title` es parte de la *plataforma del navegador* de Angular.
Si inicia su aplicación en una plataforma diferente,
tendrá que proporcionar un servicio de `Title` diferente que comprenda
el concepto de un "título de documento" para esa plataforma específica.
Idealmente, la aplicación en sí no conoce ni se preocupa por el ambiente de ejecución.
