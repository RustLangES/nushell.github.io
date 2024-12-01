# Fundamentos de Nushell

Este capítulo explica algunos de los fundamentos del lenguaje de programación Nushell.  
Después de revisarlo, deberías tener una idea de cómo escribir programas simples en Nushell.

Nushell tiene un sistema de tipos rico.  
Encontrarás tipos de datos (data types) típicos como cadenas de texto (strings) o enteros (integers), y otros menos comunes, como rutas de celdas (cell paths).  
Además, una de las características principales de Nushell es la noción de _datos estructurados_ (structured data), lo que significa que puedes organizar los tipos en colecciones: listas (lists), registros (records) o tablas (tables).  
A diferencia del enfoque tradicional de Unix, donde los comandos se comunican a través de texto plano, los comandos de Nushell se comunican mediante estos tipos de datos.  
Todo lo anterior se explica en [Tipos de Datos](types_of_data.md).

[Carga de Datos](loading_data.md) explica cómo leer formatos de datos comunes, como JSON, y convertirlos en _datos estructurados_. Esto incluye nuestro propio formato de datos "NUON".

Al igual que los shells de Unix, los comandos de Nushell pueden componerse en [pipelines](pipelines.md) para pasar y modificar un flujo de datos (stream of data).

Algunos tipos de datos tienen características interesantes que merecen sus propias secciones: [cadenas de texto](working_with_strings.md), [listas](working_with_lists.md) y [tablas](working_with_tables.md).  
Además de explicar estas características, estas secciones también muestran cómo realizar operaciones comunes, como componer cadenas de texto o actualizar valores en una lista.

Por último, la [Referencia de Comandos](/commands/) enumera todos los comandos integrados con descripciones breves.  
Ten en cuenta que también puedes acceder a esta información desde Nushell utilizando el comando [`help`](/commands/docs/help.md).