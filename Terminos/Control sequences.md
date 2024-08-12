# Secuencias de control

Una secuencia de control es un tipo de [[Token]] que no es un carácter. Usualmente sirven para guardar macros y comandos.

Los diferentes tipos de secuencias de control son:
- Primitivas
- Macros

# Definición de las secuencias de control

En esta sección revisaremos todas las definiciones de las secuencias de control básicas que se pueden usar en $\TeX$. El orden es el mismo de aparición en el [[TeXbook]].

## \\lq

Sirve como sustituto del carácter `` ` ``.

El comando `\lq` es un [[macro]] y está definido como:
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

El comando `\rq` es un [[macro]] y está definido como:
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

El comando `\thinspace` es un [[macro]] y está definido como:
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

El comando `\input` es una [[primitiva]].

Usando `\show` aparece como:
```tex
> \input=\input.
```
