# Cómo $\TeX$ lee lo que escribes

$\TeX$ puede leer exactamente 256 caracteres y este le asigna un valor numérico llamado [[Catcode|código de categoría]] que indica la función del carácter en el programa.

Estos valores, sus significados y los caracteres asociados por default a estos valores son los siguientes: 

| **Categoría** | **Significado**        | **Caracteres**                         |
| :-----------: | ---------------------- | -------------------------------------- |
|       0       | Carácter de escape     | `\`                                    |
|       1       | Inicio de grupo        | `{`                                    |
|       2       | Final de grupo         | `}`                                    |
|       3       | Cambio matemático      | `$`                                    |
|       4       | Alineación de tabla    | `&`                                    |
|       5       | Final de línea         | `⏎`                                    |
|       6       | Parámetro              | `#`                                    |
|       7       | Superíndice            | `^`                                    |
|       8       | Subíndice              | `_`                                    |
|       9       | Carácter ignorado      | ⟨null⟩                                 |
|      10       | Espacio                | `␣`                                    |
|      11       | Letra                  | `A`, …,`Z` y `a`, …,`z`                |
|      12       | Otro carácter          | Ninguno de los anteriores o siguientes |
|      13       | Carácter activo        | `~`                                    |
|      14       | Carácter de comentario | `%`                                    |
|      15       | Carácter inválido      | ⟨delete⟩                               |

$\TeX$ usa estos códigos para indicar que algunos caracteres tienen significados específicos, para usarlos en el texto normal hay que usar comandos especiales:
- `\$` para $
- `\%` para %
- `\&` para &
- `\#` para #
- `\_` para \_
- `$\{$` para $\{$
- `$\}$` para $\}$
- `$\backslash$` para $\backslash$
Unos casos especiales son `\^` y `\~` que indican los acentos circunflejo y la tilde de la ñ. Por ejemplo `\^o` se verá como ô y `\~o` se verá como õ. Para obtener los símbolos solo hay que agregar un grupo vacío: `\^{}` y `\~{}`.

---
La manera exacta en como $\TeX$ lee las líneas es mediante los [[Token|tokens]]. Cada línea es convertida en una lista de tokens y estos son alguno de los siguientes:
- Un carácter unido a un código de categoría
- Una secuencia de comando

Por ejemplo, el texto `{\hskip 36pt}` es convertido en la siguiente lista de tokens: 
>[!ejemplo]
>`{`$_1$  &emsp; [`hskip`] &emsp; `3`$_{12}$ &emsp; `6`$_{12}$ &emsp; `␣`$_{10}$ &emsp; `p`$_{11}$ &emsp; `t`$_{12}$ &emsp; `}`$_2$

El token [`hskip`] no tiene un código de categoría ya que es un comando y no un carácter. Además el espacio después de `\hskip` no entra en la lista de tokens ya que los espacios después de los comandos son omitidos.

Una vez un texto se ha convertido en una lista de token, esta es inmutable, por ejemplo, los códigos de categoría ya no se pueden modificar y  los espacios consecutivos o después de un comando no son combinados o eliminados.

---
Existe un programa más básico que $\TeX$ llamado `INITEX`, este sirve para crear instalaciones de $\TeX$ desde cero, tablas de guiones para guionar las palabras de manera más rápida o archivos de formato como el usado para plain $\TeX$. A cambio, este programa tiene menos memoria que utilizar, por lo que no es óptimo para generar documentos.

Cuando `INITEX` inicia solo conoce las primitivas de $\TeX$, todos los 256 caracteres estás inicializados con código de categoría 12 a excepción de:
- `\` de categoría 0
- `⏎` de categoría 5
- ⟨null⟩ de categoría 9
- `␣` de categoría 10
- `A`, …,`Z` y `a`, …,`z` de categoría 11
- `%` de categoría 14
- ⟨delete⟩ de categoría 15

Esto significa que algunos comandos que dependen de los símbolos de agrupamiento como `\def` o `\hbox` no pueden funcionar. De este modo, los archivos de formato usualmente empiezan con definiciones como las siguientes:
```tex
\catcode`\{=1
\catcode`\}=2
```
El comando `\catcode` es una primitiva que permite cambiar el código de categoría a cualquiera de los 256 caracteres. De este modo código anterior simplemente está indicando que los caracteres `{` y `}` tendrán un código de categoría 1 y 2, respectivamente.

---
Aunque los comandos son tratados como un solo objeto, mediante el comando `\string` es posible romper un comando en su cadena de texto que la compone. De este modo si escribes
```tex
\string\contseq
```
obtendrás la lista de tokens: `\`$_{12}$, `c`$_{12}$, `o`$_{12}$, `n`$_{12}$, `t`$_{12}$, `s`$_{12}$, `e`$_{12}$, `q`$_{12}$.

Siempre que el comando `\string` es usado, este devolverá una lista de tokens de categoría 12 a excepción del espacio (`␣`) que tendrá categoría 10.

También, el carácter usado para denotar el carácter de control puede ser modificado mediante el valor `\escapechar`, por default este está inicializado como
```tex
\escapechar=92
```
que es el el código [[ASCII]] para la barra invertida (\\).

---
Otra forma de obtener secuencias de control, además de usar el carácter de escape, es mediante los comandos `\csname` y `\endcsname`. La sintaxis es la siguiente:
>[!sintaxis]
>`\csname`⟨_char tokens_⟩`\endcsme`

donde ⟨_char tokens_⟩ puede contener:
- Caracteres de cualquier categoría
- Comandos que se expanden en caracteres

No hay diferencia entre una secuencia control creada usando el carácter de escape y los comandos `\csname` y `\endcsname`. De este modo el código
```tex
\csname TeX\csname
```
tiene exactamente el mismo  significado que el comando `\TeX`.

Notar que ⟨_char tokens_⟩ se expande antes de crear un token, de este modo el siguiente código
```tex
\csname\string\TeX\endcsname
```
generará el extraño comando [`\TeX`], es decir el comando conformado por los 4 caracteres `\`, `T`, `e`, `X`.
>[!nota]
>El código `\csname\TeX\endcsname` no funcionará, dado que en la expansión de `\TeX` existen algunas primitivas. De esta forma el comando `\string` es necesario ya que, a pesar de que es una primitiva, la combinación `\string\TeX` se expande en una cadena de caracteres.

---
Existen otros comandos que devuelven una lista de tokens. Primero tenemos los comandos `\number` y `\romannumeral` que toman un número y devuelve una lista de tokens de categoría 12 que corresponderá a su notación decimal o en numeración romana.  Su sintaxis es la siguiente
>[!sintaxis]
>`\number`⟨_number_⟩
>`\romannumeral`⟨_number_⟩

donde ⟨_number_⟩ pueden ser una cadena de caracteres de tipo número (`0`, …,`9`), un comando que se expanda en caracteres de tipo número, un contador o un registro.

Por ejemplo, `\number124` y `\romannumeral124` devolverá `124` y `xxiv`, respectivamente. De igual forma si el registro `\count5` tiene guardado el valor 12 entonces `\number\count5` y `\romannumeral\count5` devolverá `12` y `xii`, respectivamente.

Aunque el primer comando pareciera redundante, este elimina los ceros a la izquierda. De este modo `\number-0015` devolverá `-15`.

Otro ejemplo son los comandos gemelos `\uppercase` y `\lowercase` que toman una lista de tokens y las convierte en su forma mayúscula y minúscula, respectivamente. La sintaxis es la siguiente:
>[!sintaxis]
>`\uppercase{`⟨_token list_⟩`}`
>`\lowercase{`⟨_token list_⟩`}`

Por ejemplo, `\uppercase{Hola}` devolverá `HOLA`, mientras que `\lowercase{Hola}` devolverá `hola`.

El funcionamiento es mediante unos códigos, similares a los códigos de categoría, llamados `\ucode` y `\lcode`. Por default todos los códigos `\ucode` y `\lcode` están inicializados en `0` a excepción de las letras `A`, …,`Z` y `a`, …,`z` que tienen su `\lcode` y `\ucode` respectivo.

De esta forma, cuando se usa el comando `\uppercase` (respectivamente `\lowercase`) este cambia tokens los caracteres en ⟨_token list_⟩ cuyo `\ucode` (`\lcode`) es distinto de cero, por el carácter correspondiente a su `\ucode` (`\lcode`) con el mismo código de categoría. Las secuencias de control permanecen inalteradas ya que no tienen `\ucode` o `\lcode`.

Notar que estos comandos aplican sus cambios en distintos momentos de la ejecución. Por ejemplo si `\n` es un comandos que se expande como `20`, entonces el código
```tex
\romannumeral\n0
```
devolverá `cc` en vez de `xx0`, ya que primero se expande `\n` antes de que se aplique `\romannumeral`. Por lo que en realidad se está aplicando `\romannumeral200`.

Si se quiere el primer resultado hay que usar `\romannumeral\n\relax0` para indicar que el `0` que sigue después de `\n` no es parte del número.

Otro ejemplo, si `\cs` se expande como `Text`, entonces `\uppercase{\cs}` devolverá `Text` ya que `\uppercase` se aplica antes de la expansión de `\cs`. Si se quiere que `\cs` se expanda antes, hay que usar el comando `\expandafter`. De este modo, el código
```tex
\uppercase\expandafter{\cs}
```
devolverá `TEXT`.