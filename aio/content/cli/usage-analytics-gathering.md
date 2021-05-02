# Recopilación y visualización de análisis de uso

Los usuarios pueden optar por compartir sus datos de uso del Angular CLI con [Google Analytics](https://support.google.com/analytics/answer/1008015?hl=en), usando el [comando del CLI `ng analytics`](analytics).
Los datos también son compartidos con el equipo de Angular, y son usados para mejorar el CLI.

La recopilación de los datos analiticos del CLI están deshabilitados por defecto, y deben ser habilitados al nivel del proyecto por usuarios individuales. Estos no pueden ser habilitados a nivel de proyecto por todos los usuarios.

Los datos recopilados de esta manera pueden ser visualizados en el sitio de Google Analytics, pero no están automáticamente visibles en el sitio Google Analytics de tu organización.
Como administrador de un grupo de desarrollo de Angular, tú puedes configurar tu instancia del Angular CLI para ser capaz de visualizar los datos analiticos del uso del Angular CLI de tu propio equipo.
Esta opción de configuración es independiente y adicional a otros análisis de uso que tus usuarios pueden estar compartiendo con Google.

## Habilitar el acceso a los datos de uso del CLI

Para configurar el acceso a los datos de uso del CLI de tus usuarios, usa el comando `ng config`  para añadir una llave al archivo de configuración global [`angular.json` workspace configuration file](guide/workspace-config).
La llave se encuentra debajo de `cli.analyticsSharing` en el nivel superior del archivo, fuera de la sección `projects`.
El valor de la llave es el tracking ID de tu organización, asignada por Google Analytics
Este ID es una cadena parecida a `UA-123456-12`.

Tú puedes escoger usar una cadena descriptiva como el valor de la llave, o que se asigne una llave aleatoria cuando se ejecute el comando del CLI.
Por ejemplo, el siguiente comando añade una llave de configuración llamada "tracking".

<code-example language="sh" class="code-shell">
ng config --global cli.analyticsSharing.tracking UA-123456-12
</code-example>

Para desactivar esta característica, ejecuta el siguiente comando:

<code-example language="sh" class="code-shell">
ng config --global --remove cli.analyticsSharing
</code-example>


## Por seguimiento de usuario

Puedes agregar un id de usuario personalizado en la configuración global, para identificar el uso exclusivo de los comandos y banderas.
Si ese usuario habilita el análisis CLI en su propio proyecto, su análisis muestra un seguimientos y etiquetas de su uso individual.


<code-example language="sh" class="code-shell">
ng config --global cli.analyticsSharing.user SOME_USER_NAME
</code-example>

Para generar un nuevo ID de usuario aleatorio, ejecuta el siguiente comando:

<code-example language="sh" class="code-shell">
ng config --global cli.analyticsSharing.user ""
</code-example>
