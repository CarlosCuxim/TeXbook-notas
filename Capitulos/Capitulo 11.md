# Cajas

La manera en la que $\TeX$ crea documentos es mediante una estructura llamada “caja”. Estos son objetos bidimensionales con forma rectangular y tienen tres medidas asociadas: *altura*, *anchura* y *profundidad*.

>[!example] Imagen
> ![[box-info.svg|400]]

Todos los caracteres de una fuente son cajas con sus medidas preestablecidas por la fuente. Sin embargo, la forma de la fuente y su caja podría no coincidir. La corrección itálica (que también es dada por la fuente) indica cuanto más el carácter se pasa de su caja.
>[!example] Imagen
> ![[g-comp.svg|400]]

Otro tipo de caja que $\TeX$ maneja son las “cajas negras”, que son rectángulos completamente rellenados como $\blacksquare$, aunque usualmente vienen en forma de rectas verticales u horizontales. En este tipo de cajas se puede controlar todas sus dimensiones y se acceden mediante los comandos `\hrule` y `\vrule`.

$\TeX$ puede pegar las cajas de manera horizontal o vertical, en ambos casos se alinean usando el punto de referencia. Por ejemplo el comando `\hbox` permite crear cajas compuesta de cajas alineadas horizontalmente, como en:
```
\hbox{A line of type}
```
Análogamente para `\vbox` que crea cajas verticales.

>[!nota]
>Las cajas creadas con `\hbox` y `\vbox` son irrompibles, es decir, no se pueden separar por el guionado.

Las medidas de anchura, altura y profundidad pueden ser negativas, esto sirve para crear espacios negativos. También es posible elevar o bajar las cajas mediante los comando `\raise` y `\lower`. Por ejemplo el logo de $\TeX$ está definido como:
```tex
\def\TeX{T\kern-.1667em \lower.5ex\hbox{E}\kern-.125em X}
```

---
Es posible guardar cajas en registros. Esto se hace mediante el comando `\setbox`⟨_number_⟩. Por ejemplo, para crear un registro que guarde el logo de $\TeX$ basta con escriibir:
```tex
\setbox0=\hbox{T\kern-.1667em \lower.5ex\hbox{E}\kern-.125em X}
```
Esto permitirá acceder a la caja mediante el comando `\box`⟨_number_⟩. También es posible mostrar la información de la caja en el `.log` mediante el comando `\showbox`⟨_number_⟩. Por ejemplo la información de la caja anterior es la siguiente:
```tex
> \box0=
\hbox(6.83331+2.15277)x18.6108
.\tenrm T
.\kern -1.66702
.\hbox(6.83331+0.0)x6.80557, shifted 2.15277
..\tenrm E
.\kern -1.25
.\tenrm X
```
Los valores de la primera fila son, respectivamente, la altura, profundidad y anchura de la caja guardada. El resto de la información muestra el contenido de la caja.