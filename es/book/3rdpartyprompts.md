# Cómo Configurar Prompts de Terceros

## Nerd Fonts

No son requeridas las fuentes de Nerd Fonts, pero pueden mejorar la presentación del prompt mediante glifos adicionales e iconografía.


> Nerd Fonts parchea (patches) fuentes orientadas a desarrolladores con una gran cantidad de glifos (iconos).
> Específicamente, agrega un gran número de glifos (glypsh) extra de fuentes icónicas populares como Font Awesome, Devicons, Octicons, entre otras.

* [Nerd Fonts website](https://www.nerdfonts.com)
* [Repositorio](https://github.com/ryanoasis/nerd-fonts)

## oh-my-posh

[Sitio](https://ohmyposh.dev/)

[Repositorio](https://github.com/JanDeDobbeleer/oh-my-posh)

Si te gusta [oh-my-posh](https://ohmyposh.dev/), puedes usarlo con Nushell en unos pocos pasos. Funciona muy bien con Nushell. Cómo configurar oh-my-posh con Nushell:

1. Instala Oh My Posh y descarga los temas de oh-my-posh siguiendo la [guía](https://ohmyposh.dev/docs/installation/linux). 
2. Descarga e instala [Nerd Font](https://github.com/ryanoasis/nerd-fonts). 
3. Genera el archivo `.oh-my-posh.nu`. Por defecto, se generará en tu directorio de inicio. Puedes usar `--config` para especificar un tema; de lo contrario, oh-my-posh incluye un tema predeterminado. 
4. Inicializa el prompt de oh-my-posh agregando la instrucción `~/.oh-my-posh.nu` en `~/.config/nushell/config.nu` (o la ruta mostrada por `$nu.config-path`).

```nu
# Genera el archivo .oh-my-posh.nu
> oh-my-posh init nu --config ~/.poshthemes/M365Princess.omp.json

# Inicializa oh-my-posh.nu al iniciar el shell agregando esta línea a tu archivo config.nu
> source ~/.oh-my-posh.nu
```

Para usuarios de MacOS:

1. Puedes instalar oh-my-posh con `brew`, siguiendo la siguiente [guía](https://ohmyposh.dev/docs/installation/macos).
2. Descarga e instala [Nerd Font](https://github.com/ryanoasis/nerd-fonts).
3. Configura el `PROMPT_COMMAND` en el archivo indicado por `$nu.config-path`. Aquí tienes un fragmento de código:

```nu
let posh_dir = (brew --prefix oh-my-posh | str trim)
let posh_theme = $'($posh_dir)/share/oh-my-posh/themes/'
# Cambia el nombre del tema a zash/space/robbyrussel/powerline/powerlevel10k_lean/
# material/half-life/lambda o temas de doble línea: amro/pure/spaceship, etc.
# Para más [temas de ejemplo](https://ohmyposh.dev/docs/themes)
$env.PROMPT_COMMAND = { || oh-my-posh prompt print primary --config $'($posh_theme)/zash.omp.json' }
# Opcional
$env.PROMPT_INDICATOR = $"(ansi y)$> (ansi reset)"
```

## Starship

[Sitio](https://starship.rs/)

[Repositorio](https://github.com/starship/starship)

1. Sigue los enlaces anteriores e instala Starship.
2. Instala Nerd Fonts según tus preferencias.  
3. Usa el ejemplo de configuración siguiente. Asegúrate de configurar la variable de entorno `STARSHIP_SHELL`.

::: tip
Una forma alternativa de habilitar Starship está descrita en las instrucciones de [Starship Quick Install](https://starship.rs/#nushell).

El enlace anterior es la integración oficial de Starship con Nushell y es la forma más sencilla de hacer funcionar Starship sin configuraciones manuales:

- Starship creará su propio script de configuración/entorno. (configuration/environment)
- Solo necesitas incluirlo en `env.nu` y usarlo en `config.nu`.
  :::

Ejemplo de configuración para Starship:

```nu
$env.STARSHIP_SHELL = "nu"

def create_left_prompt [] {
    starship prompt --cmd-duration $env.CMD_DURATION_MS $'--status=($env.LAST_EXIT_CODE)'
}

# Use nushell functions to define your right and left prompt
$env.PROMPT_COMMAND = { || create_left_prompt }
$env.PROMPT_COMMAND_RIGHT = ""

# Los indicadores (PROMPT_INDICATOR) son variables de entorno que representan
# el estado del prompt
$env.PROMPT_INDICATOR = ""
$env.PROMPT_INDICATOR_VI_INSERT = ": "
$env.PROMPT_INDICATOR_VI_NORMAL = "〉"
$env.PROMPT_MULTILINE_INDICATOR = "::: "
```

Ahora reinicia Nu.

```
nushell on 📙 main is 📦 v0.60.0 via 🦀 v1.59.0
❯
```

## Purs

[Repositorio](https://github.com/xcambar/purs)
