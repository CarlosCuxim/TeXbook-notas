# Los caracteres que escribes

$\TeX$ puede manejar 256 caracteres. Algunos cumplen un papel especial en el programa por lo que para acceder a ellos es necesario usar comandos. Uno de estos comandos es `\char`, la sintaxis es la siguiente:
> [!abstract] Sintaxis
> `\char`⟨_number_⟩

donde Des cualquier número de 0 hasta 255. Esto devolverá el carácter correspondiente a ese número. Por ejemplo `\char98` devolverá `b`.

> [!nota]
> La forma en la que $\TeX$ identifica los caracteres es mediante el código [[ASCII]]. 

---
$\TeX$ tiene distintas formas de trabajar con parámetros de tipo número ⟨_number_⟩, una forma es mediante la notación decimal, sin embargo también puede manejar números en notación octal o hexadecimal.

Para la notación octal basta con anteceder el número con el carácter `'`, mientras que para la notación hexadecimal basta con anteceder el número con el carácter `"` (los dígitos de tipo letra deben estar en mayúsculas).

De este modo `\char'37` y `\char"1F` tendrán el mismo significado que `\char31`.

Otra forma de expresar ⟨_number_⟩ es mediante los mismos caracteres. Si el token `` ` ``$_{12}$ precede a un carácter o un comando de un solo carácter, este expresará el código de ese carácter. Por ejemplo ``\char`b`` y ``\char`\b`` tendrá el mismo significa que `\char98`.

>[!nota]
> La importancia de la segunda notación es para los caracteres que tienen un significado especial, como `%`.

---
Aunque se puede definir comandos que devuelvan un carácter específico mediante el comando `\def`, $\TeX$ tiene un comando específico para esta función. Este es el comando `\chardef`, la sintaxis es la siguiente:
> [!abstract] Sintaxis
> `\chardef`⟨_control sequence_⟩`=`⟨_number_⟩

Por ejemplo, el comando `\%` está definido como:
```tex
\chardef\%=`\%
```

---
Un problema con el comando `\char` es que no permite usarse en secuencias de control (ni siquiera con `\csname` y `\endcsname`). Sin embargo $\TeX$ ofrece una manera de solventar esto.

Si la cadena de caracteres `^^` es seguido de un carácter de código $n$, entre 0 y 127, entonces este será equivalente a escribir el carácter de código $n+64$ o $n-64$, en el caso que $n \leq 64$ o $n-64$, respectivamente.

Por ejemplo, dado que los caracteres `0` y `p` tienen códigos 48 y 112, respectivamente, entonces `^^0` es equivalente a `p` y `^^p` es equivalente a `0`. Esto es útil cuando se necesitan caracteres que no están en el teclado o en nombre de comandos. Por ejemplo
```tex
\^^0
```
es equivalente a `\p`.

La notación anterior permite obtener caracteres de código 0 a 127, sin embargo también existe una manera de obtener el resto de caracteres. Si `^^` es seguido de un par de dígitos hexadecimales (en minúscula), entonces esto representará el carácter del código respectivo. Por ejemplo `^^6e` es equivalente a `n`.

---
## Estados de lectura

$\TeX$ tiene unas reglas muy específicas en como convierte los caracteres que se escriben en tokens. Estas reglas determinan que varios espacios en blanco se combinen en uno solo, que se omitan después de un comando o para aplicar la notación `^^`.

En primer lugar, $\TeX$ lee el texto de un archivo o la entrada desde la terminal como una sucesión de líneas. En el caso de un archivo, las líneas son obtenidas al usar el carácter `⏎` como separador.

De igual forma, en cada momento, $\TeX$ estará en alguno de los siguientes estados de lectura:
- **Estado $N$:** inicio de línea
- **Estado $M$:** mitad de línea
- **Estado $S$:** saltado de espacios.

Al inicio de cada línea $\TeX$ estará en estado $N$, sin embargo la mayor parte de tiempo estará en estado $M$ y después de un espacio o palabra de control estará en estado $S$. Estos estados determinarán el comportamiento de algunos caracteres como `␣` o `⏎`.

En primer lugar, $\TeX$ eliminará todos los caracteres de espacios en blanco (`␣`) que estén al final de una línea. De igual forma agregará un carácter de salto de línea (`⏎`) al final de cada línea, a excepción de las líneas insertadas con `I` en el modo de corrección de errores.

>[!nota]
>Dado que el carácter `⏎` será parte de la línea, se puede conseguir efectos interesantes al modificar su comportamiento.

El carácter que se agrega al final de cada línea no necesariamente debe ser `⏎`. Este carácter está definido por el valor `\endlinechar` que normalmente está inicializado como
```tex
\endlinechar=13
```
Si el valor de `\endlinechar` es negativo o mayor a 255, ningún carácter será agregado y su efecto será similar a como si todas las líneas terminaran con `%` (i.e., con un carácter de comentario).  

Ahora, si $\TeX$ se encuentra con un carácter de escape (categoría 0) en cualquier estado, se escaneará el nombre del comando de la siguiente manera
1. Si no hay más caracteres en la línea, el nombre estará vacío (como en `\csname\endcsname`).
2. Si el siguiente carácter no es una letra (categoría 11), el nombre será únicamente ese carácter.
3. Si el siguiente carácter es una letra, el nombre consistirá en todas las letras consecutivas, empezando con la actual y terminando hasta justo antes del primer carácter no letra o hasta el final de la línea.

Este nombre se convertirá en un token de secuencia de comando y se cambiará al estado $S$ en el caso 3 y el 2 si el nombre consiste en el carácter de espacio (categoría 10). De otra forma, $\TeX$ cambiará al estado $M$.

>[!nota]
>De modo normal es imposible obtener una secuencia de control cuyo nombre sea vacío ya que $\TeX$ agrega un carácter `⏎` al final de cada línea de un archivo o línea agregada desde la terminal. Sin embargo técnicamente es posible mediante las líneas insertadas con `I` en el modo de corrección de errores.

Si $\TeX$ se encuentra con un carácter de superíndice (categoría 7) en cualquier estado y es seguido de otro carácter de superíndice y un carácter de código $c<128$, entonces el trio de caracteres es eliminado y en su lugar es colocado el carácter de código $c\pm64$, dependiendo de si es mayor o menor a 64. Sin embargo, si el carácter de superíndice es seguido de otro carácter de superíndice y dos dígitos hexadecimales en minúscula (`0`, …,`9` y `a`, …, `f`), entonces el cuarteto de caracteres es eliminado y en su lugar es colocado el carácter cuyo código es el mismo que el número hexadecimal dado por los dos dígitos. Este reemplazo también es aplicado si son encontrados en los pasos 2 y 3 del escaneo de nombre de las secuencias de control descrita anteriormente. Después del reemplazo, $\TeX$ comienza de otra vez como si el nuevo carácter hubiese estado ahí todo el tiempo. Si un carácter de superíndice no es del trio o cuarteto anterior, entonces se aplican las reglas posteriores.

Si un carácter es de categoría 1, 2, 3, 4, 6, 8, 11, 12 o 13, o es un carácter de categoría 7 que no es el principio de una secuencia especial de reemplazo descrita anteriormente, se convierte ese carácter en un token, al adjuntarle su respectivo código de categoría, y se cambiará al estado $M$.
>[!nota]
>Esta regla es la que se aplica en casi todos los casos. Casi todo carácter que no es un espacio es manejada por esta regla.

Si $\TeX$ se encuentra con un carácter de fin de línea (categoría 5), se descartará cualquier información que podría quedar en esa línea. Entonces:
- Si $\TeX$ está en estado $N$, el carácter de fin de línea será convertido en el token de control de secuencia [`par`].
- Si $\TeX$ está en estado $M$ el carácter de fin de línea será convertido en un token `␣`$_{10}$ (de código 32 y categoría 10).
- Si $\TeX$ está en estado $S$, el carácter de fin de línea será simplemente descartado.

Si $\TeX$ se encuentra con un carácter ignorado (categoría 9), simplemente pasará por alto ese carácter como si no estuviera ahí y permanecerá en el mismo estado.

Si $\TeX$ ve un carácter de espacio (categoría 10), la acción dependerá del estado actual:
- Si $\TeX$ está en estado $N$ o $S$, el carácter será simplemente ignorado y $\TeX$ permanecerá en el mismo estado.
- Si $\TeX$ está en estado $M$, el carácter será convertido en un token `␣`$_{10}$ (de código 32 y categoría 10) y $\TeX$ entrará en estado $S$.
>[!nota]
>El código de carácter de un token de espacio siempre será 32, es decir, para $\TeX$, el único espacio en blanco es token `␣`$_{10}$.

Si $\TeX$ ve un carácter de comentario (categoría 14), entonces desecha el carácter y cualquier otra información que podría quedar en la línea actual.

Finalmente, si $\TeX$ ve un carácter inválido (categoría 15), pasará por alto ese carácter, imprimirá un mensaje de error en la terminal y permanecerá en el mismo estado.

Si $\TeX$ ya no tiene nada más que leer en la línea actual, pasará a la siguiente línea y entrará en estado $N$. Sin embargo, si `\endinput` ha sido especificado en un archivo agregado por `\input` o si un archivo agregado por `\input` ha finalizado, $\TeX$ regresará a lo que sea que haya estado leyendo cuando el comando `\input` fue originalmente dado. 
