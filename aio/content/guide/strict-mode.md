# Modo Estricto

Cuando creas un nuevo espacio de trabajo o un proyecto, tienes la opción de crearlos en un modo estricto usando la marca `--strict`.

Habilitar esta marca inicializa su nuevo espacio de trabajo o proyecto con algunas configuraciones nuevas que mejoran la capacidad de mantenimiento, lo ayudan a detectar errores con anticipación y permiten que el CLI realice optimizaciones avanzadas en su aplicación.
Además, las aplicaciones que usan estas configuraciones más estrictas son más fáciles de analizar estáticamente, lo que puede ayudar a que el comando `ng update` refactorice el código de manera más segura y precisa cuando esté actualizando a futuras versiones de Angular.

Específicamente, la bandera "estricta" hace lo siguiente:

* Habilita [el modo `strict` en Typscript](https://www.staging-typescript.org/tsconfig#strict), así como otras marcas de rigor recomendadas por el equipo de TypeScript. Específicamente, `forceConsistentCasingInFileNames`, `noImplicitReturns`,  `noFallthroughCasesInSwitch`.
* Activa los indicadores estrictos del compilador Angular [`strictTemplates`](guide/angular-compiler-options#stricttemplates) y [`strictInjectionParameters`](guide/angular-compiler-options#strictinjectionparameters)
* [Los presupuestos de tamaño de paquete](guide/build#configuring-size-budgets) se han reducido en ~ 75%
* Activa la [regla tslint `no-any`] para evitar declaraciones de tipo `any`
* [Marca su aplicación como libre de efectos secundarios](https://webpack.js.org/guides/tree-shaking/#mark-the-file-as-side-effect-free) para permitir un tree-shaking más avanzado.

Puede aplicar esta configuración a nivel de proyecto y espacio de trabajo.

Para crear un nuevo espacio de trabajo y una aplicación usando el modo estricto, ejecute el siguiente comando:

<code-example language="sh" class="code-shell">

ng new [nombre-del-proyecto] --strict

</code-example>

Para crear una nueva aplicación en modo estricto dentro de un espacio de trabajo no estricto existente, ejecute el siguiente comando:

<code-example language="sh" class="code-shell">

ng generate application [nombre-del-proyecto] --strict

</code-example>

{@a side-effect}

### Efectos secundarios no locales en aplicaciones

Cuando crea proyectos y espacios de trabajo usando el modo `estricto`, notará un archivo` package.json` adicional, ubicado en el directorio `src/app/`.
Este archivo informa a las herramientas y a los paquetes que el código de este directorio está libre de efectos secundarios no locales. Los efectos secundarios no locales en el código de la aplicación no son comunes y su uso no se considera un buen patrón de codificación.
Más importante aún, el código con este tipo de efectos secundarios no se puede optimizar, lo que da como resultado un aumento de los tamaños de paquete y aplicaciones que se cargan más lentamente.

Si necesita más información, los siguientes enlaces pueden resultar útiles.

* [Tree-shaking](https://webpack.js.org/guides/tree-shaking/)
* [Lidiar con los efectos secundarios y las funciones puras en JavaScript](https://dev.to/vonheikemen/dealing-with-side-effects-and-pure-functions-in-javascript-16mg)
* [Cómo lidiar con los efectos secundarios sucios en su función pura JavaScript](https://jrsinclair.com/articles/2018/how-to-deal-with-dirty-side-effects-in-your-pure-functional-javascript/)
