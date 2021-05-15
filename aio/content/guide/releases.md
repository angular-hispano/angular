# Versionado y lanzamientos de Angular

Reconocemos que al utilizar el framework Angular necesitas estabilidad. La estabilidad asegura que librerías y componentes reutilizables, tutoriales, herramientas, y las prácticas aprendidas no queden obsoletas de manera inesperada. La estabilidad es esencial para que prospere el ecosistema alrededor de Angular.

Compartimos el deseo de que Angular siga evolucionando. Nos esforzamos por garantizar que las bases sobre las que construyas mejoren continuamente para que puedas estar al día con el resto del ecosistema web y las necesidades de tus usuarios.

Este documento contiene las prácticas que seguimos para proveer una plataforma de desarrollo de aplicaciones de vanguardia equilibrada con estabilidad. Nos esforzaremos por garantizar que futuros cambios siempre sean predecibles. Queremos que todos los que dependan de Angular sepan cuándo y cómo serán añadidas nuevas características, y que puedan estar bien preparados cuando se eliminen las obsoletas.


<div class="alert is-helpful">

Las prácticas descritas en este documento aplican para Angular 2.0 y posteriores. Si actualmente te encuentras usando AngularJS mira [Actualizando desde Angular JS](guide/upgrade "Upgrading from AngularJS"). _AngularJS_ es el nombre de todas las versiones v1.x de Angular.

</div>

{@a versioning}
## Versionado de Angular

ALos números de versionado de Angular indican la cantidad de cambios que son introducidos por versión. El uso de [versionado semántico](https://semver.org/ "Semantic Versioning Specification") ayuda a comprender el potencial impacto de actualizar a una nueva versión.

Los números de versionado de Angular tienen tres partes: `principal.menor.parche`. Por ejemplo, la version 7.2.11 indica version principal 7, version menor 2, y parche nivel 11.

El número de version es incrementado en base al nivel de cambios que incluya el lanzamiento.

* Los **lanzamientos principales** contienen funcionalidades nuevas y significativas, se espera algo de asistencia mínima de los desarrolladores durante la actualización. Al actualizar a una nueva versión principal, es posible que debas ejecutar scripts de actualización, refactorizar código, correr tests adicionales, y aprender nuevas APIs.


* Los **lanzamientos menores** contienen nuevas funcionalidades más pequeñas. Los lanzamientos menores son completamente retrocompatibles; no se espera asistencia de los desarrolladores durante la actualización, pero, opcionalmente, puedes modificar tus aplicaciones y bibliotecas para comenzar a usar las nuevas APIs, funcionalidades, y capacidades que hayan sido agregadas en el lanzamiento. Actualizamos las peer dependencies en versiones menores ampliando las versiones compatibles, pero no requerimos que los proyectos actualicen estas dependencias.


* Los **lanzamientos de parches** son de bajo riesgo, lanzamientos de corrección de errores. No se requiere asistencia de los desarrolladores durante esta actualización.

<div class="alert is-helpful">

**Nota:** A partir de la versión 7 de Angular, las versiones principales de Angular core y CLI están alineadas. Esto significa que para usar la CLI mientras desarrollas una aplicación Angular, la versión de `@angular/core` y la CLI deben ser iguales.
</div>

{@a updating}
### Rutas de actualización soportadas

De acuerdo con el esquema de control de versiones descrito anteriormente, nos comprometemos a soportar las siguientes rutas de actualización:

* Si estás actualizando dentro de la **misma versión principal,** puedes omitir las versiones intermedias y actualizar directamente a la versión deseada. Por ejemplo, puedes actualizar directamente de la versión 7.0.0 a 7.2.11.


* Si estás actualizando de **una versión principal a otra,** recomendamos que **no te saltes las versiones principales.** Sigue las instrucciones para actualizar gradualmente a la próxima versión principal, probando y validando en cada paso. Por ejemplo, si deseas actualizar de la version 6.x.x a la version 8.x.x, te recomendamos que primero actualices a la última 7.x.x. Después de actualizar con éxito a 7.x.x, puedes actualizar a 8.x.x.


Mira [Manteniéndose al día](guide/updating "Updating your projects") para obtener más información sobre cómo actualizar tus proyectos de Angular a la versión más reciente.


{@a previews}
### Versiones de vista previa

Permitimos obtener una vista previa de lo que viene al proporcionar "Next" y Candidatos de lanzamiento (`rc`) prelanzamientos para cada lanzamiento principal y menor:

* **Next:** Este lanzamiento se encuentra activamente en desarrollo y pruebas. La próxima versión se indica mediante una etiqueta de versión adjunta con el identificador `-next`, como `8.1.0-next.0`.

* **Candidato de lanzamiento:** Una versión que tiene funcionalidades completas y en prueba final. Un candidato de lanzamiento se identifica mediante una etiqueta de lanzamiento adjunta con el identificador `-rc`, como la versión `8.1.0-rc.0`.

La ultima versión preliminar de la documentación `next` o `rc` está disponible en [next.angular.io](https://next.angular.io).


{@a frequency}
## Frecuencia de lanzamientos

Trabajamos por un calendario regular de lanzamientos, para que puedas planificar y coordinar tus actualizaciones con la continua evolución de Angular.

<div class="alert is-helpful">

Descargo de responsabilidad: las fechas se ofrecen como orientación general y las ajustaremos cuando sea necesario para garantizar la entrega de una plataforma de alta calidad.

</div>

En general, puedes esperar el siguiente ciclo de lanzamientos:

* Un lanzamiento principal cada 6 meses.

* 1-3 lanzamientos menores por cada lanzamiento principal

* Un lanzamiento de parche y un lanzamiento previo (`next` o `rc`) casi todas las semanas

Este ritmo de lanzamientos brinda a los desarrolladores impacientes acceso a nuevas funciones tan pronto como estén completamente desarrolladas y pasen por nuestros procesos de revisión de código y pruebas de integración, mientras se mantiene la estabilidad y confiabilidad de la plataforma para los usuarios de producción que prefieren recibir funciones después de haber sido validadas por Google y otros desarrolladores que usan las compilaciones previas al lanzamiento.



{@a lts}
{@a support}
## Política de soporte y calendario

Todas nuestras versiones principales tienen soporte durante 18 meses.

* 6 meses de * soporte activo *, durante los cuales se lanzan actualizaciones y parches programados regularmente.

* 12 meses de * soporte a largo plazo (LTS) *, durante los cuales solo se publican parches de seguridad y correcciones críticas.

La siguiente tabla proporciona el estado de las versiones de Angular en soporte.


Version | Estatus| Lanzamiento  |Activo termina| LTS termina
------- | ------ | ------------ | ------------ | ------------
^10.0.0 | Activo | Jun 24, 2020 | Dic 24, 2020 | Dic 24, 2021
^9.0.0  | Activo | Feb 06, 2020 | Ago 06, 2020 | Ago 06, 2021
^8.0.0  | LTS    | May 28, 2019 | Nov 28, 2019 | Nov 28, 2020

Versiones Angular ^4.0.0, ^5.0.0, ^6.0.0 y ^7.0.0 ya no están bajo soporte.

{@a deprecation}
## Prácticas de deprecación


En ocasiones &quot;cambios importantes&quot;, como la eliminación de la compatibilidad con determinadas API y funcionalidades, son necesarios para innovar y mantenerse al día con las nuevas mejores prácticas, dependencias cambiantes, o cambios en la propia plataforma (web).

Para hacer estas transiciones lo más sencillas posibles, hacemos estos compromisos contigo:

* Trabajaremos arduamente para minimizar la cantidad de cambios importantes y brindar herramientas de migración cuando sea posible.

* Seguiremos la política de deprecado que se describe aquí, por lo que tendrás tiempo para actualizar tus aplicaciones a las últimas API y mejores prácticas.

Para ayudarte a asegurar que tengas tiempo suficiente y una ruta clara para actualizar, esta es nuestra política de deprecado:

* **Anuncio:** Anunciamos API y funciones obsoletas en el [registro de cambios](https://github.com/angular/angular/blob/master/CHANGELOG.md "Angular change log"). Las API obsoletas aparecen en la [documentación](api?status=deprecated) con ~~tachado.~~ Cuando anunciamos una deprecación, también anunciamos una ruta de actualización recomendada. Por conveniencia,  [Deprecations](guide/deprecations) contiene un resumen de las API y las funciones obsoletas.

* **Periodo de deprecación:** Cuando una API o una función está obsoleta, seguirá estando presente en las dos próximas versiones principales. Después de eso, las API y las funciones obsoletas serán candidatas a la eliminación. Se puede anunciar una deprecación en cualquier versión, pero la eliminación de una API o una función obsoleta solo ocurrirá en la versión principal. Hasta que se elimine una funcionalidad o API obsoleta, se mantendrá de acuerdo con la política de soporte de LTS, lo que significa que solo se solucionarán los problemas críticos y de seguridad.

* **Dependencias npm:** Solo realizamos actualizaciones de dependencias de npm que solicitan cambios en las aplicaciones en un lanzamiento principal. En lanzamientos menores, actualizamos las peer dependencies ampliando las versiones compatibles, pero no se requiere que los proyectos actualicen estas dependencias hasta una futura versión principal. Esto significa que durante los lanzamientos menores de Angular, las actualizaciones de dependencias de npm dentro de las aplicaciones y bibliotecas de Angular son opcionales.




{@a public-api}
## Superficie de API pública

Angular es una colección de muchos paquetes, subproyectos y herramientas. Para evitar el uso accidental de API privadas&mdash;y para comprender claramente lo que cubren las prácticas descritas aquí&mdash;documentamos lo que se considera y no se considera nuestra superficie de API pública. Para obtener más detalles, consulta [Superficie de API pública admitida de Angular](https://github.com/angular/angular/blob/master/docs/PUBLIC_API.md "Supported Public API Surface of Angular").

Cualquier cambio en la superficie de la API pública se realizará mediante las políticas de control de versiones, soporte y deprecado descritas anteriormente.

{@a labs}
## Angular Labs

Angular Labs es una iniciativa para cultivar nuevas funciones e iterar sobre ellas rápidamente. Angular Labs proporciona un lugar seguro para la exploración y experimentación del equipo de Angular.

Los proyectos de Angular Labs no están listos para su uso en producción y no se asume ningún compromiso para llevarlos a producción. Las políticas y prácticas que se describen en este documento no se aplican a los proyectos de Angular Labs.
