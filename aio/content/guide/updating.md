# Mantener sus proyectos de Angular actualizados

Al igual que Web y todo el ecosistema web, Angular está mejorando continuamente. Angular equilibra la mejora continua con un fuerte enfoque en la estabilidad y en facilitar las actualizaciones. Mantener su aplicación Angular actualizada le permite aprovechar las nuevas características de vanguardia, así como las optimizaciones y correcciones de bug.

Este documento contiene información y recursos para ayudarte a mantener tus aplicaciones y bibliotecas de Angular actualizadas.

Para obtener información sobre nuestra política y prácticas de versiones&mdash;incluyendo las prácticas de soporte y depreciación, así como el calendario de lanzamientos&mdash;consulte [Versiones y lanzamientos de Angular](guide/releases "Versiones y lanzamientos de Angular").


<div class="alert is-helpful">

Si actualmente utiliza AngularJS, consulte [Actualizando desde AngularJS](guide/upgrade "Actualizando desde AngularJS"). _AngularJS_ es el nombre de todas las versiones v1.x de Angular.

</div>


{@a announce}
## Recibir notificaciones de nuevas publicaciones

Para recibir una notificación cuando haya nuevas publicaciones, siga [@angular](https://twitter.com/angular "@angular en Twitter") en Twitter o suscríbase al [ blog de Angular](https://blog.angular.io "blog de Angular").

{@a learn}
## Aprender acerca de las nuevas funcionalidades 

¿Qué hay de nuevo? ¿Qué ha cambiado? Compartimos las cosas más importantes que necesitas saber en el blog de Angular en [anuncios de lanzamiento]( https://blog.angular.io/tagged/release%20notes "blog de Angular - anuncios de lanzamiento").

Para revisar la lista completa de cambios, organizados por versión, vea el [Registro de cambios de Angular](https://github.com/angular/angular/blob/master/CHANGELOG.md "Registro de cambios de Angular").


{@a checking-version-app}
## Comprobando su versión de Angular

Para comprobar la versión de Angular de tu aplicación: Desde el directorio de tu proyecto, utiliza el comando `ng version`.


{@a checking-version-angular}
## Encontrando la versión actual de Angular

La versión estable más reciente de Angular aparece en la [documentación de Angular](https://angular.io/docs "documentación de Angular") en la parte inferior de la navegación lateral izquierda. Por ejemplo, `stable (v5.2.9)`.

También puede encontrar la versión más actual de Angular utilizando el comando CLI [`ng update`](cli/update). por defecto, [`ng update`](cli/update) (sin argumentos adicionales) lista las actualizaciones que están disponibles para usted.


{@a updating}
## Actualizando su entorno y aplicaciones

Para facilitar la actualización, proporcionamos instrucciones completas en la [Guía de actualización de Angular](https://update.angular.io/ "Guía de actualización de Angular").

La Guía de Actualización de Angular proporciona instrucciones de actualización personalizadas, basadas en las versiones actuales y de destino que usted especifique. Incluye rutas de actualización básicas y avanzadas, para adaptarse a la complejidad de sus aplicaciones. También incluye información sobre la resolución de problemas y cualquier cambio manual recomendado para ayudarle a sacar el máximo provecho de la nueva versión.

Para las actualizaciones simples, el comando CLI [`ng update`](cli/update) es todo lo que necesitas. Sin argumentos adicionales, [`ng update`](cli/update) enumera las actualizaciones disponibles y proporciona los pasos recomendados para actualizar su aplicación a la versión más actual.

[Versiones y lanzamientos de Angular](guide/releases#versioning "Prácticas de lanzamiento, versionado de Angular") describe el nivel de cambio que puede esperar basándose el número de versión de un lanzamiento. También describe las rutas de actualización admitidas.


{@a resources}
## Resumen de recursos

* Anuncios de lanzamiento: [blog de Angular - Anuncios de lanzamiento](https://blog.angular.io/tagged/release%20notes "Anuncios del blog de Angular sobre los últimos lanzamientos")

* Anuncios de lanzamiento (antiguos): [blog de Angular - anuncios sobre lanzamientos anteriores a agosto de 2017](https://blog.angularjs.org/search?q=available&by-date=true "Anuncios del blog de Angular sobre versiones anteriores a agosto de 2017")

* Detalles de lanzamiento: [Registro de cambios de Angular](https://github.com/angular/angular/blob/master/CHANGELOG.md "Registro de cambios de Angular")

* Instrucciones de actualización: [Guía de actualización de Angular](https://update.angular.io/ "Guía de actualización de Angular")

* Actualizar la referencia del comando: [Referencia del comando `ng update` de la CLI de Angular](cli/update)

* Prácticas de versionado, liberación, soporte y depreciación: [Versiones y lanzamientos de Angular](guide/releases "Versiones y lanzamientos de Angular")
