# Programación en Nu

Este capítulo profundiza en los detalles de Nushell como lenguaje de programación.  
Cada característica principal del lenguaje tiene su propia sección.

Al igual que la mayoría de los lenguajes de programación permiten definir funciones, Nushell utiliza [comandos personalizados](custom_commands.md) para este propósito.

Si vienes de otros shells, tal vez estés acostumbrado a usar [alias](aliases.md).
Los alias de Nushell funcionan de manera similar, pero son parte del lenguaje de programación y no solo una característica del shell.

Las operaciones comunes, como la suma o la búsqueda con expresiones regulares (regex), se pueden realizar mediante [operadores](operators.md).
No todas las operaciones son compatibles con todos los tipos de datos, y Nushell se encargará de informarte si hay una incompatibilidad.

Puedes almacenar resultados intermedios en [variables](variables.md).
Las variables pueden ser inmutables, mutables o constantes en tiempo de "parseo" (parse-time constant).

Las últimas tres secciones están dirigidas a la organización de tu código:

[Scripts](scripts.md) son la forma más sencilla de organizar el código: simplemente coloca el código en un archivo y lo cargas.
Además, puedes ejecutar scripts como programas independientes con firmas de línea de comandos usando el comando "especial" `main`.

Con los [módulos](modules.md), al igual que en muchos otros lenguajes de programación, es posible componer tu código a partir de piezas más pequeñas.  
Los módulos te permiten definir una interfaz pública frente a comandos privados, e importar comandos personalizados, alias y variables de entorno desde ellos.


Los [Overlays](overlays.md) se construyen (build) sobre los módulos.
Definir un overlay permite introducir las definiciones del módulo en su propia "capa" (layer) intercambiable que se aplica encima de otros overlays.
Esto habilita características como activar entornos virtuales o sobrescribir conjuntos de comandos predeterminados con variantes personalizadas.

La biblioteca estándar también incluye un [framework de pruebas](testing.md) si deseas comprobar que tu código reutilizable funcione perfectamente.