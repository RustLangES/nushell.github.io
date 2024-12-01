# Aliases

Los Aliases en Nushell ofrecen una manera de hacer un reemplazo simple de las llamadas de comando (comandos externos e internos). Esto te permite crear un nombre de atajo para un comando más largo, incluyendo sus argumentos por defecto.

Por ejemplo, vamos a crear un alias llamado `ll` el cual se expandirá a `ls -l`.

```nu
> alias ll = ls -l
```

Ahora podemos llamar este alias:

```nu
> ll
```

Una vez lo hacemos, es como si hubieramos escrito `ls -l`. Esto también nos permite pasar flags o parámetros posicionales. Por ejemplo, también podemos escribir:

```nu
> ll -a
```

Y obtener el equivalente a haber escrito `ls -l -a`.

## Listar todos los Aliases cargados

Tus aliases utilizables pueden verse en `scope aliases` y `help aliases`.

## Persistencia

Para hacer tus aliases persistentes, deben ser añadidos a tu archivo _config.nu_ ejecutando `config nu` para abrir un editor e insertarlos, y luego reiniciando nushell.
Ej. con el anterior `ll` alias, puedes añadir `alias ll = ls -l` en cualquier parte de _config.nu_

```nu
$env.config = {
    # main configuration
}

alias ll = ls -l

# some other config and script loading
```

## Piping en Aliases

Tenga en cuenta que `alias uuidgen = uuidgen | tr A-F a-f` (para hacer que uuidgen en mac se comporte como en linux) no funcionará.
La solución es definir un comando sin parámetros que llame al programa del sistema `uuidgen` a través de `^`.

```nu
def uuidgen [] { ^uuidgen | tr A-F a-f }
```

Ver más en esta [custom commands](custom_commands.md) sección de este libro.

O un ejemplo más idiomático con comandos internos de nushell

```nu
def lsg [] { ls | sort-by type name -i | grid -c | str trim }
```

mostrando todos los archivos y carpetas listados en una cuadrícula.

## Replacing Existing Commands Using Aliases

::: warning Cuidado!
Al sustituir comandos, es mejor hacer primero un "back up" del comando y evitar el error de recursión.
:::

Cómo hacer back up a un comando como `ls`:

```nu
alias core-ls = ls    # This will create a new alias core-ls for ls
```

Ahora puedes usar `core-ls` como `ls` en tu programación-nu. Verás más abajo cómo usar `core-ls`.

La razón por la que necesitas usar un alias es porque, a diferencia de `def`, los aliases son dependientes de la posición. Por lo tanto, necesitas un "back up" del comando antiguo primero con un alias, antes de redefinirlo.
Si no haces un backup del comando y reemplazas el comando usando `def` vas a tener un error de recursión.

```nu
def ls [] { ls }; ls    # Do *NOT* do this! This will throw a recursion error

#output:
#Error: nu::shell::recursion_limit_reached
#
#  × Recursion limit (50) reached
#     ╭─[C:\Users\zolodev\AppData\Roaming\nushell\config.nu:807:1]
# 807 │
# 808 │ def ls [] { ls }; ls
#     ·           ───┬──
#     ·              ╰── This called itself too many times
#     ╰────
```

La manera recomendada para sustituir un comando existente es hacer shadow al comando.
Este es un ejemplo de shadowing al comando `ls`.

```nu
# An escape hatch to have access to the original ls command
alias core-ls = ls

# Call the built-in ls command with a path parameter
def old-ls [path] {
  core-ls $path | sort-by type name -i
}

# Shadow the ls command so that you always have the sort type you want
def ls [path?] {
  if $path == null {
    old-ls .
  } else {
    old-ls $path
  }
}
```
