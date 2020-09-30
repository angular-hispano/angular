# Recopilación de métricas de uso
Puede ayudar al equipo de Angular a priorizar funciones y mejoras al permitir que Angular
equipo para enviar estadísticas de uso de comandos de línea de comandos a Google. El equipo de Angular no recolecta
estadísticas de uso a menos que opte explícitamente durante la instalación o actualización de Angular CLI.

## ¿Qué se recolecta?
El análisis de uso incluye los comandos y las banderas seleccionadas para cada ejecución. El análisis de uso puede
incluye la siguiente información:

- Su sistema operativo (Mac, distribución de Linux, Windows) y su versión.
- Número de CPU, cantidad de RAM.
- Versión CLI de nodo y angular (solo versión local).
- Cuánto tiempo tardó cada comando en inicializarse y ejecutarse.
- Nombre del comando que se ejecutó.
- Para los comandos de Schematics (agregar, generar, nuevo y actualizar), una lista de banderas en la lista blanca.
- Para los comandos de compilación (compilar, servir), la cantidad y el tamaño de los paquetes (inicial y diferido),
  unidades de compilación, el tiempo que tomó compilar y reconstruir, y las unidades básicas específicas de Angular
  Uso de API.
- Código de error de excepciones y datos de fallos. No se recopila ningún rastro de pila.

Solo se informan los esquemas y constructores desarrollados y de propiedad de Angular. Esquemas de terceros y
Los constructores no envían datos al equipo de Angular.

## Participar
Al instalar Angular CLI o actualizar una versión existente, se le solicita que permita global
recopilación de estadísticas de uso. Si dice que no u omite la indicación, no se recopilan datos.

Comenzando con la versión 8, agregamos el comando `analytics` a la CLI. Puede cambiar su suscripción
decisión en cualquier momento utilizando este comando.

### Desactivación de análisis de uso
Para deshabilitar la recopilación de análisis, ejecute el siguiente comando:

```bash
# Disable all usage analytics.
ng analytics off
```

### Habilitación de análisis de uso
Para habilitar el análisis de uso, ejecute el siguiente comando:

```bash
# Enable all usage analytics.
ng analytics on
```

### Solicitud
Para volver a preguntar al usuario sobre el análisis de uso, ejecute el siguiente comando:

```bash
# Prompt for all usage analytics.
ng analytics prompt
```
