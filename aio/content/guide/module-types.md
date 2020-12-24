# Directrices para crear NgModules

En este tema se proporciona una visión general conceptual de las diferentes categorías de [NgModules](guide/glossary-ngmodule "Definición de NgModule") que puede crear para organizar el código en una estructura modular.
Estas categorías no están grabadas en piedra.
Es posible que desee crear NgModules para otros fines o combinar las características de algunas de estas categorías.

NgModules son una excelente manera de organizar una aplicación y mantener el código relacionado con una funcionalidad o característica específica separada de otro código.
Utilice NgModules para consolidar [componentes](guide/glossary#component "Definición de componente"), [directivas](guide/glossary#directive "Definición de directiva") y [pipes](guide/glossary#pipe "Definición de pipes)") en bloques cohesivos de funcionalidad.

Enfoque cada bloque en una característica o dominio de negocio, un flujo de trabajo o flujo de navegación, una colección común de utilidades, o uno o más [proveedores](guide/glossary#provider "Definición del proveedor") para [servicios](guide/glossary#service "Definición de servicio").

Para obtener más información sobre NgModules, vea [Organización de la aplicación con NgModules](guide/ngmodules "Organización de la aplicación con NgModules").

<div class="alert is-helpful">

Para ver la aplicación de ejemplo utilizada en temas relacionados con NgModules, consulte el <live-example name="ngmodules"></live-example>.

</div>

## Resumen de las categorías de NgModule

Todas las aplicaciones comienzan por [bootstrapping el root NgModule](guide/bootstrapping "Lanzamiento de una aplicación con un NgModule raíz").
Puede organizar sus otros NgModules de la manera que desee.

En este tema se proporcionan algunas directrices para las siguientes categorías generales de NgModules:

* [Domain](#domain): Un dominio NgModule se organiza en torno a una característica, dominio empresarial o experiencia de usuario.
* [Enruteado](#Enruteado): El componente superior del NgModule actúa como el destino de una ruta de navegación [Enrutador](guide/glossary-router "Definition of router").
* [Enrutamiento](#enrutamiento): Un NgModule de enrutamiento proporciona la configuración de enrutamiento para otro NgModule.
* [Servicio](#servicio): Un servicio NgModule proporciona servicios de utilidad como acceso a datos y mensajería.
* [Widget](#widget): Un widget NgModule hace que un componente, directiva o un pipe estén disponibles para otros NgModules.
* [Compartido](#compartidio): Un NgModule compartido pone un conjunto de componentes, directivas y canalizaciones disponibles para otros NgModules.

En la tabla siguiente se resumen las características clave de cada categoría.

<table>
 <tr>
   <th style="vertical-align: top">
     NgModule
   </th>

   <th style="vertical-align: top">
     Declaraciones
   </th>

   <th style="vertical-align: top">
     Proveedores
   </th>

   <th style="vertical-align: top">
     Exportaciones
   </th>

   <th style="vertical-align: top">
     Importado por
   </th>
 </tr>

 <tr>
   <td>Dominio</td>
   <td>Si</td>
   <td>Rare</td>
   <td>Top component</td>
   <td>Another domain, AppModule</td>
 </tr>

 <tr>
   <td>Enrutado</td>
   <td>Si</td>
   <td>Raro</td>
   <td>No</td>
   <td>Ninguno</td>
 </tr>

 <tr>
   <td>Enrutamiento</td>
   <td>No</td>
   <td>Si (Guardias)</td>
   <td>RouterModule</td>
   <td>Otro dominio (para enrutamiento)</td>
 </tr>

 <tr>
   <td>Servicios</td>
   <td>No</td>
   <td>Si</td>
   <td>No</td>
   <td>AppModule</td>
 </tr>

 <tr>
   <td>Widget</td>
   <td>Si</td>
   <td>Raro</td>
   <td>Si</td>
   <td>Otro dominio</td>
 </tr>

 <tr>
   <td>Compartido</td>
   <td>Si</td>
   <td>No</td>
   <td>Si</td>
   <td>Otro dominio</td>
 </tr>
</table>

{@a domain}

## Dominio NgModules

Utilice un NgModule de dominio para ofrecer una experiencia de usuario dedicada a una función o dominio de aplicación en particular, como editar un cliente o realizar un pedido.
Un ejemplo es `ContactModule` en el <live-example name="ngmodules"></live-example>.

Un NgModule de dominio organiza el código relacionado con una función determinada, que contiene todos los componentes, el enrutamiento y las plantillas que componen la función.
Su componente principal en el dominio NgModule actúa como la característica o la raíz del dominio, y es el único componente que exporta.
De él descienden subcomponentes de apoyo privados.

Importe un NgModule de dominio exactamente una vez a otro NgModule, como un NgModule de dominio, o al NgModule raíz (ʻAppModule`) de una aplicación que contenga solo unos pocos NgModule.

Los NgModules de dominio consisten principalmente en declaraciones.
Rara vez incluye proveedores.
Si lo hace, la vida útil de los servicios proporcionados debe ser la misma que la vida útil del NgModule.

<div class="alert is-helpful">

Para obtener más información sobre los ciclos de vida, consulte [Conexión en el ciclo de vida del componente](guía/lifecycle-hooks "Conexión en el ciclo de vida del componente").

</div>

{@a routed}

## NgModules Enrutados

Utilice un NgModule enrutado para todos los [NgModules con carga diferida](guide/lazy-loading-ngmodules "Carga diferida de un NgModule").
Utilice el componente superior del NgModule como destino de una ruta de navegación del enrutador.
Los NgModules enrutados no exportan nada porque sus componentes nunca aparecen en la plantilla de un componente externo.
No importe un NgModule enrutado con carga diferida a otro NgModule, ya que esto desencadenaría una carga ansiosa, anulando el propósito de la carga diferida.

Los NgModules enrutados rara vez tienen proveedores porque carga un NgModule enrutado solo cuando es necesario (como para el enrutamiento).
Los servicios enumerados en el arreglo "proveedor" de NgModules no estarían disponibles porque el inyector raíz no conocería el NgModule cargado de forma diferida.
Si incluye proveedores, la vida útil de los servicios prestados debe ser la misma que la vida útil del NgModule.
No proporcione en toda la aplicación [servicios singleton](guía/servicios singleton) en un NgModule enrutado o en un NgModule que importa el NgModule enrutado.

<div class="alert is-helpful">

Para obtener más información sobre proveedores y NgModules enrutados con carga diferida, consulte [Limitar el alcance del proveedor](guide/providers#limiting-provider-scope-by-lazy-loading-modules "Proporcionar dependencias: limitar el alcance del proveedor").

</div>

{@a routing}

## NgModules Enrutados

Utilice un NgModule de enrutamiento para proporcionar la configuración de enrutamiento para un NgModule de dominio, separando así las preocupaciones de enrutamiento de su dominio complementario NgModule.
Un ejemplo es `ContactRoutingModule` en <live-example name =" ngmodules "> </live-example>, que proporciona el enrutamiento para su dominio complementario NgModule` ContactModule`.

<div class="alert is-helpful">

Para obtener una descripción general y detalles sobre el enrutamiento, consulte [Navegación en la aplicación: enrutamiento a vistas](guide/router "Navegación en la aplicación: enrutamiento a vistas").

</div>

Utilice un NgModule de enrutamiento para realizar las siguientes tareas:

* Definir rutas.
* Agregue la configuración del enrutador a la importación de NgModule.
* Agregue proveedores de servicios de guardia y resolución a los proveedores de NgModule.

El nombre del NgModule de enrutamiento debe ser paralelo al nombre de su NgModule complementario, utilizando el sufijo "Enrutamiento".
Por ejemplo, <code> ContactModule </code> en <code> contact.module.ts </code> tiene un NgModule de enrutamiento llamado <code> ContactRoutingModule </code> en <code> contact-routing.module.ts </ código>.

Importe un NgModule de enrutamiento solo en su NgModule complementario.
Si el NgModule complementario es el <code> AppModule </code> raíz, el <code> AppRoutingModule </code> agrega la configuración del enrutador a sus importaciones con <code> RouterModule.forRoot (rutas) </code>.
Todos los demás NgModules de enrutamiento son elementos secundarios que importan <code> RouterModule.forChild (rutas) </code>.

En su NgModule de enrutamiento, vuelva a exportar el <code> RouterModule </code> como una conveniencia para que los componentes del NgModule complementario tengan acceso a las directivas del enrutador como <code> RouterLink </code> y <code> RouterOutlet </ code >.

No use declaraciones en un NgModule de enrutamiento.
Los componentes, directivas y canalizaciones son responsabilidad del dominio complementario NgModule, no del enrutamiento NgModule.

{@a service}

## Servicios NgModules

Utilice un NgModule de servicio para proporcionar un servicio de utilidad, como acceso a datos o mensajería.
Los NgModules de servicio ideal están compuestos completamente por proveedores y no tienen declaraciones.
`HttpClientModule` de Angular es un buen ejemplo de un servicio NgModule.

Utilice sólo la raíz `AppModule` para importar el servicio NgModules.

{@a widget}

## Widget NgModules

Utiliza un widget NgModule para hacer que un componente, directiva o canalización esté disponible para NgModules externos.
Importa widgets NgModules en cualquier NgModules que necesites los widgets en sus plantillas.
Muchas bibliotecas de componentes de IU de terceros se proporcionan como widgets NgModules.

Un widget NgModule debe constar completamente de declaraciones, la mayoría de ellas exportadas.
Rara vez tendría proveedores.

{@a shared}

## NgModules Compartidos

Coloca directivas, canalizaciones y componentes de uso común en un NgModule, generalmente llamado `SharedModule`, y luego importa solo ese NgModule donde lo necesite en otras partes de su aplicación.
Puedes importar el NgModule compartido en su dominio NgModules, incluido [lazy-loaded NgModules](guide/lazy-loading-ngmodules "Lazy-loading an NgModule").
Un ejemplo es `SharedModule` en el <live-example name="ngmodules"></live-example>, donde proporciona el `AwesomePipe` pipe personalizada y `HighlightDirective` directiva.

Los NgModules compartidos no deben incluir proveedores, ni ninguno de sus NgModules importados o reexportados debe incluir proveedores.

Para aprender a usar módulos compartidos para organizar y optimizar su código, consulte [Compartir NgModules en una aplicación](guide/sharing-ngmodules "Compartir NgModules en una aplicación").

## Sigueintes pasos

También te puede interesar lo siguiente:

* Para obtener más información sobre NgModules, consulte [Organizar su aplicación con NgModules] (guía / ngmodules "Organizar su aplicación con NgModules").
* Para obtener más información sobre el NgModule raíz, consulte [Lanzamiento de una aplicación con un NgModule raíz] (guía / bootstrapping "Lanzamiento de una aplicación con un NgModule raíz").
* Para obtener más información sobre los NgModules angulares de uso frecuente y cómo importarlos a su aplicación, consulte [Módulos de uso frecuente] (guía/módulos de ng frecuentes "Módulos de uso frecuente").
* Para obtener una descripción completa de las propiedades de los metadatos de NgModule, consulte [Uso de los metadatos de NgModule] (guía / ngmodule-api "Uso de los metadatos de NgModule").

Si desea administrar la carga de NgModule y el uso de dependencias y servicios, consulte lo siguiente:

* Para obtener más información sobre cómo cargar NgModules con entusiasmo cuando se inicia la aplicación, o cómo cargar NgModules de forma perezosa de forma asíncrona por el enrutador, consulte [Lazy-loading feature modules](guide/lazy-loading-ngmodules).
* To comprender cómo proporcionar un servicio u otra dependencia para su aplicación, consulte [Proporcionar dependencias para un NgModule](guide/providers "Proporcionar dependencias para un NgModule").
* Para aprender a crear un servicio singleton para usar en NgModules, consulte [Making a service a singleton](guide/singleton-services "Haciendo un servicio un singleton").