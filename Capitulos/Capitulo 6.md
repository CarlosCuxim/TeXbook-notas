# Corriendo $\TeX$

Para correr $\TeX$ basta con correr el comando `tex` en la consola de comandos. Esto mostrará algo similar al siguiente texto:
```tex
This is TeX, Version 3.141 (preloaded format=plain 89.7.15)
** 
```

En este estado se puede introducir
1. El nombre de un archivo `.tex` para que sea ejecutado
2. Un secuencia de control

El programa no finalizará hasta que se ejecute el comando `\end`.  Después de esta primera acción, el programa cambiará al modo de entrada con un solo asterisco, en este estado se puede escribir como si se estuviera en un archivo `.tex`. 

Después de ejecutar el comando `\end` la salida será un documento `.dvi`, este puede ser abierto con algún software especializado como `xdvi`.

> [!nota]
> Si se desea un archivo `.pdf` se puede usar algun programa como `dvipdfm` o algún otro “engine” como pdfTeX o LuaTeX.

Otra forma de ejecutar el programa es escribiendo el nombre del archivo después del comando `tex` en la consola de comandos, por ejemplo: `tex file`. Si el archivo contiene el comando `\end` automáticamente se obtendrá el archivo `.dvi`, en caso contrario se entrará al modo de entrada con un solo asterisco.

---
En $\TeX$ se puede crear comandos mediante el carácter `%`. Todo lo que esté después de `%` hasta el final de la línea se omitirá.

---
El comando `\hsize` guarda el tamaño de ancho de columna. De este modo escribiendo
```tex
\hsize=4in
```
hace que el ancho de columna sea de 4 pulgadas.

---
$\TeX$ automáticamente rompe las líneas automáticamente para obtener el mejor resultado posible. Sin embargo en algunos casos $\TeX$ no puede encontrar una forma de romper las líneas, por lo que ocurren las alertas _overfull boxes_ y _underfull boxes_.

Las _overfull boxes_ ocurren cuando el texto de una línea supera el tamaño de columna, ya sea por que está conformado por palabras muy grandes o por que no se pudo encontrar una separación de guiones apropiada. El error suele aparecer en el `.log` como sigue:
```tex
Overful \hbox (2pt too wide) in paragraph at lines xx--xx
<Texto donde está el error>
```
El tamaño dentro de los paréntesis muestra el exceso del tamaño de la línea con respecto al del ancho de columna.

Una forma de eliminar este error es modificar el valor `\tolerance`, el cual indica que tanto se puede estirar los espacios de las palabras. Por default este valor está inicializado como
```tex
\tolerance=200
```
donde el valor de 200 indica el mayor “badness” tolerado.  Este valor “badness” es un número calculado en cada fila que indica que tan estirados están los espacios de esa fila.

Otra forma de eliminar el error es aplicar una alineación derecha mediante el comando `\raggedright`.  En este estado, el badness refleja que tan grande es el espacio con respecto al margen derecho.

Finalmente, tenemos el tamaño `\hfuzz` el cual indica la tolerancia para el exceso de tamaño de la fila. Por default está inicializado como
```tex
\hfuzz=0.1pt
```
Es decir, si la fila se excede menos de 0.1pt entonces el error _overfull box_ no aparecerá.

Las _underfull boxes_ ocurren cuando el badness de esa línea supera un valor específico. El error suele aparecer en el `.log` como sigue:
```tex
Underfull \hbox (badness 10000) in paragraph at lines xx--xx
<Texto donde está el error>
```
El número dentro de la línea muestra el badness de esa fila.

Una forma de eliminar este error es modificar el valor `\hbadness`, el cual indica el mínimo badness para el cual se muestra el error. Por default este valor está inicializado como
```tex
\hbadness=1000
```

---
Finalmente, cuando ocurre un error, un mensaje como el siguiente puede aparecer en la terminal o el `.log`:
```tex
! <Tipo de error>
l.x <Parte antes del error>
		<Parte despues del error>
?
```

Existen varios tipos de errores como `! Undefined control sequence` que aparece cuando se coloca un comando sin definir (o un comando que fue escrito incorrectamente).

Después de la descripción del error podemos hacer ciertas acciones para arreglarlo o ignorarlo. Por ejemplo, si escribimos `?` y después presionamos `⏎` tendremos un mensaje como el siguiente:
```tex
Type <return> to proceed, S to scroll future error messages,
R to run without stopping, Q to run quietly,
I to insert something, E to edit your file,
1 or ... or 9 to ignore the next 1 to 9 tokens of input,
H for help, X to quit.
?
```
El resto de opciones y sus efectos son los siguientes:
- `⏎`: $\TeX$ reanudará el proceso, después de recuperarse del error lo mejor que pueda.
- `S`: $\TeX$ reanudará el proceso sin pausar por instrucciones si un error nuevo aparece. Sin embargo los errores aparecerán en la terminal y se escribirán en el `.log`. Es similar a presionar `⏎` al resto de mensajes.
- `R`: Es como `S` pero más fuerte, ya que no parará si el nombre de un archivo no es encontrado.
- `Q`: Es como `R` pero omite los mensajes en la terminal.
- `I`⟨_texto_⟩: Inserta ⟨_texto_⟩ antes de la parte después del error. Se asume que ⟨_texto_⟩ no termina con espacios en blanco.
- ⟨$n=1,\ldots,99$⟩: Se ignorarán los siguientes $n$ caracteres (o comandos) y pausará de nuevo para dar otro vistazo.
- `H`: Describe el error de una manera más informal.
- `X`: Terminará el compilado, se agregarán los detalles al `.log` y las páginas que ya hayan sido procesadas al `.dvi`.
- `E`: Es como `X`, sin embargo, también abrirá un editor de texto para corregir los errores.

También existe algunos comandos que permiten cambiar la interacción del programa, como `\scrollmode`, `\nonstopmode`, `\batchmode` que corresponden a escribir `S`, `R` o `Q`, respectivamente, `\errorstopmode` regresa al tipo de interacción normal.

Algunos errores pueden ser largos, el comando `\errorcontextlines` indica el número de líneas máximo que un error puede contener. Por defecto está inicializado como
```tex
\errocontextlines=5
```