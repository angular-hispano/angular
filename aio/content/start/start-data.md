# Manejo de datos

Al final de [Navegación en la aplicación](start/start-routing "Navegación en la aplicación"), la aplicación de tienda en linea tiene un catalogo con dos vistas: una lista de producto y detalles del producto.
Los usuarios pueden hacer clic en el nombre de un producto de la lista para ver los detalles en una nueva vista, con una URL o ruta distinta.

Esta pagina te guiará a través de la creación del carrito de compras en tres fases:

* Actualice la vista de detalles del producto para incluir un botón de "Comprar", el cual agrega el producto actual a una lista de productos que administra un servicio de carrito.
* Agregue un componente cart, que muestra los artículos en el carrito.
* Agregue un componente de envío, el cual recupera los precios de envío de los artículos en el carrito mediante el uso del `HttpClient` de Angular para recuperar los datos de envío de un archivo `.json`. 

{@a services}
## Servicios

Los servicios son una parte integral de las aplicaciones de Angular. En Angular, un servicio es una instancia de una clase que se puede poner a disposición de cualquier parte de su aplicación usando el [sistema de inyección de dependencias](guide/glossary#dependency-injection "Definición de inyección de dependencias") de Angular.

Los servicios son el lugar donde se comparten datos entre las partes de su aplicación. Para la tienda en línea, el servicio de carrito es donde se almacenan los datos y métodos de su carrito.

{@a create-cart-service}
## Crear el servicio de carrito de compras

Hasta este punto, los usuarios pueden ver la información del producto y
simular compartir y recibir notificaciones sobre cambios de productos.
Sin embargo, no pueden comprar productos.

En esta sección, agregue un botón "Comprar" a la vista de los detalles del producto 
y configure un servicio de carrito para almacenar la información 
de los productos en el carrito.

<div class="alert is-helpful">

En una parte posterior de este tutorial, [Usar formularios para la entrada del usuario](start/start-forms "Formularios para la entrada del usuario"), lo guiará a través del acceso a este servicio de carrito desde la vista donde el usuario realiza el pago.

</div>

{@a generate-cart-service}
### Definir un servicio de carrito

1. Para generar un servicio de carrito, haga clic con el botón derecho en la carpeta `app`, elija `Angular Generator` y elija `Service`. Nombre el nuevo servicio `cart`.

        <code-example header="src/app/cart.service.ts" path="getting-started/src/app/cart.service.1.ts"></code-example>

    <div class="alert is-helpful>

    El generador StackBlitz podría proporcionar el servicio de carrito en `app.module.ts` de forma predeterminada. Eso difiere del ejemplo, el cual usa una técnica de optimización de paquetes, un decorador `@Injectable()` con la declaración `{ provideIn: 'root' }`.
    Para obtener más información sobre los servicios, consulte [Introducción a los servicios y la inyección de dependencia](guide/architecture-services "Conceptos > Introducción a los servicios y ID".

    </div>

1. En la clase `CartService`, defina una propiedad de `items` para almacenar el array de los productos actuales en el carrito.

    <code-example path="getting-started/src/app/cart.service.ts" header="src/app/cart.service.ts" region="props"></code-example>

1. Defina métodos para agregar artículos al carrito, retorne los artículos del carrito y borre los artículos del carrito.

    <code-example path="getting-started/src/app/cart.service.ts" header="src/app/cart.service.ts" region="methods"></code-example>

    * El método `addToCart()` agrega un producto a un array de `items`.

    * El método `getItems()` recolecta los artículos que los usuarios agregan al carrito y retorna cada artículo con su cantidad asociada.

    * El método `clearCart()` retorna un array vacío de elementos.

{@a product-details-use-cart-service}
### Utilice el servicio de carrito 

Esta sección lo guía a través del uso del servicio de carrito para agregar un producto al carrito con un botón "Comprar".

1. Abra `product-details.component.ts`.

1. Configure el componente para utilizar el servicio de carrito.

    1. Importe el servicio de carrito.

        <code-example header="src/app/product-details/product-details.component.ts" path="getting-started/src/app/product-details/product-details.component.ts" region="cart-service">
        </code-example>

    1. Inyecte el servicio de carrito agregándolo al `constructor()`.

        <code-example path="getting-started/src/app/product-details/product-details.component.ts" header="src/app/product-details/product-details.component.ts" region="inject-cart-service">
        </code-example>

        <!--
        To do: Consider defining "inject" and describing the concept of "dependency injection"
        -->

1. Defina el método `addToCart()`, que agrega el producto actual al carrito.

    El método `addToCart()` hace las siguientes tres cosas:
    * Recibe el `product` actual.
    * Utiliza el método `addToCart()` del servicio de carrito para agregar el producto al carrito.
    * Muestra un mensaje de que ha agregado un producto al carrito.

    <code-example path="getting-started/src/app/product-details/product-details.component.ts" header="src/app/product-details/product-details.component.ts" region="add-to-cart"></code-example>

1. Actualice la plantilla de detalles del producto con un botón "Comprar" que agrega el producto actual al carrito.

    1. Abra `product-details.component.html`.

    1. Agregue un botón con la etiqueta "Comprar" y vincule el evento `click()` al método `addToCart()`:

        <code-example header="src/app/product-details/product-details.component.html" path="getting-started/src/app/product-details/product-details.component.html">
        </code-example>
    
    <div class="alert is-helpful">

    The line, `<h4>{{ product.price | currency }}</h4>` uses the `currency` pipe to transform `product.price` from a number to a currency string. A pipe is a way you can transform data in your HTML template. For more information about Angular pipes, see [Pipes](guide/pipes "Pipes").

    La línea, `<h4>{{product.price | currency}}</h4>` usa el pipe `currency` para transformar `product.price` de un número a un string de moneda. Una pipe es una forma de transformar datos en su plantilla HTML. Para obtener más información sobre las pipes en Angular, consulte [Pipes](guide/pipes "Pipes").

    </div>

1. Para ver el nuevo botón "Comprar", actualice la aplicación y haga clic en el nombre de un producto para mostrar sus detalles.

    <div class="lightbox">
      <img src='generated/images/guide/start/product-details-buy.png' alt="Display details for selected product with a Buy button">
    </div>

 1. Haga clic en el botón "Comprar" para agregar el producto a la lista almacenada de artículos en el carrito y mostrar un mensaje de confirmación.

    <div class="lightbox">
      <img src='generated/images/guide/start/buy-alert.png' alt="Display details for selected product with a Buy button">
    </div>


## Crear la vista del carrito

En este punto, los usuarios pueden poner artículos en el carrito haciendo clic en "Comprar", pero aún no pueden ver su carrito.

Cree la vista del carrito en dos pasos:

1. Cree un componente de carrito y configure el enrutamiento al nuevo componente. En este punto, la vista del carrito solo tiene texto predeterminado.
1. Muestre los artículos del carrito.

### Configurar el componente

 Para crear la vista del carrito, comience siguiendo los mismos pasos que hizo para crear el componente de detalles del producto y configure el enrutamiento para el nuevo componente.

1. Genere un componente de carrito, llamado `cart`.

    Recordatorio: En la lista de archivos, haga clic con el botón derecho en la carpeta `app`, elija `Angular Generator` y `Component`.

    <code-example header="src/app/cart/cart.component.ts" path="getting-started/src/app/cart/cart.component.1.ts"></code-example>

1. Agregue una ruta (un patrón de URL) para el componente del carrito.

    Abra `app.module.ts` y agregue una ruta para el componente `CartComponent`, con un `path` de `cart`:

    <code-example header="src/app/app.module.ts" path="getting-started/src/app/app.module.ts" region="cart-route">
    </code-example>

1. Actualice el botón "Pagar" para que se dirija a la URL `/cart`.

    Abra `top-bar.component.html` y agregue una directiva `routerLink` que apunte a `/cart`.

    <code-example
        header="src/app/top-bar/top-bar.component.html"
        path="getting-started/src/app/top-bar/top-bar.component.html"
        region="cart-route">
    </code-example>

1. Para ver el nuevo componente del carrito, haga clic en el botón "Pagar". Puedes ver el texto predeterminado "cart works!", y la URL tiene el patrón `https://getting-started.stackblitz.io/cart`, donde `getting-started.stackblitz.io` puede ser diferente para su proyecto de StackBlitz.

    <div class="lightbox">
      <img src='generated/images/guide/start/cart-works.png' alt="Display cart view before customizing">
    </div>

### Mostrar los artículos del carrito

Puede utilizar los servicios para compartir datos entre componentes:

* El componente de detalles del producto ya utiliza el servicio de carrito para agregar productos al carrito.
* Esta sección le muestra cómo utilizar el servicio de carrito para mostrar los productos en el carrito.


1. Abra `cart.component.ts`.

1. Configure el componente para utilizar el servicio de carrito.

    1. Importe el `CartService` del archivo `cart.service.ts`.

        <code-example header="src/app/cart/cart.component.ts" path="getting-started/src/app/cart/cart.component.2.ts" region="imports">
        </code-example>

    1. Inyecte el `CartService` para que el componente del carrito pueda usarlo.

        <code-example path="getting-started/src/app/cart/cart.component.2.ts" header="src/app/cart/cart.component.ts" region="inject-cart">
        </code-example>

1. Defina la propiedad `items` para almacenar los productos en el carrito.

    <code-example path="getting-started/src/app/cart/cart.component.2.ts" header="src/app/cart/cart.component.ts" region="items">
    </code-example>

1. Configure los artículos usando el método `getItems()` del servicio de carrito. Recuerde que definió este método [cuando generó `cart.service.ts`](#generate-cart-service)

    La clase resultante `CartComponent` es la siguiente:

    <code-example path="getting-started/src/app/cart/cart.component.3.ts" header="src/app/cart/cart.component.ts" region="props-services">
    </code-example>

1. Actualice la plantilla con un header y use un `<div>` con un `*ngFor` para mostrar cada uno de los artículos del carrito con su nombre y precio.

    La plantilla resultante `CartComponent` es la siguiente:

    <code-example header="src/app/cart/cart.component.html" path="getting-started/src/app/cart/cart.component.2.html" region="prices">
    </code-example>

1. Pruebe su componente de carrito.

    1. Haga clic en "Mi tienda" para ir a la vista de la lista de productos.
    1. Haga clic en el nombre de un producto para mostrar sus detalles.
    1. Haga clic en "Comprar" para agregar el producto al carrito.
    1. Haga clic en "Pagar" para ver el carrito.
    1. Para agregar otro producto, haga clic en "Mi tienda" para volver a la lista de productos.

  Repita para agregar más artículos al carrito.

    <div class="lightbox">
      <img src='generated/images/guide/start/cart-page-full.png' alt="Cart view with products added">
    </div>


<div class="alert is-helpful">

Consejo de StackBlitz: cada vez que se actualiza la vista previa, el carrito se limpia. Si realiza cambios en la aplicación, la página se actualiza, por lo que deberá volver a comprar productos para completar el carrito.

</div>

<div class="alert is-helpful">

Para obtener más información sobre los servicios, consulte [Introducción a los servicios y la inyección de dependencias](guide/architecture-services "Conceptos > Introducción a los servicios y ID").

</div>


## Recuperar precios de envío
<!-- Accessing data with the HTTP client -->

El cliente HTTP de Angular, `HttpClient`, es una forma integrada de obtener datos de API externas y proporcionarlos a su aplicación como una transmisión.

Los servidores a menudo devuelven datos en forma de transmisión.
Las transmisiones son útiles porque facilitan la transformación de los datos devueltos y hace las modificaciónes de la forma en que se solicitan esos datos.
El cliente HTTP de Angular, `HttpClient`, es una forma integrada de obtener datos de API externas y proporcionarlos a su aplicación como una transmisión.

Esta sección le muestra cómo utilizar el cliente HTTP para recuperar los precios de envío de un archivo externo.

### Datos de envío predefinidos

La aplicación que StackBlitz genera para esta guía viene con datos de envío predefinidos en `assets/shipping.json`.
Utilice estos datos para agregar los precios de envío de los artículos en el carrito.

<code-example header="src/assets/shipping.json" path="getting-started/src/assets/shipping.json">
</code-example>


### Utilice `HttpClient` en el `AppModule`

Antes de que pueda usar el cliente HTTP de Angular, debe configurar su aplicación para usar `HttpClientModule`.

El `HttpClientModule` de Angular registra los proveedores que su aplicación necesita para usar una sola instancia del servicio `HttpClient` en toda su aplicación.

1. Abra `app.module.ts`.

  Este archivo contiene importaciones y funciones que están disponibles para toda la aplicación.

1. Importe `HttpClientModule` del paquete `@angular/common/http` en la parte superior del archivo con las otras importaciones. Como hay varias otras importaciones, este fragmento de código las omite por brevedad. Asegúrese de dejar las importaciones existentes en su lugar.

    <code-example header="src/app/app.module.ts" path="getting-started/src/app/app.module.ts" region="http-client-module-import">
    </code-example>

1. Agregue `HttpClientModule` al array de `AppModule` `@NgModule()` `imports` para registrar los proveedores` HttpClient` de Angular globalmente.

    <code-example path="getting-started/src/app/app.module.ts" header="src/app/app.module.ts" region="http-client-module">
    </code-example>

### Utilice `HttpClient` en el servicio de carrito

Ahora que el `AppModule` importa el `HttpClientModule`, el siguiente paso es inyectar el servicio `HttpClient` en su servicio de tal forma que su aplicación pueda obtener datos e interactuar con APIs y recursos externos.


1. Abra `cart.service.ts`.

1. Importe `HttpClient` del paquete `@angular/common/http`.

    <code-example header="src/app/cart.service.ts" path="getting-started/src/app/cart.service.ts" region="import-http">
    </code-example>

1. Inyecte el `HttpClient` en el constructor de `CartService`:

    <code-example path="getting-started/src/app/cart.service.ts" header="src/app/cart.service.ts" region="inject-http">
    </code-example>


### Definir el método `get()`

Multiples componentes pueden aprovechar el mismo servicio.
Más adelante en este tutorial, el componente de envío utiliza el servicio de carrito para recuperar los datos de envío a través de HTTP desde el archivo `shipping.json`.
Primero, defina un método `get()`.

1. Continue trabajando en `cart.service.ts`.

1. Debajo del método `clearCart()`, defina un nuevo método `getShippingPrices()` que use el método `HttpClient` `get()` para recuperar los datos de envío.

    <code-example header="src/app/cart.service.ts" path="getting-started/src/app/cart.service.ts" region="get-shipping"></code-example>


<div class="alert is-helpful">

Para obtener más información sobre el `HttpClient` de Angular, consulte la [Interacción cliente-servidor](guide/http "Interacción del servidor a través de HTTP")

</div>

## Definir la vista de envío

Ahora que su aplicación puede recuperar datos de envío, cree un componente y una plantilla de envío.

1. Genere un nuevo componente llamado `shipping`.

    Recordatorio: En la lista de archivos, haga clic con el botón derecho en la carpeta `app`, elija `Angular Generator` y `Component`.

    <code-example header="src/app/shipping/shipping.component.ts" path="getting-started/src/app/shipping/shipping.component.1.ts"></code-example>

1. En `app.module.ts`, agregue una ruta para el envío. Especifique un `path` para `shipping` y un componente de `ShippingComponent`.

    <code-example header="src/app/app.module.ts" path="getting-started/src/app/app.module.ts" region="shipping-route"></code-example>

    Todavía no hay un enlace al nuevo componente de envío, pero puede ver la plantilla en el panel de vista previa ingresando la URL que especifica su ruta. La URL tiene el patrón: `https://getting-started.stackblitz.io/shipping` donde la parte `getting-started.stackblitz.io` puede ser diferente para su proyecto de StackBlitz.

1. Modifique el componente de envío para que utilice el servicio de carrito para recuperar los datos de envío a través de HTTP desde el archivo `shipping.json`.

    1. Importe el servicio de carrito.

        <code-example header="src/app/shipping/shipping.component.ts" path="getting-started/src/app/shipping/shipping.component.ts" region="imports"></code-example>

    1. Defina una propiedad `shippingCosts`.

        <code-example path="getting-started/src/app/shipping/shipping.component.ts" header="src/app/shipping/shipping.component.ts" region="props"></code-example>

    1. Inyecte el servicio de carrito en el constructor de `ShippingComponent`:

        <code-example path="getting-started/src/app/shipping/shipping.component.ts" header="src/app/shipping/shipping.component.ts" region="inject-cart-service"></code-example>

    1. Establezca la propiedad `shippingCosts` usando el método `getShippingPrices()` del servicio de carrito.

        <code-example path="getting-started/src/app/shipping/shipping.component.ts" header="src/app/shipping/shipping.component.ts" region="ctor"></code-example>

1. Actualice la plantilla del componente de envío para mostrar los tipos de envío y los precios usando el pipe `async`:

    <code-example header="src/app/shipping/shipping.component.html" path="getting-started/src/app/shipping/shipping.component.html"></code-example>

    El pipe `async` retorna el último valor de una transmisión de datos y continúa haciéndolo durante la vida útil de un componente dado. Cuando Angular destruye ese componente, el pipe `async` se detiene automáticamente. Para obtener información detallada sobre el pipe `async`, consulte la [documentación de la API de AsyncPipe](/api/common/AsyncPipe).

1. Agregue un enlace desde la vista del carrito a la vista de envío:

    <code-example header="src/app/cart/cart.component.html" path="getting-started/src/app/cart/cart.component.2.html"></code-example>

1. Pruebe su función de precios de envío:

    Haga clic en el botón "Pagar" para ver el carrito actualizado. Recuerde que cambiar la aplicación hace que se actualice la vista previa, lo que vacía el carrito.

    <div class="lightbox">
      <img src='generated/images/guide/start/cart-empty-with-shipping-prices.png' alt="Cart with link to shipping prices">
    </div>

    Haga clic en el enlace para navegar a los precios de envío.

    <div class="lightbox">
      <img src='generated/images/guide/start/shipping-prices.png' alt="Display shipping prices">
    </div>


## Próximos pasos

¡Felicidades! Tiene una aplicación de tienda en línea con un catálogo de productos y un carrito de compras. También puede buscar y mostrar los precios de envío.

Para continuar explorando Angular, elija cualquiera de las siguientes opciones:
* [Continuar con la sección de "Formularios".](start/start-forms "Pruébelo: formularios para la entrada del usuario") para finalizar la aplicación agregando la vista del carrito de compras y un formulario de pago.
* [Salte a la sección de "Despliegue"](start/start-deployment "Pruébelo: Despliegue") para pasar al desarrollo local o desplegar su aplicación en Firebase o en su propio servidor.
