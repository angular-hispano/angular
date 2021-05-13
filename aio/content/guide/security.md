# Seguridad

Esta página describe las protecciones incorporadas de Angular contra vulnerabilidades comunes de las aplicaciones web y ataques tales como ataques de secuencia de comandos en sitios cruzados (cross-site scripting attacks). No cubre seguridad en lo que respecta a la aplicación, tales como autenticación (_¿Quién es este usuario?_) y autorización (_¿Qué puede hacer este usuario?_).

Para más información sobre los ataques y cómo mitigarlos descritos en este documento, ve a [OWASP Guide Project](https://www.owasp.org/index.php/Category:OWASP_Guide_Project).

Puedes ejecutar el <live-example></live-example> en Stackblitz y descargar el código desde ahí.



<h2 id='report-issues'>
  Reportando vulnerabilidades
</h2>



Para reportar vulnerabilidades en el propio Angular, envíanos un email a [security@angular.io](mailto:security@angular.io).

Para más información sobre cómo Google maneja los problemas de seguridad, ve a [Google's security
philosophy](https://www.google.com/about/appsecurity/).



<h2 id='best-practices'>
  Mejores prácticas
</h2>



* **Mantente al corriente con los últimos lanzamientos de las librerías de Angular.**
Regularmente actualizamos las librerías de Angular, y estas actualizaciones pueden arreglar defectos de seguridad descubiertos en versiones anteriores. Puedes ver el [registro de cambios](https://github.com/angular/angular/blob/master/CHANGELOG.md) de Angular para ver las actualizaciones relacionadas con la seguridad.

* **No modifiques tu copia de Angular.**
Las versiones privadas o personalizadas de Angular tienden a quedarse atrás de la versión más actual y pueden no incluir importantes correcciones y mejoras en la seguridad. En vez de eso, comparte tus mejoras con la comunidad y haz un *pull request*.

* **Evita las APIs de Angular marcadas en la documentación como "_Riesgo de seguridad_".**
Para más información, ve a la sección [confiando en valores seguros](guide/security#bypass-security-apis) en esta página.



<h2 id='xss'>
  Prevención de secuencias de comando en sitios cruzados (Cross-Site Scripting - XSS)
</h2>



Las [Secuencias de comandos en sitios cruzados (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting) permite a los atacantes inyectar código malicioso a las páginas web. Tal código podría, por ejemplo, robar datos de los usuarios (en particular, datos de inicio de sesión) o realizar acciones para imitar al usuario. Este es uno de los ataques más comunes en la web.

Para bloquear los ataques XSS, debes prevenir que el código malicioso no entre al DOM (Document Object Model - Modelo de Objeto de Documento). Por ejemplo, si los atacantes te engañan para que insertes una etiqueta `<script>` en el DOM, pueden ejecutar código arbitrario en tu sitio web. El ataque no está limitado solo a las etiquetas `<script>`&mdash;muchos elementos y propiedades en el DOM permiten la ejecución de código, por ejemplo, `<img onerror="...">` y `<a href="javascript:...">`. Si  los datos controlados por el atacante ingresan al DOM, espera vulnerabilidades de seguridad.

### Modelo de Seguridad para Secuencia de Comandos de Sitios Cruzados de Angular

Para bloquear sistemáticamente los errores por XSS, Angular maneja todos los valores como no confiables (*untrusted*) por defecto. Cuando un valor es insertado al DOM desde una plantilla, vía propiedad, atributo, estilo, enlazado por la clase, o interpolación, Angular los desinfecta y escapa todos los valores que no son de confianza.

_Las plantillas de Angular son iguales que el código ejecutable_: Se confía en que el HTML, atributos, y expresiones vinculadas (pero no los valores ligados) en las plantillas sean seguras. Esto significa que las aplicaciones deben prevenir valores que un atacante pueda controlar para llegar al código fuente de una plantilla. Nunca generes plantillas con código fuente al concatenar las entradas del usuario y las plantillas.

Para prevenir estas vulnerabilidades, usa el [compilador de plantillas fuera de línea](guide/security#offline-template-compiler), también conocido como _inyección de plantillas_.

{@a sanitization-and-security-contexts}
### Desinfección y contextos de seguridad

_Desinfección_ es la inspección de un valor no confiado, convirtiéndolo en un valor que sea seguro insertarlo en el DOM. En muchos casos, la desinfección no cambia todos los valores por completo. La desinfección depende del contexto: un valor que es inofensivo en CSS es potencialmente peligroso en una URL.

Angular define los siguientes contextos de seguridad:
* **HTML** es usado cuando se interpreta un valor como HTML, por ejemplo, cuando se une con un `innerHtml`.
* **Style** es usado cuando se une CSS a una propiedad en el `style`.
* **URL** es usado para propiedades en la URL, tales como `<a href>`.
* **Resource URL** es una URL que será cargada y ejecutada como código, por ejemplo, en un `<script src>`.

Angular desinfecta valores que no son de confianza para el HTML, estilos y URLs; Desinfectar recursos en URLs no es posible porque contienen código arbitrario. En modo de desarrollo, Angular muestra advertencias en la consola cuando tiene que cambiar un valor durante la desinfección.

### Ejemplo de desinfección

La siguiente plantilla une el valor del `htmlSnippet`, una vez al interpolarse en el contenido de un elemento, y una vez al unirse a la propiedad `innerHTML` de un elemento:


<code-example path="security/src/app/inner-html-binding.component.html" header="src/app/inner-html-binding.component.html"></code-example>


El contenido interpolado siempre es escapado&mdash;el HTML no es interpretado y el explorador muestra paréntesis angulares en el contenido de texto del elemento.

Para que el HTML sea interpretado, únalo a una propiedad HTML tal como `innerHTML`. Pero al vincular un valor que un atacante pueda controlar en el `innerHTML` normalmente provoca una vulnerabilidad XSS. Por ejemplo, código contenido y ejecutado una etiqueta `<script>`:


<code-example path="security/src/app/inner-html-binding.component.ts" header="src/app/inner-html-binding.component.ts (class)" region="class"></code-example>


Angular reconoce el valor como no seguro y automáticamente lo desinfecta, este remueve la etiqueta `<script>` pero conserva el contenido seguro como el elemento `<b>`.


<div class="lightbox">
  <img src='generated/images/guide/security/binding-inner-html.png' alt='A screenshot showing interpolated and bound HTML values'>
</div>


### Uso directo de las API DOM y funciones de desinfección explícitas 

Las API DOM integradas del navegador no te protegen automáticamente de las vulnerabilidades de seguridad. Por ejemplo, `document`, el nodo disponible a través de `ElementRef`, y muchas APIs de terceros contienen métodos no seguros. De la misma manera, si tú interactúas con otras librerías que manipulan el DOM, probablemente no tendrás la misma desinfección automática como con las interpolaciones de Angular. Evita interactuar directamente con el DOM y en lugar de eso, usa las plantillas de Angular donde sea posible.

Para los casos donde esto sea inevitable, usa las funciones de desinfección integradas de Angular. Desinfecta valores que no sean de confianza con el método [DomSanitizer.sanitize](api/platform-browser/DomSanitizer#sanitize) y el contexto apropiado en `SecurityContext`. Esa función también acepta valores que fueron marcados como de confianza usando las funciones `bypassSecurityTrust`..., y no las desinfectará, como [se describe a continuación](#bypass-security-apis).

### Política de Seguridad de Contenido

La Política de Seguridad de Contenido (Content Security Policy - CSP) es una técnica de defensa en profundidad para prevenir ataques XSS. Para habilitar el CSP, configura tu servidor web para retornar una apropiada cabecera HTTP con `Content-Security-Policy`. Leer más sobre las políticas de seguridad de contenidos en [Una introducción a las Políticas de Seguridad de Contenidos](http://www.html5rocks.com/en/tutorials/security/content-security-policy/) en la página web de HTML5Rocks.


{@a offline-template-compiler}


### Uso del compilador de plantillas fuera de línea (offline template compiler)

El compilador de plantillas fuera de línea previene muchas clases de vulnerabilidades denominadas inyección de plantillas, y mejora enormemente el desempeño de la aplicación. Usa el compilador de plantillas fuera de línea en despliegues de producción; no generes plantillas dinámicamente. Angular confía en el código de las plantillas, así que al generar plantillas, en particular plantillas que contienen datos de usuarios, elude las protecciones integradas en Angular.

Para más información sobre cómo construir formularios dinámicamente de forma segura, vea la guía de [Formularios Dinámicos](guide/dynamic-form).

### Protección del Lado del Servidor contra ataques XSS

El HTML construido del lado del servidor es vulnerable a ataques de inyección. Inyectar código en las plantillas en una aplicación de Angular es igual a inyectar código ejecutable en la aplicación: le da control total de la aplicación al atacante. Para prevenir esto, usa un lenguaje en las plantillas que automáticamente escape valores para prevenir vulnerabilidades XSS en el servidor. No generes plantillas de Angular en el lado del servidor usando lenguaje para las plantillas. Hacer esto conlleva un alto riesgo de introducir vulnerabilidades de inyección en las plantillas.


<h2 id='bypass-security-apis'>
  Confiando en valores seguros
</h2>


Algunas aplicaciones genuinamente necesitan incluir código ejecutable, mostrar un `<iframe>` de alguna URL, o construir URLs potencialmente peligrosas. Para prevenir la desinfección automática en cualquiera de estas situaciones, puedes decirle a Angular que inspeccionaste un valor, verificaste cómo se generó, y te aseguraste que siempre será seguro. Pero *ten cuidado*. Si confías en un valor que podría ser malicioso, estás introduciendo una vulnerabilidad de seguridad en tu aplicación. Si dudas, busca un revisor de seguridad profesional.

Para marcar un valor como de confianza, inyecta el `DomSanitizer` y llama uno de los siguientes métodos:

* `bypassSecurityTrustHtml`
* `bypassSecurityTrustScript`
* `bypassSecurityTrustStyle`
* `bypassSecurityTrustUrl`
* `bypassSecurityTrustResourceUrl`

Recuerda, sí un valor es seguro depende del contexto, entonces, elige el contexto adecuado para el valor que desees usarlo. Imagina que la siguiente plantilla necesita unir una URL a una función de un `javascript:alert(...)`:

<code-example path="security/src/app/bypass-security.component.html" header="src/app/bypass-security.component.html (URL)" region="URL"></code-example>


Normalmente, Angular desinfecta automáticamente las URL, deshabilita el código peligroso, y en modo de desarrollo, registra esta acción en la consola. Para prevenir esto, marca el valor de la URL como una URL de confianza usando la función `bypassSecurityTrustUrl`:


<code-example path="security/src/app/bypass-security.component.ts" header="src/app/bypass-security.component.ts (trust-url)" region="trust-url"></code-example>



<div class="lightbox">
  <img src='generated/images/guide/security/bypass-security-component.png' alt='A screenshot showing an alert box created from a trusted URL'>
</div>


Si necesitas convertir entradas de un usuario a un valor de confianza, utiliza un método controlador. La siguiente plantilla permite a los usuarios ingresar un ID de un vídeo de YouTube y carga el video correspondiente en un `<iframe>`. El atributo `<iframe src>` es un recurso de contexto de seguridad de la URL, porque una fuente no confiable puede, por ejemplo, infiltrarse en archivos de descarga que usuarios desprevenidos podrían ejecutar. Entonces, llama a un método en el controlador para construir una URL de vídeo confiable, esto provoca que Angular permita el enlace en el `<iframe src>`:


<code-example path="security/src/app/bypass-security.component.html" header="src/app/bypass-security.component.html (iframe)" region="iframe"></code-example>



<code-example path="security/src/app/bypass-security.component.ts" header="src/app/bypass-security.component.ts (trust-video-url)" region="trust-video-url"></code-example>




<h2 id='http'>
  Vulnerabilidades a nivel HTTP
</h2>


Angular tiene soporte integrado para ayudar a prevenir dos vulnerabilidades HTTP comunes, la falsificación de solicitudes entre sitios (CSRF o XSRF) y la inclusión de scripts entre sitios (XSSI). Ambos deben mitigarse principalmente en el lado del servidor, pero Angular proporciona ayudantes para facilitar la integración en el lado del cliente.


<h3 id='xsrf'>
  Falsificación de solicitudes entre sitios (Cross-Site Request Forgery)
</h3>


En una falsificación de solicitudes entre sitios (CSRF o XSRF), un atacante engaña al usuario a visitar una página web diferente (como `evil.com`) con código malicioso que secretamente envía una petición maliciosa al servidor de la aplicación (como `example-bank.com`).

Asume que el usuario ha iniciado sesión a la aplicación en `example-bank.com`. El usuario abre un correo electrónico y cliquea un enlace a `evil.com`, que abre una nueva pestaña.

La página `evil.com` envía inmediatamente una solicitud maliciosa a `example-bank.com`. Quizás es una solicitud para transferir dinero de la cuenta del usuario a la cuenta del atacante. El explorador automáticamente envía las cookies del `example-bank.com` (incluyendo la cookie de autenticación) con esta solicitud.

Si el servidor de `example-bank.com` carece de protección contra XSRF, no puede saber la diferencia entre una solicitud legítima de la aplicación y la solicitud falsa de `evil.com`.

Para prevenir esto, la aplicación debe asegurarse que una solicitud de un usuario se origina de la aplicación real, no de un sitio diferente. El servidor y el cliente deben cooperar para frustrar este ataque.

En una técnica común anti-XSRF, el servidor de la aplicación envía un token de autenticación generado aleatoriamente en una cookie. El código del cliente lee la cookie y añade una solicitud personalizada en la cabecera (*header*) con el token en todas las solicitudes subsecuentes. El servidor compara el valor de la cookie recibida con el valor del la solicitud de la cabecera y rechaza la solicitud si los valores están perdidos o no coinciden.

Esta técnica es efectiva porque todos los exploradores implementan la _misma política de origen_. Solo el código del sitio web en el que las cookies se establecen pueden leer las cookies de ese sitio y establecer encabezados personalizados en solicitudes a ese sitio. Eso significa que solo tu aplicación puede leer el token de esta cookie y establecer la cabecera personalizada. El código malicioso en `evil.com` no puede hacer esto.

El método `Httpclient` de Angular tiene soporte integrado para la mitad de la técnica del lado del cliente. Lee más sobre esto en la [guía de HttpClient](/guide/http#security-xsrf-protection).

Para obtener información sobre CSRF en el Proyecto de Seguridad en las Aplicaciones Web Abiertas (Open Web Application Security Project (OWASP)), ve a <a href="https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29">Falsificación de Solicitudes Entre Sitios (CSRF)</a> y la 
<a href="https://www.owasp.org/index.php/CSRF_Prevention_Cheat_Sheet">Hoja de ayuda en la prevención de Falsificación de Solicitudes Entre Sitios (CSRF)</a>.

El documento de la Universidad de Stanford <a href="https://seclab.stanford.edu/websec/csrf/csrf.pdf">Defensas robustas contra la Falsificación de Solicitudes Entre Sitios (CSRF)</a> es una fuente rica en detalles.

También vea la <a href="https://www.youtube.com/watch?v=9inczw6qtpY" title="Cross Site Request Funkery Securing Your Angular Apps From Evil Doers">charla fácil de entender de Dave Smith sobre XSRF en el AngularConnect del 2016</a>.


<h3 id='xssi'>
  Inclusión de Secuencia de Comandos Entre Sitios Cruzados (Cross-site script inclusion - XSSI)
</h3>


La Inclusión de Secuencia de Comandos Entre Sitios Cruzados, también conocida como vulnerabilidad en JSON, puede permitir que el sitio web de un atacante lea datos de una API JSON. El ataque funciona en exploradores viejos al sobreescribir constructores de objetos nativos en JavaScript, y luego incluir una API URL utilizando una etiqueta `<script>`.

Este ataque solo es exitoso y el JSON retornado es ejecutable como si fuese JavaScript. Los servidores pueden prevenir un ataque al prefijar todas las respuestas JSON para hacerlas como no ejecutables, por convención, usando una cadena de texto muy conocida: `")]}',\n"`.

La librería de Angular `HttpClient` reconoce esta convención y automáticamente separa la cadena de texto `")]}',\n"` de todas las respuestas antes de seguir analizándola.

Para más información, vea la sección de XSSI en la [Publicación en el Blog de seguridad web de Google](https://security.googleblog.com/2011/05/website-security-for-webmasters.html).



<h2 id='code-review'>
  Auditando Aplicaciones de Angular
</h2>


Las aplicaciones de Angular deben seguir los mismos principios de seguridad como una aplicación web común, y debe ser auditada como tal. Las APIs específicas de Angular que deberían ser auditadas en una revisión de seguridad, tal como los métodos  [_bypassSecurityTrust_](guide/security#bypass-security-apis), están marcados en la documentación como sensible en lo que respecta a la seguridad.
