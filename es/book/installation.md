# Instalando Nu

Existen muchas maneras de instalar y ejecutar Nu. Puedes descargar binarios precompilados (pre-built binaries) desde nuestra [página de releases](https://github.com/nushell/nushell/releases), [usar tu gestor de paquetes favorito (package manager)](https://repology.org/project/nushell/versions) o compilar desde el código fuente.

El binario principal de Nushell se llama `nu` (o `nu.exe` en Windows). Después de la instalación, puedes iniciarlo escribiendo `nu`.

@[code](@snippets/installation/run_nu.sh)

[[toc]]

## Binarios Precompilados (Pre-built Binaries)

Los binarios de Nu están publicados para Linux, macOS y Windows [en cada release de GitHub](https://github.com/nushell/nushell/releases). Simplemente descárgalos, extrae los binarios y cópialos a una ubicación que esté en tu PATH.

## Gestores/Administradores de Paquetes (Package Managers)

Nu está disponible a través de varios gestores de paquetes:

[![Packaging status](https://repology.org/badge/vertical-allrepos/nushell.svg)](https://repology.org/project/nushell/versions)

Para macOS y Linux, [Homebrew](https://brew.sh/) es una opción popular (`brew install nushell`).

Para Windows:

- [Winget](https://docs.microsoft.com/en-us/windows/package-manager/winget/) (`winget install nushell`)
- [Chocolatey](https://chocolatey.org/) (`choco install nushell`)
- [Scoop](https://scoop.sh/) (`scoop install nu`)

Para instalación multiplataforma (Cross Platform):

- [npm](https://www.npmjs.com/) (`npm install -g nushell` Nota: los plugins de Nu no están incluidos si instalas de esta forma.)

## Imágenes de Contenedores Docker (Docker Container Images)

Las imágenes Docker están disponibles en el Registro de Contenedores de GitHub (GitHub Container Registry). Hay imágenes para las versiones más recientes (latest release) de Alpine y Debian.
Puedes ejecutar la imagen en modo interactivo usando:

```nu
docker run -it --rm ghcr.io/nushell/nushell:<version>-<distro>
```

Donde `<version>` es la versión de Nushell que quieres ejecutar y `<distro>` es `alpine` o la última versión soportada de Debian, como `bookworm`.

Para ejecutar un comando específico:

```nu
docker run --rm ghcr.io/nushell/nushell:latest-alpine -c "ls /usr/bin | where size > 10KiB"
```

Para ejecutar un script desde el directorio actual usando Bash:

```nu
docker run --rm \
    -v $(pwd):/work \
    ghcr.io/nushell/nushell:latest-alpine \
    "/work/script.nu"
```

## Compilación desde el Código Fuente (Build from Source)

También puedes compilar Nu desde el código fuente. Primero necesitas configurar el entorno de herramientas de Rust y sus dependencias (dependencies).

### Instalación de un Conjunto de Compiladores (Compiler Suite)

Para que Rust funcione correctamente, necesitarás un conjunto de compiladores compatible. Estas son las opciones recomendadas:

- Linux: GCC o Clang
- macOS: Clang (instala Xcode)
- Windows: MSVC (instala [Visual Studio](https://visualstudio.microsoft.com/vs/community/) or the [Visual Studio Build Tools](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022))
   - Asegúrate de instalar la carga de trabajo  "Desarrollo de escritorio con C++" (Desktop development with C++ workload).
  - Cualquier edición de Visual Studio funcionará (la edición Community es gratuita)

### Instalación de Rust

Si no tienes Rust instalado, la mejor manera es a través de [rustup](https://rustup.rs/). Rustup es una forma de gestionar instalaciones de Rust, incluyendo la gestión del uso de diferentes versiones de Rust.

Nu requiere la **versión estable más reciente (1.66.1 o posterior)** de Rust. La mejor manera es dejar que `rustup` encuentre la versión correcta para usted.La primera vez que abras `rustup` te preguntará qué versión de Rust deseas instalar:

@[code](@snippets/installation/rustup_choose_rust_version.sh)

Cuando esté listo, pulse 1 y, a continuación, enter.

Si prefieres no instalar Rust a través de `rustup`, también puedes instalarlo a través de otros métodos (por ejemplo, desde un paquete (package) en una distribución Linux). Sólo asegúrate de instalar una versión de Rust que sea 1.66.1 o posterior.

### Dependencias (Dependencies)

#### Debian/Ubuntu

Necesitará instalar los paquetes "pkg-config", "build-essential" y "libssl-dev":

@[code](@snippets/installation/install_pkg_config_libssl_dev.sh)

#### Distribuciones basadas en RHEL

Necesitarás instalar "libxcb", "openssl-devel" y "libX11-devel":

@[code](@snippets/installation/install_rhel_dependencies.sh)

#### macOS

Usando [Homebrew](https://brew.sh/), instala `openssl` y `cmake`:

@[code](@snippets/installation/macos_deps.sh)

### Compilación (Build) desde [crates.io](https://crates.io) usando Cargo

Los lanzamientos (releases) de Nushell están publicados (published) en el registro de paquetes Rust (Rust package registry) [crates.io](https://crates.io/). Esto facilita la creación e instalación de la última versión de Nu con `cargo`:

```nu
cargo install nu
```

La herramienta `cargo` realizará el trabajo de descargar Nu y sus dependencias de origen, compilarlo e instalarlo en la ruta del contenedor de carga.

Nota: los plugins predeterminados deben instalarse por separado al usar `cargo`. Consulta la sección [Plugins Installation](./plugins.html#core-plugins) del libro para más detalles.

### Construcción desde el repositorio de GitHub (Building from the GitHub repository)

También puedes compilar Nu desde el código fuente más reciente en GitHub. Esto te da acceso inmediato a las últimas características (features) y correcciones (bug fixes). Primero, clona el repositorio:

@[code](@snippets/installation/git_clone_nu.sh)

Luego, compila (build) y ejecuta (run) Nu:

@[code](@snippets/installation/build_nu_from_source.sh)

También puedes compilar y ejecutar Nu en modo de lanzamiento, lo que permite más optimizaciones:

@[code](@snippets/installation/build_nu_from_source_release.sh)

Las personas familiarizadas con Rust pueden preguntarse por qué hacemos un paso de "compilación" (build) y otro de "ejecución" (run) si "ejecutar" hace una compilación por defecto. Esto es para evitar una deficiencia de la nueva opción `default-run` en Cargo, y asegurar que todos los plugins se construyen, aunque esto puede no ser necesario en el futuro.