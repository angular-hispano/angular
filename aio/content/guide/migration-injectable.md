# Migración por falta de decoradores `@Injectable()` y definiciones incompletas de los provider

### ¿Para qué sirve este esquema?

  1. Este esquema añade el decorador `@Injectable()` a las clases que son suministradas a la aplicación pero  no tienen este decorador.
  2. El esquema actualiza los proveedores que siguen el patrón `{provide: SomeToken}` para ser explícitamente específicos `useValue: undefined`.

**Ejemplo de la falta del decorador `@Injectable()`**

_Antes de la migración:_
```typescript
export class MyService {...}
export class MyOtherService {...}
export class MyThirdClass {...}
export class MyFourthClass {...}
export class MyFifthClass {...}

@NgModule({
  providers: [
    MyService,
    {provide: SOME_TOKEN, useClass: MyOtherService},
    // The following classes do not need to be decorated because they
    // are never instantiated and just serve as DI tokens.
    {provide: MyThirdClass, useValue: ...},
    {provide: MyFourthClass, useFactory: ...},
    {provide: MyFifthClass, useExisting: ...},
  ]
})
```

_Después de la migración:_
```ts
@Injectable()
export class MyService {...}
@Injectable()
export class MyOtherService {...}
export class MyThirdClass {...}
export class MyFourthClass {...}
export class MySixthClass {...}

...
```

Note que `MyThirdClass`, `MyFourthClass` y `MyFifthClass` no necesitan tener el decorador
`@Injectable()` porque nunca se instancian, pero sólo se utiliza como [DI token][DI_TOKEN] .

**Ejemplo de un proveedor que necesita `useValue: undefined`**

Este ejemplo muestra un proveedor siguiendo el patrón `{provide: X}`.
El proveedor necesita ser migrado para tener una definición más explicita donde `useValue: undefined` es especificado.

_Antes de la migración_:
```typescript
{provide: MyToken}
```
_Después de la migración_:
```typescript
{provide: MyToken, useValue: undefined}
```

### ¿Por qué es necesario añadir `@Injectable()`?

En nuestra documentación, siempre hemos recomendando añadir el decorador `@Injectable()` a cualquier clase que provee o es inyectada en su aplicación.
Sin embargo, en ciertos casos las versiones anteriores de Angular permitían la inyección de una clase sin el decorador, como en el modo AOT.
Esto significa que si accidentalmente usted omite el decorador, su aplicación podría seguir funcionando a pesar de que el decorador `@Injectable()` falte en algunos lugares. Esta es una problemática para futuras versiones de Angular.
Eventualmente, nosotros planeamos requerir estrictamente el decorador por qué al hacerlo permite una mayor optimización tanto del compilador como del tiempo de ejecución.
Este esquema añade cualquier decorador `@Injectable()` que pueda hacer falta para que su aplicación este preparada para el futuro.

### ¿Por qué es necesario añadir `useValue: undefined`?

Considere el siguiente patrón:

```typescript
@NgModule({
  providers: [{provide: MyService}]
})
```
Los proveedores que utilicen este patrón se comportarán como si se proporcionara `MyService` como [DI token][DI_TOKEN] con el valor de `undefined`.
Este no es el caso de Ivy, donde los proveedores serán interpretados como si `useClass: MyService` fuera especificado. Esto significa que los proveedores se comportarán de manera diferente cuando se actualicen a la version 9 o superiores. Para estar seguros de que el proveedor se comporte de la misma manera que antes, el valor del DI debería ser explícitamente `undefined`.
### ¿Cuando debería agregar el decorador `@Injectable()` a las clases ?

Cualquier clase que sea un proveedor debe tener un decorador `@Injectable()`
El decorador es necesario para el framework cree correctamente una instancia de esa clase a través del DI.

Sin embargo, las clases que ya tienen los decoradores `@Pipe`, `@Component` o `@Directive` no necesitan tener `@Injectable()`.
El decorador de la clase existente ya le da instrucciones al compilador para generar la información necesaria.
### ¿Debería actualizar mi librería?
Si, sí tu librería tiene alguna clase que debería ser inyectada, estas deberían ser actualizadas con el decorador `@Injectable()`
En una futura versión de Angular, la falta del decorador `@Injectable()` siempre arrojará un error.

Adicionalmente, los proveedores de las librerías que siguen el patrón `{provide: X}` deberían ser actualizadas para especificar un valor explicito. Sin un valor explicito, estos proveedores podrán comportarse diferente de acuerdo a la versión de Angular en aplicaciones que consumen su librería.

[DI_TOKEN]: guide/glossary#di-token