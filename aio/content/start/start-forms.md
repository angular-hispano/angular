# Inténtalo: Usa formularios para capturar la información del usuario

Una vez terminado [Gestión de datos](start/start-data "Try it: Managing Data"), la aplicación tienda en línea tiene un catálogo de productos y un carrito de compras.

Esta sección te guía para agregar una funcionalidad de pago con un formulario para capturar los datos del usuario.

## Formularios en Angular

Los formularios en Angular están construidos sobre el formulario HTML estándar, para ayudarte a crear controladores personalizados y simplificar la experiencia de validación de campos. Un formulario reactivo en Angular se compone de dos partes: los objetos que viven en el componente para almacenar y gestionar el formulario, y la visualización que vive en la plantilla.

## Definir el modelo del formulario de pago

Primero, configura el modelo del formulario de pago. Se define en el componente clase, este modelo es la fuente de la verdad para el estado del formulario.

1. Abre `cart.component.ts`.

2. El servicio de Angular `FormBuilder` proporciona los métodos para crear los controladores. Tal como otros servicios que hayas usado, necesitas importar e inyectar el servicio antes de usarlo:

   1. Importar `FormBuilder` del módulo `@angular/forms`.

     <code-example header="src/app/cart/cart.component.ts" path="getting-started/src/app/cart/cart.component.ts" region="imports">
     </code-example>

   El módulo `ReactiveFormsModule` proporciona el servicio `FormBuilder`, ya ha importado `AppModule` (en `app.module.ts`).

   2. Inyecta el servicio `FormBuilder`.

     <code-example header="src/app/cart/cart.component.ts" path="getting-started/src/app/cart/cart.component.ts" region="inject-form-builder">
     </code-example>

3. En la clase `CartComponent`, define la propiedad `checkoutForm` para almacenar el modelo del formulario.

<code-example header="src/app/cart/cart.component.ts" path="getting-started/src/app/cart/cart.component.ts" region="checkout-form">
</code-example>

4. Para capturar el nombre y la dirección del usuario, configura la propiedad `checkoutForm` usando el método `group()` de `FormBuilder` a un formulario que contenga los campos `name` y `addess`. Agrega esto entre las llaves, `{}`, del constructor.

   <code-example header="src/app/cart/cart.component.ts" path="getting-started/src/app/cart/cart.component.ts" region="checkout-form-group"></code-example>

5. Para el proceso de pago, el usuario debe enviar el nombre y la dirección. Una vez enviada la orden se debe limpiar el formulario y el carrito.

   1. En `cart.component.ts`, define un método `onSubmit()` para procesar el formulario. Usa el método `clearCart()` de `CartService` para eliminar los elementos del carrito y restaurar el formulario después de enviarlo. En una aplicación de la vida real, este método también debe enviar los datos a un servidor externo. Por último el componente completo se vería así:

   <code-example header="src/app/cart/cart.component.ts" path="getting-started/src/app/cart/cart.component.ts">
   </code-example>

Habiendo definido el modelo del formulario en el componente, necesitas un formulario de pago que refleje el modelo en la vista.

## Crear el formulario de pago

Sigue estos pasos para agregar un formulario de pago en la sección inferior de la vista del componente "Cart".

1. Abre `cart.component.html`.

2. Al final de la plantilla agrega un formulario HTML para capturar los datos del usuario.

3. Usa el enlace de propiedades de `formGroup` para enlazar el `checkoutForm` a la etiqueta `form` en la plantilla. También agrega un botón 'Comprar' para enviar el formulario.

  <code-example header="src/app/cart/cart.component.html" path="getting-started/src/app/cart/cart.component.3.html" region="checkout-form">
  </code-example>

4. En la etiqueta form, usa el evento `ngSubmit` para escuchar el envío del formulario e invocar el método `onSubmit()` con el valor `checkoutForm`.

  <code-example path="getting-started/src/app/cart/cart.component.html" header="src/app/cart/cart.component.html (cart component template detail)" region="checkout-form-1">
  </code-example>

5. Agrega los campos `name` y `address`. Usa el atributo enlazador `formControlName` para enlazar los controladores `name` y `address` de `checkoutForm` a sus respectivas etiquetas input. El componente final es el siguiente:

  <code-example path="getting-started/src/app/cart/cart.component.html" header="src/app/cart/cart.component.html" region="checkout-form-2">
  </code-example>

Después de agregar algunos elementos en el carrito, el usuario ahora puede verificar los artículos a comprar, ingresar su nombre y dirección y enviar su compra:

<div class="lightbox">
  <img src='generated/images/guide/start/cart-with-items-and-form.png' alt="Cart view with checkout form">
</div>

Para confirmar el envío del formulario, abre la consola donde podrás ver un objeto que contiene el nombre y la dirección enviadas.

## Siguientes pasos

¡Felicitaciones! Has completado una tienda online con un catálogo de productos, un carrito de compras, y una funcionalidad de pagos.

[Continúa con la sección "Despliegue"](start/start-deployment "Try it: Deployment") para desplegar en local, o despliega tu aplicación en Firebase o tu propio servidor.
