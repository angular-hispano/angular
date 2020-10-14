# Migración de `Renderer` a `Renderer2`

## Descripción general de la migración

La clase `Renderer` ha sido marcada como obsoleta desde la versión 4 de Angular. Esta sección proporciona orientación sobre la migración de esta API obsoleta a la `Renderer2` API más nueva y lo que significa para su aplicación.

## ¿Por qué debería migrar a Renderer2?

La clase obsoleta `Renderer` se eliminó en la versión 9 de Angular, por lo que es necesario migrar a una API compatible.
Usar `Renderer2` es la estrategia recomendada porque admite un conjunto de funciones similar a `Renderer`.
La superficie de la API es bastante grande (con 19 métodos), pero el esquema debería simplificar este proceso para sus aplicaciones.

## ¿Se requiere alguna acción por mi parte?

No. El esquema debería manejar la mayoría de los casos con la excepción de `Renderer.animate()` y `Renderer.setDebugInfo()`, que ya no son compatibles.

## ¿Cuales son los metodos `__ngRendererX`? ¿Por qué son necesarios?

Algunos métodos no tienen equivalentes exactos en `Renderer2`, o corresponden a más de una expresión.
Por ejemplo, ambos procesadores tienen un metodo `createElement()`, pero no son iguales porque una llamada como `renderer.createElement(parentNode, namespaceAndName)` en el `Renderer` corresponde al siguiente bloque de código en `Renderer2`:

```ts
const [namespace, name] = splitNamespace(namespaceAndName);
const el = renderer.createElement(name, namespace);
if (parentNode) {
  renderer.appendChild(parentNode, el);
}
return el;
```

La migración debe garantizar que los valores de retorno de las funciones y los tipos de variables permanezcan iguales. Para manejar la mayoría de los casos de forma segura, el esquema declara funciones auxiliares en la parte inferior del archivo del usuario. Estos ayudantes encapsulan su propia lógica y mantienen los reemplazos dentro de su código en una sola llamada de función. A continuación, se muestra un ejemplo de cómo se ve la migración `createElement()`:

**Antes:**

```ts
public createAndAppendElement() {
  const el = this.renderer.createElement('span');
  el.textContent = 'hello world';
  return el;
}
```

**Después:**

<code-example>

public createAndAppendElement() {
const el = **ngRendererCreateElement(this.renderer, this.element, 'span');
el.textContent = 'hello world';
return el;
}
// Generated code at the bottom of the file
**ngRendererCreateElement(renderer: any, parentNode: any, nameAndNamespace: any) {
const [namespace, name] = **ngRendererSplitNamespace(namespaceAndName);
const el = renderer.createElement(name, namespace);
if (parentNode) {
renderer.appendChild(parentNode, el);
}
return el;
}
**ngRendererSplitNamespace(nameAndNamespace: any) {
// returns the split name and namespace
}

</code-example>

Al implementar estas funciones auxiliares, el esquema garantiza que solo se declaren una vez por archivo y que sus nombres sean lo suficientemente únicos como para que exista una pequeña posibilidad de colisionar con funciones preexistentes en su código. El esquema también mantiene sus tipos de parámetros `any` para que no tenga que insertar lógica adicional que asegure que sus valores tengan el tipo correcto.

### Soy autor de una biblioteca. ¿Debería ejecutar esta migración?

**Los autores de bibliotecas definitivamente deberían usar esta migración para alejarse del `Renderer`. De lo contrario, las bibliotecas no funcionarán con aplicaciones creadas con la versión 9.**

### Lista completa de migraciones de métodos

La siguiente tabla muestra todos los métodos que la migración maps desde `Renderer` a `Renderer2`.

| Renderer                                                                                 | Renderer2                                                                                                                                                                                      |
| ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `listen(renderElement, name, callback)`                                                  | `listen(renderElement, name, callback)`                                                                                                                                                        |
| `setElementProperty(renderElement, propertyName, propertyValue)`                         | `setProperty(renderElement, propertyName, propertyValue)`                                                                                                                                      |
| `setText(renderNode, text)`                                                              | `setValue(renderNode, text)`                                                                                                                                                                   |
| `listenGlobal(target, name, callback)`                                                   | `listen(target, name, callback)`                                                                                                                                                               |
| `selectRootElement(selectorOrNode, debugInfo?)`                                          | `selectRootElement(selectorOrNode)`                                                                                                                                                            |
| `createElement(parentElement, name, debugInfo?)`                                         | `appendChild(parentElement, createElement(name))`                                                                                                                                              |
| `setElementStyle(el, style, value?)`                                                     | `value == null ? removeStyle(el, style) : setStyle(el, style, value)`                                                                                                                          |
| `setElementAttribute(el, name, value?)`                                                  | `attributeValue == null ? removeAttribute(el, name) : setAttribute(el, name, value)`                                                                                                           |
| `createText(parentElement, value, debugInfo?)`                                           | `appendChild(parentElement, createText(value))`                                                                                                                                                |
| `createTemplateAnchor(parentElement)`                                                    | `appendChild(parentElement, createComment(''))`                                                                                                                                                |
| `setElementClass(renderElement, className, isAdd)`                                       | `isAdd ? addClass(renderElement, className) : removeClass(renderElement, className)`                                                                                                           |
| `projectNodes(parentElement, nodes)`                                                     | `for (let i = 0; i < nodes.length; i++) { appendChild(parentElement, nodes[i]); }`                                                                                                             |
| `attachViewAfter(node, viewRootNodes)`                                                   | `const parentElement = parentNode(node); const nextSibling = nextSibling(node); for (let i = 0; i < viewRootNodes.length; i++) { insertBefore(parentElement, viewRootNodes[i], nextSibling);}` |
| `detachView(viewRootNodes)`                                                              | `for (let i = 0; i < viewRootNodes.length; i++) {const node = viewRootNodes[i]; const parentElement = parentNode(node); removeChild(parentElement, node);}`                                    |
| `destroyView(hostElement, viewAllNodes)`                                                 | `for (let i = 0; i < viewAllNodes.length; i++) { destroyNode(viewAllNodes[i]); }`                                                                                                              |
| `setBindingDebugInfo()`                                                                  | This function is a noop in `Renderer2`.                                                                                                                                                        |
| `createViewRoot(hostElement)`                                                            | Should be replaced with a reference to `hostElement`                                                                                                                                           |
| `invokeElementMethod(renderElement, methodName, args?)`                                  | `(renderElement as any)[methodName].apply(renderElement, args);`                                                                                                                               |
| `animate(element, startingStyles, keyframes, duration, delay, easing, previousPlayers?)` | Throws an error (same behavior as `Renderer.animate()`)                                                                                                                                        |
