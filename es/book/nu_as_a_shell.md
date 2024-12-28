# Nu como Shell

Los capítulos de [Fundamentos de Nu](nu_fundamentals.md) y [Programación en Nu](programming_in_nu.md) se centraron principalmente en los aspectos del lenguaje de Nushell.

Este capítulo te dará idea sobre las partes de Nushell relacionadas con su intérprete ([REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) de Nushell).
Algunos conceptos forman parte directamente del lenguaje de programación de Nushell (como las variables de entorno), mientras que otros están implementados únicamente para mejorar la experiencia interactiva (como los hooks), y, por lo tanto, no están presentes al ejecutar un script.

Muchos parámetros de Nushell pueden ser [configurados](configuration.md).
La configuración en sí misma se almacena como una variable de entorno (environment variable).
Además, Nushell tiene varios archivos de configuración que se ejecutan al inicio, donde puedes colocar comandos personalizados, alias, etc.

Una característica destacada de cualquier shell son las [variables de entorno](environment.md).
En Nushell, estas variables tienen un ámbito definido y pueden tener cualquier tipo compatible con Nushell.
Esto introduce consideraciones adicionales de diseño, por lo que se recomienda consultar la sección vinculada para más detalles.

Otras secciones explican cómo trabajar con [stdout, stderr y códigos de salida (exit codes)](stdout_stderr_exit_codes.md), cómo [escapar una llamada de comando a una llamada externa](escaping.md) (escape a command call to the external command call), y cómo [configurar prompts de terceros](3rdpartyprompts.md) para que funcionen con Nushell.

Una característica interesante de Nushell son las [shells](shells_in_shells.md), que te permiten trabajar en múltiples directorios simultáneamente.

Nushell también tiene su propio editor de línea, [Reedline](line_editor.md).
Con la configuración de Nushell, es posible ajustar características de Reedline, como el prompt, las combinaciones de teclas, el historial o los menús.

También es posible definir [firmas (signatures) personalizadas para comandos externos](externs.md), lo que permite crear [autocompletados personalizados](custom_completions.md) para ellos (los autocompletados personalizados también funcionan para los comandos personalizados de Nushell).

La sección [Colores y Temas en Nu](coloring_and_theming.md) detalla cómo configurar la apariencia de Nushell.

Si deseas programar algunos comandos para ejecutarse en segundo plano, la sección de [Tareas en segundo plano en Nu](background_task.md) proporciona una guía simple para seguir.

Finalmente, los [hooks](hooks.md) te permiten insertar fragmentos de código de Nushell para que se ejecuten en ciertos eventos.
