# Introducción

Hola, y bienvenido al proyecto Nushell.
El objetivo de este proyecto es tomar la filosofía de Unix sobre shells, donde las tuberías (pipes) conectan comandos simples, y llevarla al estilo moderno de desarrollo.
Así, en lugar de ser solo un shell o un lenguaje de programación, Nushell conecta ambos al reunir un lenguaje de programación rico y un shell completo en un solo paquete.

Nu toma inspiración de varios terrenos conocidos: shells tradicionales como Bash, shells basados en objetos como PowerShell, lenguajes gradualmente tipados como TypeScript, programación funcional, programación de sistemas, y más. Pero en lugar de intentar ser un todoterreno, Nu se enfoca en hacer bien algunas cosas específicas:

- Ser un shell flexible y multiplataforma con un estilo moderno.
- Resolver problemas como un lenguaje de programación moderno que trabaja con la estructura de tus datos.
- Ofrecer mensajes de error claros y soporte limpio para IDE.

## Este libro

El libro está dividido en capítulos que, a su vez, están divididos en secciones.
Puedes hacer clic en los encabezados de los capítulos para obtener más información.

- [Installation](installation.md), por supuesto, te ayuda a introducir Nushell en tu sistema.
- [Getting Started](getting_started.md) muestra los conceptos básicos. También explica algunos principios de diseño donde Nushell difiere de shells típicos como Bash.
- [Nu Fundamentals](nu_fundamentals.md) explica los conceptos básicos del lenguaje Nushell.
- [Programming in Nu](programming_in_nu.md) profundiza en las características del lenguaje y muestra varias formas de organizar y estructurar tu código.
- [Nu as a Shell](nu_as_a_shell.md) se centra en las características del shell, especialmente la configuración y el entorno.
- [Coming to Nu](coming_to_nu.md) ofrece un inicio rápido para usuarios provenientes de otros shells o lenguajes.
- [Design Notes](design_notes.md) incluye explicaciones detalladas sobre algunas decisiones de diseño de Nushell.
- [(Not So) Advanced](advanced.md) aborda temas más avanzados (aunque no son tan avanzados, ¡échales un vistazo!).

## The Many Parts of Nushell

El proyecto Nushell consta de múltiples repositorios y subproyectos. Puedes encontrarlos todos en [nuestra organización de GitHub](https://github.com/nushell).

- El repositorio principal de Nushell se encuentra [aquí](https://github.com/nushell/nushell). Está dividido en varias cajas (crates) que pueden utilizarse como bibliotecas independientes en tus propios proyectos, si lo deseas.
- El repositorio de nuestra página [nushell.sh](https://nushell.sh), incluido este libro, está [aquí](https://github.com/nushell/nushell-book).
- Nushell tiene su propio editor de líneas que [tiene su propio repositorio](https://github.com/nushell/reedline)
- [`nu_scripts`](https://github.com/nushell/nu_scripts) es un lugar para compartir scripts y módulos con otros usuarios hasta que tengamos algún tipo de gestor de paquetes.
- [Nana](https://github.com/nushell/nana) es un esfuerzo experimental para explorar interfaces gráficas de usuario para Nushell.
- [Awesome Nu](https://github.com/nushell/awesome-nu) contiene una lista de herramientas que funcionan con el ecosistema de Nushell: extensiones (plugins), scripts, extensiones de editor (editor extension), integraciones de terceros, etc.
- editor extension
- [Nu Showcase](https://github.com/nushell/showcase) es un lugar para compartir trabajos relacionados con Nushell, ya sean blogs, arte o cualquier otra cosa.
- [Request for Comment (RFC)](https://github.com/nushell/rfcs) sirve como un lugar para proponer y discutir cambios de diseño importantes. Aunque actualmente está subutilizado, esperamos usarlo más a medida que nos acerquemos y avancemos más allá de la versión 1.0.

## Contribuir

¡Aceptamos contribuciones!
[As you can see](#the-many-parts-of-nushell), como puedes ver, hay muchos lugares donde puedes contribuir.
La mayoría de los repositorios contienen un archivo `CONTRIBUTING.md` con consejos y detalles para ayudarte a comenzar (si no, considera contribuir creando uno).

Nushell está escrito en [Rust](https://www.rust-lang.org).
Pero no necesitas ser programador de Rust para ayudar.
Si sabes algo de desarrollo web, puedes contribuir a mejorar este sitio web o el proyecto Nana. 
Los [Dataframes](dataframes.md) pueden beneficiarse si tienes experiencia en procesamiento de datos.

Si has escrito un script genial, un plugin o integrado Nushell en algún lugar, agradeceríamos tu contribución a `nu_scripts` o `Awesome Nu`. 
Descubrir errores, documentar los pasos para reproducirlos y reportarlos en GitHub también es una ayuda valiosa.
¡Puedes contribuir a Nushell simplemente usándolo!

Dado que Nushell evoluciona rápido, este libro necesita actualizaciones constantes.
Contribuir a este libro no requiere habilidades especiales más allá de un conocimiento básico de Markdown.
Además, puedes considerar traducir partes del libro a tu idioma.

## Comunidad

El lugar principal para discutir sobre Nushell es nuestro [Discord](https://discord.com/invite/NtAbbGn).
También puedes seguirnos en [Twitter](https://twitter.com/nu_shell) para noticias y actualizaciones.
Finalmente, puedes usar las discusiones en GitHub o reportar problemas en los issues de GitHub.