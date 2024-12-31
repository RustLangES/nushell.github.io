# Cómo se ejecuta el código Nushell

En [Pensando en Nu](./thinking_in_nu.md#think-of-nushell-as-a-compiled-language), te animamos a _"pensar en Nushell como un lenguaje compilado"_ debido a cómo se procesa el código de Nushell. También cubrimos varios ejemplos de código que no funcionan en Nushell debido a ese proceso.

La razón subyacente es una estricta separación de las etapas de **_análisis y evaluación (parsing and evaluation)_**, lo que **_impide la funcionalidad tipo `eval`_**. En esta sección, explicaremos en detalle qué significa esto, por qué lo hacemos y cuáles son las implicaciones. La explicación busca ser lo más sencilla posible, pero podría ayudarte haber programado en otro lenguaje antes.

[[toc]]

## Lenguajes interpretados vs compilados

### Lenguajes interpretados

Nushell, Python y Bash (entre muchos otros) son lenguajes _"interpretados"_.

Comencemos con un simple programa de Nushell que muestra "Hello, World!" (¡Hola, Mundo!):

```nu
# hello.nu

print "Hello, World!"
```

Por supuesto, esto se ejecuta como se espera usando `nu hello.nu`. Un programa similar escrito en Python o Bash se vería (y se comportaría) casi igual.

En lenguajes _"interpretados"_ el código generalmente se maneja de la siguiente manera:

```
Código fuente (Source Code) → Intérprete (Interpreter) → Resultado (Result)
```

Nushell sigue este patrón, y su "intérprete" se divide en dos partes:

1. `Código fuente → Analizador (Parser) → Representación Intermedia (Intermediate Representations - IR)`
2. `IR → Motor de evaluación (Evaluation Engine) → Resultado`

Primero, el código fuente es analizado por el Parser y convertido en una representación intermedia (IR), que en el caso de Nushell es simplemente una colección de estructuras de datos. Luego, estas estructuras se pasan al motor (Engine) para su evaluación y salida de resultados.

Esto, también, es común en los lenguajes interpretados. Por ejemplo, el código fuente de Python generalmente se [convierte en bytecode](https://github.com/python/cpython/blob/main/InternalDocs/interpreter.md) antes de la evaluación.

### Lenguajes compilados

Por otro lado,  están los lenguajes típicamente «compilados», como C, C++ o Rust. Por ejemplo, aquí hay un simple _"Hello, World!"_ en Rust:

```rust
// main.rs

fn main() {
    println!("Hello, World!");
}
```

Para "ejecutar" este código, debe ser:

1. Compilado en [instrucciones de código máquina](https://en.wikipedia.org/wiki/Machine_code)
2. Los resultados de la compilación almacenados como un archivo binario en el disco

Los primeros dos pasos se manejan con `rustc main.rs`.

3. Luego, para producir un resultado, necesitas ejecutar el binario (`./main`), que pasa las instrucciones a la CPU.

Entonces:

1. `Código fuente ⇒ Compilador ⇒ Código máquina` (Source Code ⇒ Compiler ⇒ Machine Code)
2. `Código máquina ⇒ CPU ⇒ Resultado` (Machine Code ⇒ CPU ⇒ Result)

::: important
Puedes ver que la secuencia compilar-ejecutar (compile-run) no es muy diferente de la secuencia analizar-evaluar (parse-evaluate) de un intérprete. Comienzas con código fuente, lo analizas "parseas" (o compilas) en algún estado (p. ej., bytecode, IR, código máquina), luego evalúas (o ejecutas) el IR para obtener un resultado. Podrías pensar en el código máquina como otro tipo de IR y en la CPU como su intérprete.

Una gran diferencia, sin embargo, entre los lenguajes interpretados y compilados es que los lenguajes interpretados generalmente implementan una función _`eval`_, mientras que los lenguajes compilados no. ¿Qué significa esto?
:::

## Lenguajes dinámicos vs estáticos

::: tip Terminología
En general, la diferencia entre un lenguaje dinámico y uno estático es cuánta parte del código fuente se resuelve durante la compilación (o análisis -parsing-) vs. la evaluación/ejecución (Evaluation/Runtime):

- Los lenguajes _"estáticos"_ realizan más análisis de código (p. ej., verificación de tipos (type-checking), [propiedad de los datos (data ownership)](https://doc.rust-lang.org/stable/book/ch04-00-understanding-ownership.html)) durante la compilación/análisis (Compilation/Parsing).

- Los lenguajes _"dinámicos"_ realizan más análisis de código, incluyendo `eval` de código adicional, durante la evaluación/ejecución (Evaluation/Runtime).

Para los propósitos de esta discusión, la principal diferencia entre un lenguaje estático y dinámico es si tiene o no una función `eval`.

:::

### Función Eval

La mayoría de los lenguajes dinámicos e interpretados tienen una función `eval`. Por ejemplo, [Python `eval`](https://docs.python.org/3/library/functions.html#eval) (también, [Python `exec`](https://docs.python.org/3/library/functions.html#exec)) o [Bash `eval`](https://linux.die.net/man/1/bash).

El argumento de un `eval` es _«código fuente dentro de código fuente»_, normalmente ejecutado de forma condicional o dinámica. Esto significa que, cuando un lenguaje interpretado encuentra un `eval` en el código fuente durante Parse/Eval (analizar/evaluar), normalmente interrumpe el proceso normal de Evaluación para iniciar un nuevo Parse/Eval en el argumento del código fuente del `eval`.

Aquí hay un simple ejemplo de Python `eval` para demostrar este concepto (¡potencialmente confuso!):

```python:line-numbers
# hello_eval.py

print("Hello, World!")
eval("print('Hello, Eval!')")
```

Cuando ejecutas el archivo (`python hello_eval.py`), verás dos mensajes: _"Hello, World!"_ y _"Hello, Eval!_. Aquí está lo que sucede:

1. Todo el programa es analizado (Parsed).
2. (Línea 3) Se evalúa `print("¡Hola, Mundo!")`.
3. (Línea 4) Para evaluar `eval("print('Hello, Eval!')")`:
   1. `print('Hello, Eval!')` se analiza (Parsed).
   2. `print('Hello, Eval!')` se evalúa. (Evaluated)

::: tip Más diversión
Considera `eval("eval(\"print('Hello, Eval!')\")")` ¡y así sucesivamente!
:::

Observe cómo el uso de `eval` aquí añade un nuevo paso «meta» en el proceso de ejecución. En lugar de un único Parse/Eval, `eval` crea pasos Parse/Eval «recursivos» adicionales. Esto significa que el bytecode producido por el intérprete de Python puede ser modificado durante la evaluación.

Nushell no lo permite.

Como se mencionó anteriormente, sin una función `eval` para modificar el bytecode durante el proceso de interpretación, hay muy poca diferencia (a un alto nivel) entre el proceso Parse/Eval de un lenguaje interpretado y el Compile/Run en lenguajes compilados como C++ y Rust.

::: tip Idea clave
Por eso recomendamos que _"pienses en Nushell como un lenguaje compilado"_. A pesar de ser un lenguaje interpretado, su falta de `eval` le da algunas de las ventajas características, así como limitaciones, comunes en los lenguajes compilados estáticos tradicionales.
:::

Profundizaremos en su significado en la próxima sección.

## Implicaciones

Considera este ejemplo en Python:

```python:line-numbers
exec("def hello(): print('Hello eval!')")
hello()
```

::: note
En este ejemplo utilizamos `exec` en lugar de `eval` porque puede ejecutar cualquier código válido de Python, mientras que `eval` está limitado a expresiones. Sin embargo, el principio es similar en ambos casos.
:::

Durante la interpretación (interpretation):

1. Todo el programa es Analizado. (Parsed)
2. Para Evaluar (Evaluate) la Línea 1:
   1. `def hello(): print('Hello eval!')` es Analizado (Parsed)
   2. `def hello(): print('Hello eval!')` es Evaluado (Evaluated)
3. (Línea 2) `hello()` es Evaluado (Evaluated).

Observa que, hasta el paso 2.2, el intérprete no sabe que existe una función llamada `hello`. Esto hace que el [análisis estático](https://es.wikipedia.org/wiki/An%C3%A1lisis_est%C3%A1tico_de_programas) en lenguajes dinámicos sea un desafío. En este ejemplo, la existencia de la función `hello` no puede verificarse simplemente analizando (o compilando) el código fuente. El intérprete debe evaluar (ejecutar) el código para descubrirla.

- En un lenguaje estático y compilado, la ausencia de una función se detecta de manera garantizada en tiempo de compilación (compile-time).
- Sin enbargo, en un lenguaje dinámico e interpretado, esto se convierte en un _posible_ error en tiempo de ejecución (runtime). Si la función definida mediante `eval` se llama condicionalmente, el error podría no descubrirse hasta que esa condición se cumpla en producción.

::: important
En Nushell, hay **exactamente dos pasos**:

1. Analizar todo el código fuente. (Parse)
2. Evaluar todo el código fuente. (Evaluate)

Esta es la secuencia completa de Análisis/Evaluación (Parse/Eval).
:::

::: tip Consejo
Al no permitir funcionalidades similares a `eval`, Nushell previene estos tipos de errores relacionados con `eval`. Llamar a una definición inexistente se detecta de manera garantizada en tiempo de análisis (parse-time) en Nushell.

Además, una vez completado el análisis sintáctico (parsing), podemos estar seguros de que el código de bytes (IR) no cambiará durante la evaluación. Esto nos da una visión profunda del código de bytes (IR) resultante, lo que permite un análisis estático potente y fiable y la integración IDE que puede ser difícil de lograr con lenguajes más dinámicos.

En general, tiene más tranquilidad de que los errores se detectarán antes al escalar programas Nushell.
:::

## El REPL de Nushell

Como en la mayoría de los shells, Nushell tiene un bucle de _"Leer→Evaluar→Imprimir"_ (Read→Eval→Print Loop) ([REPL](https://es.wikipedia.org/wiki/REPL)) que se inicia cuando ejecutas `nu` sin especificar un archivo. A menudo se piensa en esto como el _"prompt"_ de línea de comandos.

::: tip Nota
En esta sección, el carácter `> ` al inicio de una línea en un bloque de código representa el **_prompt_** del shell. Por ejemplo:

```nu
> algún código...
```

El código después del prompt se ejecuta presionando la tecla <kbd>Enter</kbd>. Por ejemplo:

```nu
> print "Hello world!"
# => Hello world!

> ls
# => muestra archivos y directorios...
```

Esto significa:

- Desde dentro de Nushell (iniciado con `nu`):
  1. Escribe `print "Hello world!"`
  1. Presiona <kbd>Enter</kbd>.
  1. Nushell mostrará el resultado.
  1. Escribe `ls`
  1. Presiona <kbd>Enter</kbd>.
  1. Nushell mostrará el resultado.

:::

Cuando presionas <kbd>Enter</kbd> después de escribir un comando, Nushell:

1. **_(Leer -Read-):_** Lee la entrada de la línea de comandos.
1. **_(Evaluar -Evaluate-):_** Analiza (Parse) la entrada de la línea de comandos. 
1. **_(Evaluar -Evaluate-):_** Evalúa la entrada de la línea de comandos.
1. **_(Evaluar -Evaluate-):_** Fusiona el entorno -environment- (como el directorio actual) con el estado interno de Nushell.
1. **_(Imprimir -Print-):_** Muestra los resultados (si no son `null`).
1. **_(Bucle -Loop-):_** Espera otra entrada.

En otras palabras, cada invocación del REPL es su propia secuencia independiente de análisis-evaluación (parse-evaluation). Al fusionar el entorno nuevamente en el estado de Nushell, se mantiene la continuidad entre las invocaciones del REPL.

Compare a simplified version of the [`cd` example](./thinking_in_nu.md#example-change-to-a-different-directory-cd-and-source-a-file) from _"Thinking in Nu"_:

```nu
cd spam
source-env foo.nu
```

Vimos que esto no puede funcionar (como script u otra expresión única) porque el cambio de directorio ocurre _después_ de que la palabra clave [`source-env`](/commands/docs/source-env.md) intente leer el archivo en tiempo de análisis (parse-time).

Sin embargo, ejecutar estos comandos como entradas separadas en el REPL sí funciona:

```nu
> cd spam
> source-env foo.nu
# Yay, ¡Funciona!
```

Para entender por qué, analicemos qué sucede en el ejemplo:

1. Lee el comando `cd spam`.
2. Analiza (parse) el comando `cd spam`.
3. Evalúa el comando `cd spam`.
4. Fusiona (merge) el entorno (incluido el directorio actual) en el estado de Nushell.
5. Lee y analiza (parse) `source-env foo.nu`.
6. Evalúa `source-env foo.nu`.
7. Fusiona (merge) el entorno (incluidos los cambios realizados por `foo.nu`) en el estado de Nushell.

Cuando `source-env` intenta abrir `foo.nu` durante el análisis (parsing) en el Paso 5, puede hacerlo porque el cambio de directorio del Paso 3 se fusionó con el estado de Nushell en el Paso 4. Por lo tanto, es visible en los siguientes ciclos de Análisis/Evaluación (Parse/Eval).

### Líneas de comando multilínea en el REPL (Multiline REPL Commandlines)

Ten en cuenta que esto solo funciona para líneas de comando **_separadas_**.

En Nushell, es posible agrupar múltiples comandos en una sola línea de comando usando:

- Un punto y coma:

  ```nu
  cd spam; source-env foo.nu
  ```

- Una nueva línea:

  ```
  > cd span
    source-env foo.nu
  ```

  Nota que no hay un "prompt" antes de la segunda línea. Este tipo de línea de comando multilínea generalmente se crea con una [asignación de teclas (keybinding)](./line_editor.md#keybindings) para insertar una nueva línea al presionar <kbd>Alt</kbd>+<kbd>Enter</kbd> o <kbd>Shift</kbd>+<kbd>Enter</kbd>.

Ambos ejemplos se comportan exactamente igual en el REPL de Nushell. Toda la línea de comando (ambas instrucciones) se procesan en un solo ciclo de _Leer→Evaluar→Imprimir_ (Read→Eval→Print Loop). Por lo tanto, fallarán de la misma manera que el ejemplo anterior con el script.


::: tip
Las líneas de comando multilínea son muy útiles en Nushell, pero presta atención a palabras clave del Parser que estén fuera de orden.
:::

## Evaluación constante en tiempo de análisis (Parse-time Constant Evaluation)

Si bien es imposible añadir análisis (parsing) en la etapa de evaluación y aún mantener los beneficios de un lenguaje estático, es seguro añadir _un poco_ de evaluación al análisis.

::: tip Terminología
In the text below, we use the term _"constant"_ to refer to:
En el texto siguiente, usamos el término _"constante"_ (constant) para referirnos a:

- Una definición `const`.
- El resultado de cualquier comando que produzca un valor constante cuando se le proporcionan entradas constantes.
  :::

Por su naturaleza, las **_constantes_** y sus valores son conocidos en tiempo de análisis (parse-time). Esto contrasta fuertemente con las declaraciones y valores de _variables_.

Como resultado, podemos utilizar constantes como argumentos seguros y conocidos para palabras clave del tiempo de analisis (parse-time keywords), como [`source`](/commands/docs/source.md), [`use`](/commands/docs/use.md) y comandos relacionados.

Considera [este ejemplo](./thinking_in_nu.md#example-dynamically-creating-a-filename-to-be-sourced) de _"Thinking in Nu"_:

```nu
let my_path = "~/nushell-files"
source $"($my_path)/common.nu"
```

Sin embargo, podemos hacer lo siguiente en su lugar:

```nu:line-numbers
const my_path = "~/nushell-files"
source $"($my_path)/common.nu"
```

Analicemos el proceso de análisis y evaluación (Parse/Eval) para esta versión:

1. Todo el programa se analiza (Parse) en IR:

   1. Línea 1: La definición de `const` se analiza (parsed). Como es una asignación constante (y `const` también es una palabra clave del parser), esa asignación también se evalúa en esta etapa. Su nombre y valor se almacenan en el Parser.   
   2. Línea 2: El comando `source` se analiza (parsed). Dado que `source` es también una palabra clave del Parser, se evalúa en esta etapa. En este ejemplo, se puede analizar (parsed) correctamente ya que su argumento es **_conocido_** y puede recuperarse en este punto.
   3. El código fuente de `~/nushell-files/common.nu` se analiza (parsed). Si es inválido, se generará un error; de lo contrario, los resultados de IR se incluirán en la evaluación en la siguiente etapa.

2. Todo el IR se evalúa:
   1. Línea 1: La definición de `const` se evalúa. La variable se agrega a la pila en tiempo de ejecución (runtime stack).
   2. Línea 2: El resultado de IR del análisis (parsing) de `~/nushell-files/common.nu` se evalúa (Evaluated).

::: important Importante

- Un `eval` añade análisis adicional (additional parsing) durante la evaluación.
- Las constantes en tiempo de análisis (Parse-time) hacen lo opuesto: añaden evaluación adicional al parser.
  :::

Ten en cuenta que la evaluación permitida durante el análisis es **_muy restringida_**. Está limitada a un pequeño subconjunto de lo permitido durante una evaluación normal.

Por ejemplo, lo siguiente no está permitido:

```nu
const foo_contents = (open foo.nu)
```

Put differently, only a small subset of commands and expressions can generate a constant value. For a command to be allowed:
En otras palabras, solo un pequeño subconjunto de comandos y expresiones puede generar un valor constante. Para que un comando sea permitido:

- Debe estar diseñado para producir un valor constante.
- Todas sus entradas también deben ser valores constantes, literales o tipos compuestos (como registros (records), listas o tablas) de literales.

En general, los comandos y expresiones resultantes serán bastante simples y **_sin efectos secundarios_**(side effects). De lo contrario, el parser podría entrar fácilmente en un estado irrecuperable. Imagina, por ejemplo, intentar asignar un flujo infinito a una constante. ¡La etapa de análisis (Parse stage) nunca terminaría!

::: tip
Puedes ver qué comandos de Nushell pueden devolver valores constantes usando:

```nu
help commands | where is_const
```

:::

Por ejemplo, el comando `path join` puede producir un valor constante. Nushell también define varias rutas útiles en el registro (record) constante `$nu`. Estas pueden combinarse para crear evaluaciones constantes en tiempo de análisis (parse-time) como:

```nu
const my_startup_modules =  $nu.default-config-dir | path join "my-mods"
use $"($my_startup_modules)/my-utils.nu"
```

::: note Notas adicionales
Los lenguajes compilados ("estáticos") también suelen tener formas de transmitir cierta lógica en tiempo de compilación. Por ejemplo:

- El preprocesador de C
- Macros de Rust
- [El `comptime` de Zig](https://kristoff.it/blog/what-is-zig-comptime),  sirvió de inspiración para la evaluación de constantes en tiempo de análisis sintáctico (parte-time) de Nushell.

Existen dos razones para esto:

1. _Aumentar el rendimiento en tiempo de ejecución (Runtime):_ La lógica en la etapa de compilación no necesita repetirse durante la ejecución.

   Esto actualmente no aplica a Nushell, ya que los resultados analizados (IR) no se almacenan más allá de la Evaluación. Sin embargo, esto se ha considerado como una posible función futura.

2. Al igual que con las evaluaciones constantes en tiempo de análisis (parse-time) de Nushell, estas características ayudan (de manera segura -safely-) a superar limitaciones causadas por la ausencia de una función `eval`.
   :::

## Conclusión

Nushell opera en el espacio de los lenguajes de scripting típicamente dominados por lenguajes _"dinámicos"_ e _"interpretados"_, como Python, Bash, Zsh, Fish y muchos otros. Nushell también es _"interpretado"_ porque el código se ejecuta inmediatamente (sin una compilación manual separada).

Sin embargo, no es _"dinámico"_ porque no tiene una construcción `eval` (eval construct). En este sentido, comparte más similitudes con lenguajes _"estáticos"_ y compilados como Rust o Zig.

Esta falta de `eval` suele sorprender a muchos usuarios nuevos, y por ello puede ser útil pensar en Nushell como un lenguaje compilado y estático.