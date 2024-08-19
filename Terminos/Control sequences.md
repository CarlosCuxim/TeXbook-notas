# Secuencias de control

Una secuencia de control es un tipo de [[Token|token]] que no es un carácter. Usualmente sirven para guardar macros y comandos.

Los diferentes tipos de secuencias de control son:
- [[Primitive|Primitivas]]
- [[Macro|Macros]]

# Definición de las secuencias de control

En esta sección revisaremos todas las definiciones de las secuencias de control básicas que se pueden usar en $\TeX$. El orden es el mismo de aparición en el [[TeXbook]].

## \\lq

Sirve como sustituto del carácter `` ` ``.

El comando `\lq` está definido como:
```tex
\def\lq{`}
```

Usando `\show` aparece como:
```tex
> \lq=macro:
->`.
```

## \\rq

Sirve como sustituto del carácter ``'``.

El comando `\rq` está definido como:
```tex
\def\rq{'}
```

Usando `\show` aparece como:
```tex
> \rq=macro:
->'.
```

## \\thinspace

Sirve para introducir un espacio pequeño.

El comando `\thinspace` está definido como:
```tex
\def\thinspace{\kern .16667em }
```

Usando `\show` aparece como:
```tex
> \thinspace=macro:
->\kern .16667em .
```

## \\input

Sirve para leer y agregar un archivo de texto plano. Usualmente se usar para mover partes de texto a un archivo externo.

El comando `\input` es una [[Primitive|primitiva]].

Usando `\show` aparece como:
```tex
> \input=\input.
```

## \\'

Sirve para agregar el acento agudo (ó) a la siguiente letra.

El comando `\'` está definido como:
```tex
\def\'#1{{\accent"13 #1}}
```

Usando `\show` aparece como:
```tex
> \'=macro:
#1->{\accent 19 #1}.
```

## \\"

Sirve para agregar la diéresis (ö) a la siguiente letra.

El comando `\"` está definido como:
```tex
\def\"#1{{\accent"7F #1}}
```

Usando `\show` aparece como:
```tex
> \"=macro:
#1->{\accent "7F #1}.
```

## \\␣

Sirve para agregar un espacio en blanco. Usualmente se usará en los modos matemáticos o después de una palabra de escape.

El comando `\␣` es una [[Primitive|primitiva]].

Usando `\show` aparece como:
```tex
> \ =\ .
```

## \\⏎

Sirve como alias de `\␣`, ya que en el texto son indistinguibles.

El comando `\⏎` está definido como:
```tex
\def\^^M{\ }
```

Usando `\show` aparece como:
```tex
> \^^M=macro:
->\ .
```

## \\↹

Sirve como alias de `\␣`, ya que en el texto son indistinguibles.

El comando `\↹` está definido como:
```tex
\def\^^I{\ }
```

Usando `\show` aparece como:
```tex
> \^^I=macro:
->\ .
```

# \\TeX

Sirve para imprimir el logo de $\TeX$.

El comando `\TeX` está definido como:
```tex
\def\TeX{T\kern-.1667em \lower.5ex\hbox{E}\kern-.125em X}
```

Usando `\show` aparece como:
```tex
> \TeX=macro:
->T\kern -.1667em\lower .5ex\hbox {E}\kern -.125emX.
```

