# App shell

App shell es una manera de renderizar una porción de tu aplicación a través de una ruta en tiempo de compilación (build time).
Puede mejorar la experiencia de usuario lanzando rápidamente una page estática renderizada (un esqueleto común a todas las páginas) mientras el navegador descarga la versión completa del cliente y cambia a éste al finalizar su carga.

Esto da a los usuarios una primera visualización significativa de su aplicación que aparece rápidamente porque el navegador simplemente puede renderizar HTML y CSS sin la necesidad de inicializar JavaScript.

Obténga más información en [El modelo de aplicación Shell](https://developers.google.com/web/fundamentals/architecture/app-shell).

## Paso 1: Prepara la aplicación

Puede hacer esto con el siguiente comando CLI:
<code-example language="bash">
ng new my-app --routing
</code-example>

Para una aplicación existente, debe agregar manualmente el `RouterModule` y definir un` <router-outlet> `dentro de su aplicación.

## Paso 2: Cree el shell de la aplicación

Use la CLI para crear automáticamente el shell de la aplicación.

<code-example language="bash">
ng generate app-shell
</code-example>

* `client-project` toma el nombre de su aplicación cliente.

Después de ejecutar este comando, notará que el archivo de configuración `angular.json` se ha actualizado para agregar dos nuevos targets, con algunos otros cambios.

<code-example language="json">
"server": {
  "builder": "@angular-devkit/build-angular:server",
  "options": {
    "outputPath": "dist/my-app-server",
    "main": "src/main.server.ts",
    "tsConfig": "tsconfig.server.json"
  }
},
"app-shell": {
  "builder": "@angular-devkit/build-angular:app-shell",
  "options": {
    "browserTarget": "my-app:build",
    "serverTarget": "my-app:server",
    "route": "shell"
  },
  "configurations": {
    "production": {
      "browserTarget": "my-app:build:production",
      "serverTarget": "my-app:server:production"
    }
  }
}
</code-example>

## Paso 3: Verifique que la aplicación esté construida con el contenido del shell

Use la CLI para construir el `app-shell` target.

<code-example language="bash">
ng run my-app:app-shell
</code-example>

O use la configuración de producción.

<code-example language="bash">
ng run my-app:app-shell:production
</code-example>

Para verificar el resultado de la compilación, abra `dist/my-app/index.html`. Busque el texto por defecto `app-shell works!` para mostrar que la ruta del shell de la aplicación se ha renderizado como parte de la carpeta de distribución.
