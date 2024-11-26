# Tareas de Fondo con Nu

Actualmente, Nushell no tiene una función integrada de gestión de tareas en segundo plano, pero puedes hacerlo "soportar" tareas en segundo plano con algunas herramientas, aquí algunos ejemplos:

1. Usando una herramienta de gestión de tareas de terceros, como [pueue](https://github.com/Nukesor/pueue)
2. Usando un multiplexor de terminal, como [tmux](https://github.com/tmux/tmux/wiki) or [zellij](https://zellij.dev/)

## Usando Nu Con Pueue

El módulo toma prestado el poder de [pueue](https://github.com/Nukesor/pueue), es posible programar tareas en segundo plano a pueue, y gestionar esas tareas (como ver logs, matar tareas, o obtener el estado de ejecución de todas las tareas, crear grupos, pausar tareas, etc)

A diferencia de un multiplexor de terminal, no necesitas conectarte a múltiples sesiones de tmux, y obtener el estado de la tarea fácilmente.

Aquí ofrecemos un [nushell module](https://github.com/nushell/nu_scripts/tree/main/modules/background_task) que facilita el trabajo con pueue.

Este es un ejemplo de configuración para hacer que Nushell "soporte" tareas en segundo plano:

1. Instalar pueue
2. Ejecuta `pueued`, puedes consultar en [start-the-daemon page](https://github.com/Nukesor/pueue/wiki/Get-started#start-the-daemon) para más información.
3. Pon el archivo [task.nu](https://github.com/nushell/nu_scripts/blob/main/modules/background_task/task.nu) bajo `$env.NU_LIB_DIRS`.
4. Añade una linea al archivo `$nu.config-path`: `use task.nu`
5. Reinicia Nushell.

Luego vas a tener algunos comandos para programar tareas en segundo plano. (Ej: `task spawn`, `task status`, `task log`)

Cons: Genera un nuevo intérprete de Nushell para ejecutar cada tarea, así que no hereda las variables del scope actual, comandos personalizados, definición de alias.
Solo hereda las variables de entorno cuyo valor puede ser convertido a un string.
Por lo tanto, si quieres usar comandos personalizados o variables, tienes que [`use`](/commands/docs/use.md) o [`def`](/commands/docs/def.md) dentro del bloque dado. 

## Usando Nu con un Multiplexor de Terminal

Puedes elegir e instalar un multiplexor de terminal y utilizarlo.

Permite cambiar fácilmente entre múltiples programas en una terminal, desconectarlos (continúan ejecutándose en segundo plano) y reconectarlos en una terminal diferente. Como resultado, es muy flexible y utilizable.